---
title: "Deployment Location"
url: /developerportal/deploy/deployment-location/
weight: 60
description: "URL considerations for deploying Mendix"
---

## Introduction

When you deploy your app outside Mendix Cloud, you can choose the URL that points to your app. However, there are some restrictions on where you should deploy your app.

{{% alert color="info" %}}
In this document, `domain` is used to identify the domain registered to you through the Internet Corporation for Assigned Names and Numbers (ICANN). This is sometimes referred to as the [apex domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages#using-an-apex-domain-for-your-github-pages-site). This includes the top-level domain. For example, `example.com` would be a `domain` as used in this document.
{{% /alert %}}

For apps deployed to Mendix Cloud, you can customize a URL by adding [custom domains](/developerportal/deploy/custom-domains/).

## Paths

If you specify an app URL location on a (sub)path, the Mendix runtime needs to know the public URL of your application. This can be done by setting the [custom runtime setting](/refguide/custom-settings/#applicationrooturl-section) `ApplicationRootUrl`.

When hosting a Mendix application on a subpath, the proxy needs to forward the request from `https://subdomain.domain/my/sub/path` to the internal address where the Mendix runtime is running. See the snippet below for an example Nginx config.

```
# Location block for the subpath `/my/sub/path`.
location /my/sub/path/ {
    # Make the Mendix runtime aware of https, see documentation below for more information.
    proxy_set_header X-Forwarded-Proto "https";

    # Required for Mendix DevTools to work.
    proxy_http_version 1.1;

    # Proxy the request to the Mendix runtime.
    proxy_pass http://mendix-runtim:8080/;
}
```

{{% alert color="info" %}}
Routing based on a subpath is possible as of Studio Pro 10.3 (for details, see the [ApplicationRootUrl](/refguide/custom-settings/#applicationrooturl-section) section of the *Runtime Customization* page), although it is not supported in Mendix Cloud. For versions below 10.3, it is not possible to use a path to your app. Your app should always be at the root of your subdomain. In other words, it should be at a location like this: `https://subdomain.domain/`.

If you want to deploy several apps on the same domain, use different subdomains to identify the app. For example, use `https://appA.apps.mydomain.com/`, not `https://mydomain.com/apps/appA`.
{{% /alert %}}

## Secure cookies for on-premise applications

The Mendix runtime sets cookies with the `secure` attribute when the application is served over `https` However, in a scenario where the Mendix runtime is served from behind a loadbalancer using `http` for the internal communication, the Mendix runtime needs to be made aware that it is served over `https` to the end-users. This can be done by setting the [ApplicationRootUrl](/refguide/custom-settings/#applicationrooturl-section) Runtime setting to a `https://` link, or by setting the `X-Forwarded-Proto` or `X-Forwarded-Schema` header to `https` in the loadbalancer.

{{% alert color="info" %}}
For Mendix versions prior to Mendix 10.18 setting the [ApplicationRootUrl](/refguide/custom-settings/#applicationrooturl-section) Runtime setting to a `https://` link will not make the application aware of it being served via `https`. For Mendix 10.18 and later, setting the ApplicationRootUrl to a `http://` URL will take precedence over the `X-Forwarded-Proto` and `X-Forwarded-Schema` headers.
{{% /alert %}}


## Main Domain Name

Do not deploy your app directly at the apex domain (`https://domain/`).

This conflicts with the `https://www.domain/` URL because the main domain is often redirected there if there is no subdomain specified.

Also, you would not be able to have additional custom domains for your app because you cannot create a CNAME record that points to an apex domain.

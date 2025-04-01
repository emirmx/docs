---
title: "Ingress Controllers in Mendix for Private Cloud"
linktitle: "Ingress Controllers"
url: /developerportal/deploy/private-cloud-cluster/private-cloud-ingress-settings/controllers/
description: "Describes how to configure various ingress controllers for Mendix for Private Cloud."
weight: 10
---

## Introduction

Ingress is a Kubernetes resource that defines rules for routing external HTTP(S) traffic to services within a cluster. Instead of exposing services individually using LoadBalancers or NodePorts, Ingress provides a centralized way to manage external access efficiently.

In a Mendix environment, the Mendix Operator automatically creates both the Service and Ingress resources based on the app environment's configuration. The Service defines how traffic is routed to application pods within the cluster, while the Ingress manages external access.

However, an Ingress resource alone is just a set of rules—it requires an Ingress Controller (e.g., NGINX) to function. The Ingress Controller continuously monitors Ingress resources and updates the underlying reverse proxy to enforce the specified routing rules.

For each app environment, the URL is automatically generated based on the domain name. For example, if the domain name is set to mendix.example.com, apps will have URLs such as myapp1-dev.mendix.example.com, myapp1-prod.mendix.example.com, and so on.

To ensure proper routing, the DNS server must be configured to direct all subdomains (*.mendix.example.com) to the Ingress Controller or Load Balancer - this option is easy to configure, and adding new apps or changing domain names works instantly. Alternatively, Kubernetes External DNS can be used to manage DNS records.

## Basic Installation and Configuration

NGINX Ingress Controller 

{{% alert color="info" %}}
For Operator version 2.19.0 and Mendix version 10.3.0 onwards, NGINX path based routing is supported. A new option /(.*) in the ingress path is provided which sets the path prefix to support this feature. To support this feature, NGINX Ingress uses nginx.ingress.kubernetes.io/rewrite-target/(.*)
{{% /alert %}}

The NGINX Ingress Controller is an open-source solution that leverages NGINX as a reverse proxy and load balancer to manage Kubernetes Ingress resources. Helm is the recommended way to installing NGINX. You can download it from here

Official procedure to install nginx with manifest is published here and with helm here.

Configuring NGINX in mxpc-cli:

Open image-20250212-142207.png
image-20250212-142207.png
{{< figure src="/attachments/deployment/private-cloud/private-cloud-ingress/nginx-configuration.png" class="no-border" >}}
Select Ingress Type as kubernetes-ingress. kubernetes-ingress will configure ingress according to the additional domain name you supply. 

Ingress Domain Name - provide the domain name which you want to set for the Ingress resource file

Ingress Path -  its optional, which can be used to specify the Ingress path; default value is /

Enable TLS - allows you to enable or disable TLS for the Mendix App’s Ingress

Custom Ingress Class - enable this option for providing the ingress class name.

Ingress Class Name - provide nginx as the ingress class name 

Set Ingress Class as Annotation - Unselect this option. This option adds the legacy kubernetes.io/ingress.class annotation to set the ingress class, instead of using the ingress class name.

AWS Load Balancer Ingress Controller

{{% alert color="info" %}}
AWS Load Balancer Controller is the AWS-recommended way to provide ingress capability on EKS.
{{% /alert %}}

{{% alert color="warning" %}}

To properly configure the AWS Application Load Balancer Controller, manual modifications to the OperatorConfiguration object are necessary.

In addition, some functionality (such as adding or modifying HTTP headers) is not supported by ALB. This can only be done by putting cloud front in front of ALB.
{{% /alert %}}

The AWS Load Balancer Ingress Controller integrates with Amazon’s Application Load Balancer (ALB) or Network Load Balancer (NLB) to provide ingress capabilities. It is designed specifically for AWS EKS but can be configured for any Kubernetes cluster running in AWS. 

AWS Load Balancer Ingress Controller must be deployed on your EKS cluster and at least two subnets in different Availability Zones (more details here). The recommended way to install it is using Helm following official documentation.

Configuring AWS LB in mxpc-cli:

Open image-20250212-145202.png
image-20250212-145202.png
Select Ingress Type as kubernetes-ingress. kubernetes-ingress will configure ingress according to the additional domain name you supply. 

Ingress Domain Name - provide the domain name which was registered for ALB

Ingress Path -  Set it to /*

Enable TLS - keep this option unchecked (TLS is enabled in ALB through annotations).

Custom Ingress Class - enable this option for providing the ingress class name.

Ingress Class Name - provide alb as the ingress class name 

Set Ingress Class as Annotation - Unselect this option. This option adds the legacy kubernetes.io/ingress.class annotation to set the ingress class, instead of using the ingress class name.

{{< figure src="/attachments/deployment/private-cloud/private-cloud-ingress/alb-configuration.png" class="no-border" >}}

Choose one of the following methods to update the Operator configuration, noting that your choice will determine whether the changes affect a specific app environment or all environments within a namespace:

Mendix Platform GUI per app environment:

Navigate to the Global Navigation top bar

Go to Deployment > Private Cloud

Select your Cluster

Choose the appropriate Namespace

In the Apps section, click “Configure App Cog icon”

Kubectl command-line tool at the namespace level, ensuring the changes affect all hosted app environments within that namespace:

Directly edit the OperatorConfiguration object using kubectl

Whichever method you choose, add the following ALB-specific annotations to the Ingress section of your configuration. Note that this is an example and should be customized to your specific requirements:


Open image-20250305-135015.png
image-20250305-135015.png
Mendix Platform GUI


apiVersion: privatecloud.mendix.com/v1alpha1
kind: OperatorConfiguration
# ...
# omitted lines for brevity
# ...
spec:
  # Endpoint (Network) configuration
  endpoint:
    type: ingress
    ingress:
      annotations:
        # Allow access from the public internet
        alb.ingress.kubernetes.io/scheme: internet-facing
        # 'ip' mode will route traffic directly to the pod IP
        alb.ingress.kubernetes.io/target-type: ip
        # List all subnets which the EKS cluster is attached to
        alb.ingress.kubernetes.io/subnets: subnet-value1, subnet-value2
        # To enable TLS, specify the certificate ARN here
        alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:eu-west-1:1111111111:certificate/111aaaaa-1111-1aa1-11a1-111aaaa1b1a1
        # Add this to automatically redirect HTTP traffic to HTTPS
        alb.ingress.kubernetes.io/ssl-redirect: "443"
        # Listen on standard HTTP and HTTPS ports
        alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
      # The following parameters are already configured by mxpc-cli
      domain: mendix.example.com
      enableTLS: false
      ingressClassName: alb
      path: "/*"
      pathType: ImplementationSpecific
# ...
# omitted lines for brevity
# ...
For more details, see the AWS Load Balancer Controller documentation.

Azure Application Gateway Ingress Controller (AGIC)

{{% alert color="warning" %}}

To properly configure the Azure Application Gateway Ingress Controller (AGIC), manual modifications to the OperatorConfiguration object are necessary. 

{{% /alert %}}

The Azure Application Gateway Ingress Controller (AGIC) is a specialized ingress controller for Azure Kubernetes Service (AKS) that uses Azure Application Gateway (a Layer 7 load balancer) to manage HTTP/HTTPS traffic. It continuously monitors Kubernetes resources and updates the Application Gateway to expose selected services to the Internet.

Running as a pod within the AKS cluster, AGIC translates the cluster’s state into Application Gateway configurations and applies them via Azure Resource Manager (ARM), providing seamless Azure-native ingress management. Refer this document to install AKS Application Gateway Ingress Controller.

{{% alert color="info" %}}
Azure Gateway Ingress Controller needs up to 90 seconds to remove a pod from its routing table. Stopping an app pod immediately would still send traffic to the pod for a few minutes, causing random 502 errors to appear in the client web browser. Hence, its recommended to add runtimeTerminationDelaySeconds value to the OperatorConfiguration CR.
{{% /alert %}}

Configuring AGIC in mxpc-cli:

Open image-20250212-150224.png
image-20250212-150224.png
{{< figure src="/attachments/deployment/private-cloud/private-cloud-ingress/agic-configuration.png" class="no-border" >}}
Select Ingress Type as kubernetes-ingress. kubernetes-ingress will configure ingress according to the additional domain name you supply. 

Ingress Domain Name - provide the domain name which was registered for AGIC

Ingress Path -  Set it to /

Enable TLS - allows you to enable or disable TLS for the Mendix App’s ingress.

Custom Ingress Class - enable this option for providing the ingress class name.

Ingress Class Name - provide azure/application-gateway as the ingress class name 

Set Ingress Class as Annotation - Select this option. This option adds the kubernetes.io/ingress.class annotation to set the ingress class.border" >}}

Choose one of the following methods to update the Operator configuration, noting that your choice will determine whether the changes affect a specific app environment or all environments within a namespace:

Mendix Platform GUI per app environment:

Navigate to the Global Navigation top bar

Go to Deployment > Private Cloud

Select your Cluster

Choose the appropriate Namespace

In the Apps section, click “Configure App Cog icon”

Kubectl command-line tool at the namespace level, ensuring the changes affect all hosted app environments within that namespace:

Directly edit the OperatorConfiguration object using kubectl

Open image-20250305-135015.png
image-20250305-135015.png
Mendix Platform GUI option


apiVersion: privatecloud.mendix.com/v1alpha1
kind: OperatorConfiguration
# ...
# omitted lines for brevity
# ...
spec:
  # Endpoint (Network) configuration
  endpoint:
    type: ingress
    ingress:
      annotations:
        # Specify the name of a Listener TLS Certificate to use
        appgw.ingress.kubernetes.io/appgw-ssl-certificate: agic-tls
        # Add this to automatically redirect HTTP traffic to HTTPS
        appgw.ingress.kubernetes.io/ssl-redirect: true
        # Ingress class, this is automatically set by mxpc-cli
        kubernetes.io/ingress.class: azure/application-gateway
      # The following parameters are already configured by mxpc-cli
      domain: mendix.example.com
      enableTLS: true
      path: "/"
      pathType: ImplementationSpecific
# ...
# omitted lines for brevity
# ...
If you would also like to set up TLS certificates, kindly follow SSL certificate documentation which explains how to make AGIC load a cert from KeyVault.

Traefik Ingress Controller

Traefik is a cloud-native reverse proxy and a load balancer. When deployed as an ingress controller in Kubernetes, it manages HTTP and HTTPS traffic to services running within the cluster. It automatically discovers services using Kubernetes' native APIs, based on Kubernetes Ingress resources and other configurations. One of the main advantages of using Traefik is its built in Let's Encrypt support

Official documentation to install is provided here

{{% alert color="warning" %}}
Traefik uses 2 types of providers: CRDs or Kubernetes Ingress. Please make sure you install Kubernetes Ingress one as it’s the only one supported.
{{% /alert %}}

Configuring Traefik in mxpc-cli:

Open image-20250213-084933.png
image-20250213-084933.png
Select Ingress Type as kubernetes-ingress. kubernetes-ingress will configure ingress according to the additional domain name you supply. 

Ingress Domain Name - provide the domain name which was registered for ALB

Ingress Path -  Set it to /

Enable TLS - allows you to enable or disable TLS for the Mendix App’s ingress.

Custom Ingress Class - enable this option for providing the ingress class name.

Ingress Class Name - provide traefik as the ingress class name 

Set Ingress Class as Annotation -  Unselect this option. This option adds the legacy kubernetes.io/ingress.class annotation to set the ingress class, instead of using the ingress class name.
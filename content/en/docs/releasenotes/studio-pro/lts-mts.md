---
title: "LTS, MTS, and Monthly Releases"
url: /releasenotes/studio-pro/lts-mts/
weight: 1
---

## Introduction

The goal of this document is to clarify Mendix Studio Pro version options and help you decide which version to run your applications on.

To understand the different types of versions and releases, consider the example of Studio Pro [9.24.1](/releasenotes/studio-pro/9.24/#9241) and see the sections below.

### Major Version {#major-version}

In the 9.24.1 example, the first number (**9**) indicates the major version.

Mendix supports three major versions at a time. To clarify our current offering and in relation to our SLA, today Mendix supports major versions 9, 10, and 11. 

As of Mendix 10, to make planning version upgrades easier for our users, Mendix will release in a two-year cadence, which means that every two years we plan to release a new major version in June. This will help in preparing and planning to adopt new releases and upgrade older Mendix versions. Each major version release will start with two public [beta releases](/releasenotes/release-status/).

For Mendix 10 and 11, we are planning to follow this release timeline:

| Release | Date |
| --- | --- |
| 10.6 MTS | December 2023 |
| 10.12 MTS | June 2024 |
| 10.18 MTS | December 2024 |
| 10.22 | April 2025 |
| 10.23 | May 2025 |
| 11.0 GA | June 2025 |
| 10.24 LTS | June 2025 |

### Minor Version

In the 9.24.1 example, the second number (**24**) indicates the minor version. 

A minor version is generally released every month. It includes new features and fixes, but it does not include breaking changes.

### Patch Version {#patch-version}

In the 9.24.1 example, the third number (**1**) indicates the patch version. 

A patch version is released on an as-needed basis. It includes security fixes and critical bug fixes, but it does not include new features or breaking changes (unless these address security issues).

## Long-Term Support Version (LTS) {#lts}

An LTS version is the last minor release of a major version. Its release is aligned with the GA release of the next major version. For example, the 10.24 LTS version will be released in the same month as Mendix 11 GA. Once a minor version becomes an LTS, it will not have new features added to it. 

These are the current LTS versions of Studio Pro:

* [8.18](/releasenotes/studio-pro/8.18/) (started in March, 2021)
* [9.24](/releasenotes/studio-pro/9.24/) (started in March, 2023)
* [10.24](/releasenotes/studio-pro/10.24/) (started in June, 2025)

Release notes for the current LTS versions are marked with an LTS badge (<text class="badge badge-pill badge-lts" style="margin-left:0px">LTS</text>) in the left sidebar.

Mendix advises using LTS versions for all production apps, as LTS versions of Studio Pro enjoy the highest security and stability. LTS versions also have the longest support duration compared to other versions.

{{% alert color="info" %}}

Mendix recommends using the latest patch version of the release as the latest version will contain the latest fixes.

{{% /alert %}}

### Support Duration

Support for an LTS version ends when a third consecutive major version is released for general availability (for example, support on version 6 ended with the GA release of version 9).

This diagram illustrates the LTS concept with version and [end of support](#end) examples:

{{< figure src="/attachments/releasenotes/studio-pro/lts-mts/lts.png" class="no-border" >}}

{{% alert color="info" %}}For details on extending support for an older major version, see the [Extended Support](/support/#extended-support) section in *Mendix Support*.{{% /alert %}}

### Release Cycle

An LTS starts with the release of a new higher major version for general availability. For a description of Mendix’s major release cadence, see the [Major Version](#major-version) section above.

For an LTS version, there will only be [patch](#patch) releases on an as-needed basis.

### Support with Patch Releases

For the support duration, the LTS will receive patch releases only. 

### Upgrading Impact

The impact of upgrading from an LTS to an LTS is often the lowest of these options. That is because an LTS has the longest support duration and only gets critical fixes, which results in high stability over time. In addition, depending on how rigorous your application update/release process is, the upgrade would only have to be done once, as opposed to multiple times between MTS versions or monthly release versions.

## Medium-Term Support Version (MTS) {#mts}

An MTS is a minor version released approximately every 6 months after a new major version release for general availability.

This is the current MTS version of Studio Pro:

* [10.6](/releasenotes/studio-pro/10.6/) (started in December, 2023)
* [10.12](/releasenotes/studio-pro/10.12/) (started in June, 2024)
* [10.18](/releasenotes/studio-pro/10.18/) (started in December, 2024)

Release notes for the current MTS version are marked with an MTS badge (<text class="badge badge-pill badge-mts" style="margin-left:0px">MTS</text>) in the left sidebar.

Mendix MTS versions offer a balance between getting the latest and greatest Mendix has to offer every month, and security and stability.

{{% alert color="info" %}}

Mendix recommends using the latest patch version of the release as the latest version will contain the latest fixes.

{{% /alert %}}

### Support Duration

Support for an MTS version ends three months after the LTS version of that major version is released. For example, version 10.24 is released in June 2025. Support for the 10.6, 10.12 and 10.18 MTS versions will then end in September 2025.

### Release Cycle

MTS versions are introduced as minor versions approximately every 6 months after a new major version release for general availability. There will only be patch releases for an MTS on an as-needed basis.

### Support with Patch Releases

For the support duration, the MTS will receive patch releases only.

### Upgrading Impact

The impact of upgrading between MTS versions is higher than upgrading between LTS versions. That is because an MTS has a shorter support duration than an LTS, which means it has less time to become as stable as an LTS version. In addition, the upgrade has to be done approximately every six months.

## Monthly Release Version {#patch}

A monthly release is a non-MTS/LTS minor release that only receives patches for critical/security issues if they can be released earlier and faster than the next minor version release.

### Support Duration

Monthly release versions do not enjoy regular patch releases like MTS and LTS versions do. Bugs found in monthly release versions are directly resolved in the subsequent monthly release. 

For example, if a bug is found in version 9.1, it will only be fixed in the next monthly release, which would be version 9.2.

### Release Cycle

This is a monthly release that includes new features as well as security patches that are applied to existing MTS and LTS versions.

### Support with Patch Releases

A monthly release only receives patches for critical/security issues if they can be released earlier and faster than the next minor version release. Otherwise, bugs found in a monthly release will only be fixed rolling forward to the next minor version. 

For example, if you are using version 9.1 after 9.2 has been released and you find a critical issue in 9.1, that fix will only be released in the next monthly release, which would be 9.3.

If a bug is found in a monthly-release version for an older major version, it will only be fixed as a patch on the existing LTS (for example, a bug found in 8.16 will only be fixed for a patch release on the LTS version 8.18). 

### Upgrading Impact

The impact of upgrading between monthly releases is the highest of these options. That is because every month, all new features are shipped in such minor versions, including all bug fixes and security patches. In addition, the upgrade has to be done monthly, which can be a significant drain on resources.

## End of Support {#end}

The "end of support" for a version means that fixes (including fixes for security vulnerabilities), updates, investigating, troubleshooting, and other forms of assistance are no longer provided. Applications using [unsupported Mendix versions](/developerportal/deploy/mendix-cloud-deploy/#mendix-cloud-supported-versions) cannot be deployed or run on Mendix Cloud. They can still be deployed and run on infrastructure that is owned by customers, but Mendix will not investigate any issues related to these applications.

If a new major version is nearing release, that means an older major version is nearing its end of support. If you are still using a major version for which support will end, Mendix recommends that you stop using that version and move to a newer LTS version that is supported. 

Furthermore, you should uninstall old versions for which support has ended. If you continue to use versions for which support has ended, you do so at your own risk for your applications, data, and security. Mendix does not recommend using versions for which support has ended unless you have purchased the [Extended Support](/support/#extended-support) product.

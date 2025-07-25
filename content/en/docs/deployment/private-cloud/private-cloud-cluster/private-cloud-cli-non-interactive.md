---
title: "Install and Configure Mendix on Kubernetes Non-Interactive Mode"
linktitle: "Non-Interactive Mode"
url: /developerportal/deploy/private-cloud-cli-non-interactive/
description: "Describes how to install and configure Mendix on Kubernetes in non-interactive mode"
weight: 5
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

## Introduction

To support automation namespace installation and configuration we provide a non-interactive mode in our configuration tool.

{{% alert color="info" %}} To use non-interactive mode, you need Mendix Operator version 2.1.0 or above.{{% /alert %}}

Please see [Download the Configuration Tool](/developerportal/deploy/standard-operator/#download-configuration-tool) for information on how to download the configuration tool.

{{% alert color="info" %}} Use "./mxpc-cli <command> --help" for more information about a given command. {{% /alert %}}

{{% alert color="info" %}} Non-interactive mode is not currently supported for Global Operator.{{% /alert %}}

The following parameters may be used in the commands:
  
* `--namespace` – a cluster namespace.
* `--clusterType` – a cluster type *openshift* or *generic*.
* `--clusterMode` – a cluster mode *standalone* or *connected*.
* `-i` – the *namespace id* that is shown in the **Installation** tab of a namespace in the Mendix on Kubernetes Portal.
* `-s` – the *namespace secret* that is shown in the **Installation** tab of a namespace in the Mendix on Kubernetes Portal.
* `--file` – a file which contains the configuration for the namespace.

When using connected mode, you need to put namespace id and namespace secret as arguments. These parameters are used by the Mendix Gateway Agent to connect to the Mendix on Kubernetes Portal. You can see these values in the installation command, as the -i and -s parameters, respectively.

{{< figure src="/attachments/deployment/private-cloud/private-cloud-cluster/private-cloud-cli-non-interactive/installation-command.png" class="no-border" >}}

## Base Installation

To perform the [base installation](/developerportal/deploy/standard-operator/#base-installation), use the following command:

```shell
./mxpc-cli base-install --namespace <namespace> -i <namespace-id> -s <namespace-secret> --clusterMode <cluster-mode> --clusterType <cluster-type>
```

The namespace-id and namespace-secret are only required when using Mendix on Kubernetes in connected mode.

## Apply Configuration

To [configure a standard namespace](/developerportal/deploy/standard-operator/#configure-namespace) with a configuration file, use the following command:

```shell
./mxpc-cli apply-config -i <namespace-id> -s <namespace-secret> --file <config-file>
```

The namespace-id and namespace-secret are only required when using Mendix on Kubernetes in connected mode. 

In case of standalone mode, the namespace-id and namespace-secret are not required. Instead, use the following command:

```shell
./mxpc-cli apply-config --file <config-file>
```

To generate the config file, follow the instructions described in [Creating a Mendix on Kubernetes Cluster](/developerportal/deploy/private-cloud-cluster/). The **mx_config_cli.yaml** file is generated when you click **Write YAML** during the [Review and Apply](/developerportal/deploy/standard-operator/#review-apply) phase of configuring your namespace interactively.

Below is an example of a config file. The example is provided for reference only. To make sure that the config file captures all the values of the input fields used in your own app, you must generate your own **mx_config_cli.yaml** file.

```yaml
namespace: my-namespace
cluster_mode: connected
mask:
  database_plan: true
  storage_plan: true
  ingress: true
  registry: true
  proxy: false
  custom_tls: false
database_plan:
  name: ephemeral-database
  type: ephemeral
storage_plan:
  name: ephemeral-storage
  type: ephemeral
ingress:
  type: openshift-route
  enable_tls: false
  k8s_ingress: null
  service: null
registry:
  type: openshift4
```

## Upgrade Mendix Operator and Mendix Gateway Agent

To [upgrade the versions of Mendix components in your namespace](/developerportal/deploy/private-cloud-upgrade-guide/#upgrade-cluster), use the following command:

```shell
./mxpc-cli upgrade-namespace --clusterType <cluster-type> --namespace <namespace>

```

{{% alert color="info" %}}
In case of Global Namespace installation, the upgrade procedure is not applicable for the managed namespace.
{{% /alert %}}

## Converting a Standard Namespace to Global Operator Namespace

To convert a standard namespace to a global operator namespace, perform the following steps:

```shell
./mxpc-cli global-operator convert-namespace -g <global-operator-main-namespace> -t <target-namespace>

```

{{% alert color="info" %}}
Once the conversion is complete, the managed namespace cant be reverted back to standard operator namespace.
{{% /alert %}}

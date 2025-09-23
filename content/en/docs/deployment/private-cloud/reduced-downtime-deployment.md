---
title: "Reducing Deployment Downtime"
url: /developerportal/deploy/private-cloud-reduced-downtime/
description: "Describes how to reduce downtime when deploying apps in Mendix on Kubernetes environments."
weight: 35
---
## Introduction

Kubernetes allows to update an app without downtime by [performing a rolling update](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/). Instead of stopping an app and then starting it with an updated configuration, Kubernetes can replace pods (replicas) one by one.

The Mendix on Kubernetes Operator uses a [recreate](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#recreate-deployment) strategy by default. That is, the current version (configuration) of an app stops, and then the new version starts.

Starting from version 2.24.0, the Operator will automatically perform a Rolling update for any environment that meets the [prerequisites](#prerequisites-2.24.0).

1. The environment has 2 or more replicas.
2. The configuration update does not modify the app source code (MDA or container image).

{{% alert color="info" %}}
Versions 2.20.0 to 2.23.1 of the Operator had an option to manually enable a **PreferRolling** strategy. That is, the Operator tried to perform a rolling update whenever possible. If the Operator detected that a database schema update was needed, it switched to a Recreate strategy to perform a full restart. If the new version of the app had model changes, deploying it required a schema update. In that case, the Mendix on Kubernetes Operator automatically stopped all replicas of the app, causing downtime.
{{% /alert %}}

## Prerequisites

## Prerequisites for Operator version 2.24.0 and higher{#prerequisites-2.24.0}

The Operator will automatically perform a Rolling update for any environment that meets the following conditions:

* The environment has 2 or more replicas.
* The configuration update does not modify the app source code (MDA or container image).

{{% alert color="warning" %}}
Mendix Operator versions 2.20.0 to 2.23.1 had a experimental feature that also performed database schema upgrades with a Rolling strategy.
This feature was removed in Operator 2.24.0, as it doesn't work well with the latest Mendix Runtime security features.
{{% /alert %}}

## How the Operator chooses a deployment strategy

If any of the following conditions is true, the Operator will always use a **Recreate** strategy, performing a full stop of all of the app's replicas:

* There are app pods that are running a different (older) version of the app image: there are changes in the app MDA or base OS image.
* The app environment has 1 replica.

Otherwise, the Operator will perform a **Rolling** update automatically.

As a **Rolling** strategy can run multiple versions of the app at the same time, requests from the browser must be routed to a matching app version (that is, an app that has the same microflow or nanoflow parameters). The Operator uses Kubernetes service labels to perform an atomic switch, and instantly switch all clients to the updated version. This is done automatically once the number of updated replicas reaches a certain threshold. By default the threshold is 50% of all replicas. The value is specified in the [switchoverThreshold](#prefer-rolling-in-standalone) parameter.

### Use Cases

Whether a change can be performed without downtime depends on the type of the change. For example, the following changes can be done without downtime:

* Changing app constants, MxAdmin password or debugger settings
* Changing environment variables, Runtime or Java options
* Changing Runtime Metrics settings
* Upgrading the Mendix Operator version

The following changes will cause a full restart and downtime:

* Any changes that cause a modified MDA file
* Rebuilding the same MDA version with a different base image version (e.g. switching to another Java version or installing the latest CVE patches)

## Configuring the Deployment Strategy parameters in Standalone Environments {#deployment-strategy-in-standalone}

To reduce deployment downtime, add the `deploymentStrategy` section to your `MendixApp` CR, as in the following example:

```yaml
apiVersion: privatecloud.mendix.com/v1alpha1
kind: MendixApp
metadata:
# ...
# omitted lines for brevity
# ...
spec:
  # ...
  # omitted lines for brevity
  # ...
  # Add or update this section:
  deploymentStrategy:
    switchoverThreshold: 50%
    rollingUpdate:
      maxSurge: 0
      maxUnavailable: 50%
```

For more information on the `MendixApp` CR, see [Editing CR](/developerportal/deploy/private-cloud-operator/#edit-cr).

You can specify the following options:

* **switchoverThreshold** – Specifies a threshold of updated, ready replicas after which all clients should switch to the updated version. The threshold can be a percentage or an absolute value.
    For example, setting this to **50%** will switch all clients to the updated app version once 50% of all replicas are running the updated version. If not otherwise specified, 50% is used as the default value. This option is only used if the strategy **type** is set to **PreferRolling**.
* **rollingUpdate** - Specifies parameters for rolling updates if the Operator is able to perform the update without a restart. These parameters are used as Kubernetes [rollingUpdate](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) parameters:
    * **maxSurge** – Specifies an absolute or percentage value for how many additional replicas can be added during the deployment process. The default **0** value means that no additional replicas are added during the rollout process, and instead existing replicas are stopped to avoid using additional cluster resources.
    * **maxUnavailable** – Specifies an absolute or percentage value for how many replicas can be stopped to be replaced with updated versions during the rollout process. The default **50%** value means that half of the replicas would be stopped during the update process. Lowering this value slows down the rollout process, but ensures that less replicas are stopped during the update process.

## Preventing Kubernetes Disruptions

Kubernetes can stop an app's pods if needed to stop a node (to scale down and consolidate apps to run on fewer nodes), or perform a node update (for example, install CVE patches on the host OS). You can add a [PodDisruptionBudget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/) to an app to ensure that Kubernetes only stops a limited number of an app's pods, and if necessary waits for replacement pods to become available.

To add a PodDisruptionBudget, create the following PodDisruptionBudget, replacing `<mendixapp-cr-name>` with your app's internal name (the MendixApp CR name):

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: <mendixapp-cr-name> # This should be updated to match the MenedixApp CR name
spec:
  maxUnavailable: 1 # Ensure that at most 1 replica is stopped by Kubernetes
  selector:
    matchLabels:
      privatecloud.mendix.com/app: <mendixapp-cr-name> # This should be updated to match the MenedixApp CR name
      privatecloud.mendix.com/component: mendix-app
```

## Limitations

* This feature is only supported by Mendix Operator version 2.24 (and later). Mendix Operator versions 2.20.0 to 2.23.1 used to have an experimental implementation of this feature; upgrading to 2.24.0 or later is highly recommended.
* Deploying a new version of the app will cause downtime if there are any changes in the app MDA or the base OS image.
* To ensure that scheduled events [are correctly synchronized at startup](/releasenotes/studio-pro/10.20/#improvements), it is recommended to use Mendix 10.20 or later.

---
title: Google Cloud Kubernetes Engine
hide_title: true
sidebar_position: 5
---

# Google Cloud Kubernetes Engine

The Cado platform will collect key logs and forensic artifacts from Google Cloud Kubernetes Engine containers.

:::info
Due to the way the Cado platform interacts with Kubernetes, it is not possible to import containers built from a [distroless](https://github.com/GoogleContainerTools/distroless#why-should-i-use-distroless-images) image.
:::

## Requirements

To import GKE containers with Cado Response, the `iam.serviceAccounts.implicitDelegation` IAM permission added to the Service Account.

Currently, for GKE import Cado only suports GCP accounts configured using Workload Identity Federation. See more in the [GCP Import Settings](/cado-response/deploy/gcp/gcp-settings#workload-identity-federation) page.

## Import Steps

1) Go to **Import > Kubernetes Engine**

![Cado Import Screen showing the Kubernetes Engine options](/img/import.png)

2) Go through the steps to choose your **Cluster**, **Pod** and **Container**:

![Cado Import Screen showing the available Kubernetes Engine Clusters](/img/gke.png)

3) Cado will now automatically collect all the key logs and forensic artifacts from the container to enable an investigation.
For a typical acquisition, import and processing will take a few minutes to complete.

![Cado showing the confirmation screen of a successful Kubernetes Engine container capture](/img/eks3.png)





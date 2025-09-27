# GitOps Context: Key Kubernetes Tools and Best Practices

## Overview
GitOps is a methodology for managing infrastructure and application deployments using Git as the source of truth. Changes are made via pull requests and automatically applied to Kubernetes clusters using automation tools.

## Key Kubernetes GitOps Tools

### 1. kubectl
- **Purpose:** Command-line tool for interacting with Kubernetes clusters.
- **Usage:** Apply, update, and manage resources defined in YAML manifests.
- **Common Commands:**
  - `kubectl apply -f <file.yaml>`: Apply configuration from a file.
  - `kubectl get <resource>`: List resources (e.g., pods, deployments).
  - `kubectl describe <resource> <name>`: Show detailed info.
  - `kubectl diff -f <file.yaml>`: See changes before applying.

### 2. kustomize
- **Purpose:** Native Kubernetes tool for customizing YAML configurations.
- **Usage:** Layer and patch base manifests for different environments (dev, staging, prod).
- **Common Commands:**
  - `kubectl kustomize <dir>`: Build manifests from a kustomization directory.
  - `kubectl apply -k <dir>`: Apply kustomized resources.
- **Features:**
  - Overlays for environment-specific changes
  - Patching and resource composition

### 3. Config Sync (GKE/GCP)
- **Purpose:** A GitOps system that is natively built into Google Kubernetes Engine
- **Usage:** Automatically syncs application manifests from Git repositories to clusters, manages rollbacks, and visualizes deployments.
- **Features:**
  - Web UI in the GCP Console
  - Automated sync and health checks
  - Focused on edge based deployment with a central lifecycle managmeent.

If using Google Cloud Platform (GCP), you can manage Config Sync with `gcloud`:


#### Operations

**Install Config Sync:**
```sh
gcloud container fleet config-management enable
```

**Configure a Git repository:**
```sh
gcloud beta container fleet config-management apply --membership=<CLUSTER_NAME> \
  --config=<CONFIG_YAML_PATH> --project=<PROJECT_ID>
```

**Check Config Sync status:**
```sh
gcloud beta container fleet config-management status
```

**Upgrade Config Sync version:**
To upgrade Config Sync to a newer version on your cluster, use the following command:
```sh
gcloud beta container fleet config-management update --membership=<CLUSTER_NAME> --version=<NEW_VERSION>
```
Replace `<CLUSTER_NAME>` with your cluster name and `<NEW_VERSION>` with the desired Config Sync version. For more details, includeing
the latest versions, see the official GCP documentation: https://cloud.google.com/kubernetes-engine/enterprise/config-sync/docs/release-notes

#### Troubleshooting

**Status**

To check the status of a `RepoSync` resource, use the following command, replacing `NAMESPACE` with the namespace where you created your namespace repository:

```
kubectl get reposync repo-sync -n NAMESPACE -o yaml
```

To check the status of a `ResourceGroup` for a `RootSync` object, run:

```
kubectl get resourcegroup root-sync -n config-management-system
```

To check the status of a `ResourceGroup` for a `RepoSync` object, use the following command, replacing `NAMESPACE` with the namespace where you created your namespace repository:

```
kubectl get resourcegroup repo-sync -n NAMESPACE
```

To view the logs for the `ResourceGroup Controller`, run:

```
kubectl logs deployment resource-group-controller-manager -c manager -n resource-group-system
```

**Webhook Issues**

To check if the admission webhook pods can be scheduled and are healthy, use these commands:

```
kubectl describe deploy admission-webhook -n config-management-system
kubectl get pods -n config-management-system -l app=admission-webhook
```

If you have a namespace that is stuck in the "Terminating" phase, you can use the following command to delete the Config Sync admission webhook as a workaround:

```
kubectl delete deployment.apps/admission-webhook -n config-management-system
```

### 4. ArgoCD
- **Purpose:** Declarative GitOps continuous delivery tool for Kubernetes.
- **Usage:** Automatically syncs application manifests from Git repositories to clusters, manages rollbacks, and visualizes deployments.
- **Features:**
  - Web UI for monitoring and managing deployments
  - Automated sync and health checks
  - Focused on client server with one ArgoCD server connecting to multiple clusters for deployment.

### 5. FluxCD
- **Purpose:** GitOps operator for Kubernetes that automates deployment of resources from Git.
- **Usage:** Watches Git repositories for changes and applies them to clusters, supports multi-tenancy and integrations with other tools.
- **Features:**
  - Automated reconciliation of cluster state with Git
  - Supports Helm, Kustomize, and image update automation
  - A set of in cluster controllers with Kubernetes native approach.  The UI is left to the vendors like Microsoft.

**Useful Links:**
- [GCP GitOps Best Practices](https://cloud.google.com/kubernetes-engine/enterprise/config-sync/docs/concepts/gitops-best-practices)

## Best Practices
- Store all Kubernetes manifests in Git
- Use kustomize overlays for environment separation
- Automate deployments with Config Sync, ArgoCD or FluxCD
- Protect main branches and require PR reviews
- Monitor sync status and cluster health regularly

---
This file provides context for using GitOps with Kubernetes, focusing on kubectl, kustomize, and GCP Config Sync. For more details, see the linked sources above.
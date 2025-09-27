
# GitOps Extension

This project provides context, documentation, and best practices for implementing GitOps workflows with Kubernetes. It covers key tools, operational commands, and troubleshooting steps for popular GitOps solutions including kubectl, kustomize, Config Sync (GCP), ArgoCD, and FluxCD.

## Features
- Overview of GitOps methodology and benefits
- GCP Config Sync setup, upgrade, and troubleshooting commands
- ArgoCD and FluxCD usage notes
- Best practices for managing infrastructure and application deployments


## Local Installation (Gemini CLI Extension)
To install and use this extension locally with `gemini-cli`, follow these steps:

1. Clone this repository (if not already done):
	```sh
	git clone https://github.com/mikebz/gitops-extension.git
	```
2. Link the extension locally using Gemini CLI:
	```sh
	gemini extension link /path/to/gitops-extension
	```
	Replace `/path/to/gitops-extension` with the actual path to your extension file.
3. Verify the extension is linked:
	```sh
	gemini extension list
	```
4. For more details, see the official [Gemini CLI Extension Guide](https://github.com/google-gemini/gemini-cli/blob/main/docs/extension.md).

## Supported Tools
- **kubectl**: Command-line tool for managing Kubernetes resources
- **kustomize**: Customization of Kubernetes YAML manifests
- **Config Sync (GCP)**: Native GitOps for Google Kubernetes Engine
- **ArgoCD**: Declarative GitOps continuous delivery for Kubernetes
- **FluxCD**: GitOps operator for Kubernetes with automation and reconciliation

## Operations
See `gitops.md` for detailed operational commands, including:
- Installing and configuring Config Sync
- Upgrading and checking versions
- Troubleshooting common issues
- Managing deployments with ArgoCD and FluxCD

## Best Practices
- Store all Kubernetes manifests in Git
- Use overlays for environment separation
- Automate deployments and monitor cluster health
- Protect main branches and require PR reviews

## Resources
- [GCP GitOps Best Practices](https://cloud.google.com/kubernetes-engine/enterprise/config-sync/docs/concepts/gitops-best-practices)
- [Config Sync Release Notes](https://cloud.google.com/kubernetes-engine/docs/release-notes-config-sync)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- [FluxCD Documentation](https://fluxcd.io/)

---
For more details, see the context files in this repository or reach out to the project owner.

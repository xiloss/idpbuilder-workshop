# idpbuilder-workshop

## Prerequisites
Ensure you have the following tools installed on your computer.

Required

- idpbuilder: version 0.3.0 or later
- kubectl: version 1.27 or later
- Docker desktop is supported.
-  Podman desktop is not supported however idpbuilder can create a cluster using rootful. You need tp set the DOCKER_HOST env var property using podman to let idpbuilder to talk with the engine (e.g export DOCKER_HOST="unix:///var/run/docker.sock")
  
Your computer should have at least 6 GB RAM allocated to Docker.

## Install IDPBuilder

The easiest way to get started is to grab the idpbuilder binary for your platform and run it. You can visit our nightly releases page to download the version for your system, or run the following commands:

```
arch=$(if [[ "$(uname -m)" == "x86_64" ]]; then echo "amd64"; else uname -m; fi)
os=$(uname -s | tr '[:upper:]' '[:lower:]')

idpbuilder_latest_tag=$(curl --silent "https://api.github.com/repos/cnoe-io/idpbuilder/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
curl -LO  https://github.com/cnoe-io/idpbuilder/releases/download/$idpbuilder_latest_tag/idpbuilder-$os-$arch.tar.gz
tar xvzf idpbuilder-$os-$arch.tar.gz
```

You can then run idpbuilder with the create argument to spin up your CNOE IDP:

```./idpbuilder create```

## Installation

```
idpbuilder create --use-path-routing \
  --package https://github.com/cnoe-io/stacks//ref-implementation
```

This will take ~6 minutes for everything to come up. To track the progress, you can go to the ArgoCD UI.

### What was installed?

- Argo Workflows to enable workflow orchestrations.
- Backstage as the UI for software catalog and templating. Source is available here.
- External Secrets to generate secrets and coordinate secrets between applications.
- Keycloak as the identity provider for applications.
- Spark Operator to demonstrate an example Spark workload through Backstage.

If you don't want to install a package above, you can remove the ArgoCD Application file corresponding to the package you want to remove. For example, if you want to remove Spark Operator, you can delete this file.

The only package that cannot be removed this way is Keycloak because other packages rely on it.

### Accessing UIs

- Argo CD: https://cnoe.localtest.me:8443/argocd
- Argo Workflows: https://cnoe.localtest.me:8443/argo-workflows
- Backstage: https://cnoe.localtest.me:8443/
- Gitea: https://cnoe.localtest.me:8443/gitea
- Keycloak: https://cnoe.localtest.me:8443/keycloak/admin/master/console/


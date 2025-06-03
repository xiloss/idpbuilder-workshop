# IDPBUILDER WORKSHOP

## Clone the following repositories

1. https://github.com/cloudstation-dev/stacks.git
   it contains a fork of the base examples from the official idpbuilder source, with some specific modifications and additions
2. https://github.com/cloudstation-dev/idpbuilder-workshop.git
   it contains the README and examples instructions for the workshop

## Install Requirements

### When using Ubuntu

#### Setup sudo with no password or your current user (optional)

Skip the next command if you already have that ability or if you want to enter a password each time you open a new terminal session for the current user.

```bash
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USER >/dev/null
```

Ensure you are up-to-date with all the required packages.
If you are using ubuntu as host operating system or virtual machine to run the idpbuilder workshop,
which is the base where the whole environment has been tested, ensure you have the following packages installed:

```bash
sudo apt update
sudo apt -y install openssh-server
sudo apt -y install curl
sudo apt -y install git
sudo apt -y install docker.io
```

Check if the docker daemon is running correctly

```bash
systemctl status docker
```

Ensure the docker access is granted to the current user

```bash
sudo usermod -aG docker $USER
```

and reboot

```bash
sudo reboot
```

### Prerequisites

Ensure you have the following tools installed on your computer.

#### Required

- idpbuilder: version 0.3.0 or later
- kubectl: version 1.27 or later
- docker

Docker desktop is supported.
Podman desktop is not supported, however idpbuilder can create a cluster using rootful.
You need tp set the DOCKER_HOST env var property using podman to have idpbuilder accessing the container runtime engine socket (e.g export DOCKER_HOST="unix:///var/run/docker.sock")

Your computer should have at least 6 GB RAM allocated to Docker.
Your disk space should have at least 4GB of available space for the initial configuration to work.
 

### Installation

The whole setup will be operational executing the next steps.

#### Install kubectl CLI

To install the kubectl latest version, run

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo mv ./kubectl /usr/local/bin
sudo chmod ugo+x /usr/local/bin/kubectl
kubectl version
```

As an optional step, add the completion executing the next snippets

```bash
source <(kubectl completion bash)
kubectl completion bash > $HOME/.kubectl.completion.bash.inc
printf "
# kubectl shell completion
source '$HOME/.kubectl.completion.bash.inc'
" >> $HOME/.bashrc
source $HOME/.bashrc
```

#### Install idpbuilder CLI

The easiest way to get started is to grab the idpbuilder binary for your platform and run it. You can visit our nightly releases page to download the version for your system, or run the following commands:

```bash
arch=$(if [[ "$(uname -m)" == "x86_64" ]]; then echo "amd64"; else uname -m; fi)
os=$(uname -s | tr '[:upper:]' '[:lower:]')
idpbuilder_latest_tag=$(curl --silent "https://api.github.com/repos/cnoe-io/idpbuilder/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
curl -LO  https://github.com/cnoe-io/idpbuilder/releases/download/$idpbuilder_latest_tag/idpbuilder-$os-$arch.tar.gz
tar xvzf idpbuilder-$os-$arch.tar.gz idpbuilder
sudo mv ./idpbuilder /usr/local/bin
idpbuilder version
```

idpbuilder has also a completion snippet for your local shell. For bash case, execute the following snippet

```bash
source <(idpbuilder completion bash)
idpbuilder completion bash > $HOME/.idpbuilder.completion.bash.inc
printf "
# idpbuilder shell completion
source '$HOME/.idpbuilder.completion.bash.inc'
" >> $HOME/.bashrc
source $HOME/.bashrc
```

A generic way to start with CNOE IDP using a base setup is to run

```bash
./idpbuilder create
```

#### Install helm (optional)

Although not required, the installation of the helm command can be helpful to debug and troubleshoot existing components, as so as test additional add-ons.
Use the following commands to install a specific version of helm, the 3.14 for this case:

```bash
curl -fsSL https://get.helm.sh/helm-v3.14.0-linux-amd64.tar.gz -o helm.tar.gz
tar -zxvf helm.tar.gz linux-amd64/helm
sudo mv linux-amd64/helm /usr/local/bin/helm
rm -rf linux-amd64 helm.tar.gz
helm version
```

Additionally, add the completion, current commands are for the bash shell:

```bash
source <(helm completion bash)
helm completion bash > $HOME/.helm.completion.bash.inc
printf "
# helm shell completion
source '$HOME/.helm.completion.bash.inc'
" >> $HOME/.bashrc
source $HOME/.bashrc
```

#### Install argocd CLI (optional)

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x argocd-linux-amd64
sudo mv argocd-linux-amd64 /usr/local/bin/argocd
argocd version
```

Additionally, add the completion, current snippet is for the bash shell

```bash
source <(argocd completion bash)
argocd completion bash > $HOME/.argocd.completion.bash.inc
printf "
# argocd shell completion
source '$HOME/.argocd.completion.bash.inc'
" >> $HOME/.bashrc
source $HOME/.bashrc
```

#### Install Kind (optional)

idpbuilder creates a kind cluster. In order to use additional features for troubleshooting, the kind CLI can be installed using the script available in `kind/get-kind.sh`
The script allow to select a kind version, using the last available one is generally a good choice. In case of compatibility issues, just be sure to install the same kind CLI version used by idpbuilder during the installation. The image version is visible in the initial steps after running the `idpbuilder create` command.

#### idpbuilder workshop installation

Since we are going to use a fork version of the sources, be sure to run the next command to perform the initial installation.

```bash
idpbuilder create --use-path-routing \
  --package https://github.com/cloudstation-dev/stacks//ref-implementation
```

This should take ~6 minutes for everything to come up. All the container images are pulled, be sure to have a good internet connection.

To track the progress of the components, you can go to the ArgoCD UI.
The first effective credentials to get will be the ArgoCD UI.

Let's see what was installed?
- Argo Workflows to enable workflow orchestrations.
- Backstage as the UI for software catalog and templating. Source is available here.
- External Secrets to generate secrets and coordinate secrets between applications.
- Keycloak as the identity provider for applications.
- Spark Operator to demonstrate an example Spark workload through Backstage.

Packages from the argo UI can be removed by deleting the ArgoCD Application corresponding file.
For example, if you want to remove Spark Operator, you can delete the file from the folder, or also delete it from the ArgoCD UI.

Consider that the only package that should never be removed is Keycloak, because the credentials of backstage are managed inside of it.

#### Accessing UIs

To access the components UI endpoints, consider the next list:

- Argo CD: https://cnoe.localtest.me:8443/argocd
- Argo Workflows: https://cnoe.localtest.me:8443/argo-workflows
- Backstage: https://cnoe.localtest.me:8443/
- Gitea: https://cnoe.localtest.me:8443/gitea
- Keycloak: https://cnoe.localtest.me:8443/keycloak/admin/master/console/

### Gathering the secrets of the components

All the secrets created by the idpbuilder installation can be discovered running

```bash
idpbuilder get secrets
```

### Checking installed packages

To list the packages installed and details, idpbuilder provides a subcommand as following

```bash
idpbuilder get packages
```



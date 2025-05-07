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

### Option 1: Using Bash Script

You can execute the following bash script to get started with a running version of the idpBuilder (inspect the script first if you have concerns):

```
curl -fsSL https://raw.githubusercontent.com/cnoe-io/idpbuilder/main/hack/install.sh | bash
```

verify a successful installation by running the following command and inspecting the output for the right version:

```
idpbuilder version
```

### Option 2: Manual installation

You can run the following commands for a manual installation:

```
version=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/cnoe-io/idpbuilder/releases/latest)
version=${version##*/}
curl -L -o ./idpbuilder.tar.gz "https://github.com/cnoe-io/idpbuilder/releases/download/${version}/idpbuilder-$(uname | awk '{print tolower($0)}')-$(uname -m | sed 's/x86_64/amd64/').tar.gz"
tar xzf idpbuilder.tar.gz
```

```./idpbuilder version```



### Option 3: Release page binary

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


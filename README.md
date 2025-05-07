# idpbuilder-workshop

## Prerequisites
Ensure you have the following tools installed on your computer.

Required

- idpbuilder: version 0.3.0 or later
- kubectl: version 1.27 or later
-  Have a k8s cluster or kind tool installed.
  
Your computer should have at least 6 GB RAM allocated to Docker.

## Setup

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


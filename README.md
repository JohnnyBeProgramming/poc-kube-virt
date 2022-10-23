# Run a windows VM inside of a Kubernetes cluster

This POC was inspired by these online resources:
 - https://medium.com/adessoturkey/create-a-windows-vm-in-kubernetes-using-kubevirt-b5f54fb10ffd
 - https://youtu.be/oO8VEmpojz0 

# Use Kind

I had to use Kind, as my local docker-desktop did not work on the mac.

You can install a kind cluster like this:

```
brew install kind
kind create cluster 
```

Dont forget to switch your kube context before you start...

# How to run VM's locally

In order to run a VM, we will first have to pull a VM base image and install the required CRD's:

```bash
# Install required CRD's for KubeVirt
./scripts/install-kubevirt
./scripts/install-data-importer

# Download the client CLI to load it and sample ISO image
./scripts/download-kubevirt-cli
./scripts/download-iso-image  

# Upload the downloaded ISO image into the cluster
./scripts/expose-upload-proxy
./scripts/upload-iso-image

# Create, start and connect to the VM
./scripts/create-vm
./scripts/connect-vm
```

## Connect to the VM via a web browser

Then access the console via http://{public-ip-of-ubuntu-vm}:31002

```bash
# Install the virtual VNC server in the cluster
./scripts/install-virtvnc

# Resolve to public IP and try and connect via the browser
PUBLIC_IP=$(dig -4 TXT +short o-o.myaddr.l.google.com @ns1.google.com)
VMVNC_URL="http://${PUBLIC_IP}:31002"
echo "Connecting to: $VMVNC_URL"
open $VMVNC_URL
```
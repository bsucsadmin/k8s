# k8s
This repo documents the Kubernetes Cluster deployed through Proxmox for BSU CS

Below is a simple diagram showing how the cluster works:
![diagram](https://github.com/bsucsadmin/k8s/blob/main/docs/diagram.png?raw=true)
### Installing Kubernetes
Use this [guide](https://www.cherryservers.com/blog/install-kubernetes-ubuntu) to install your Kubernetes cluster
- When adding worker nodes you can use the template on `csci-virt-03` to clone new workers and join them to the cluster instead of a full setup
- **DO NOT INSTALL CALICO!!** The guide instructs you to install Calico, however we will be using Flannel instead because Calico does not work with what we are going to do

### Installing Flannel
```bash
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
You can join your nodes to the cluster after this step

### Setting up NFS
Use the `nfs/01-setup-nfs-provisioner.yaml` file
```bash
kubectl create -f 01-setup-nfs-provisioner.yaml
```

### Installing multus-cni
```bash
kubectl apply -f https://raw.githubusercontent.com/k8snetworkplumbingwg/multus-cni/master/deployments/multus-daemonset-thick.yml
```
For multus to work with dhcp you need to run the dhcp daemonset which doesn't run automatically for some reason? If you are using the template in virt-03, the service for it has already been created and it should be running. If you aren't using the template you can find the systemd service and socket files [here](https://github.com/containernetworking/plugins/tree/main/plugins/ipam/dhcp/systemd)

### Setting up the networking
Use the `net/dhcp-net.yaml` and `net/flannel-net.yaml`
```bash
kubectl create -f net/dhcp-net.yaml
```
```bash
kubectl create -f net/flannel-net.yaml
```

### Conclusion
That should be the default k8s setup that uses the university DHCP. If you have any questions reach out to [Haren](mailto:haren.eshwaran@bsu.edu) :)

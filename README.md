# Build a Kubernetes cluster using Kubeadm via Ansible

## K8s Ansible Playbook

Build a Kubernetes cluster using Ansible with kubeadmin. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] Raspbian
- [X] CentOS Stream 8+
- [X] RHEL 8+
- [X] Rocky Linux 8+

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.16.0+
All nodes should have Python3 installed
controllers and workers must have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/
```

Second, edit `inventory/hosts.ini` to match the system information gathered above. For example:

```bash
[controller]
192.168.1.10

[worker]
192.168.1.11
192.168.1.12

[balancer]
192.168.1.13

[k3s_cluster:children]
controller
worker
```

If multiple hosts are in the controller group, the playbook will automatically set up k8s in HA using kubeadm.

This cluster was designed to use an external NGINX load balancer for the Control Plane API.  For single controller deployments the balancer IP can be left blank.

This requires at least k8s version '1.24.1' however the version is configurable by using the 'k8s_version' variable.

If needed, you can also edit 'inventory/group_vars/all.yml' to match your environment.

### Create Cluster

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/hosts.ini
```

After deployment control plane will be accessible via virtual ip-address which is defined in inventory/group_vars/all.yml as `apiserver_endpoint`

### Remove k8s cluster

```bash
ansible-playbook reset.yml -i inventory/hosts.ini
```

>You should also reboot these nodes 

## Thanks  

Thanks to Angus for testing on serveral OS's and helping identify bugs!
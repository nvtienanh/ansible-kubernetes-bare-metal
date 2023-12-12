<h1 align="center">
    Ansible-Kubernetes Repository
</h1>

<p align="center">
    Automate the provisioning of a new bare-metal multi-node Kubernetes cluster with Ansible.
    Uses all the industry-standard tools for an enterprise-grade cluster.
</p>

## Table of Contents

- **[Stack](#stack)**
- **[Requirements](#requirements)**
- **[Usage](#usage)**

## Stack

- **Ansible**: an open source IT automation engine.
- **ContainerD**: an industry-standard container runtime.
- **Kubernetes**: an open-source system for automating deployment, scaling, and management of containerized applications.
- **Calico**: an open source networking and network security solution for containers (CNI).
- **MetalLB**: a bare metal load-balancer for Kubernetes.
- **Nginx**: an Ingress controller.
- **Dashboard**: a web-based Kubernetes user interface.

## Requirements

1. A Linux machine with a superuser privileges and pre-installed Ansible.
2. Ubuntu machines that are intended to become part of the new Kubernetes cluster.
   Make sure that your SSH key is already installed on the machines by running the following command:
```
$ ssh-copy-id <The remote username>@<The IPv4 address of the remote machine>
```

## Usage

1. Clone this Git repository to your local working station:
```
$ git clone https://github.com/ArielLahiany/kubernetes.git
```

2. Change directory to the root directory of the project:
```
$ cd kubernetes
```

3. Edit the values of the default variables to your requirements:
```
$ vim defaults/main.yaml
```

4. Edit the Ansible inventory file to your requirements:
```
$ vim inventory/hosts.ini
```

5. Edit the Ansible Vault variables file to your requirements:
```
$ vim vault/main.yaml
```

6. Use Ansible Vault command line to encrypt the Vault variables file:
```
$ ansible-vault encrypt vault/main.yaml
```

7. Run the Ansible Playbook:
```
$ ansible-playbook -i inventory/hosts.ini -K playbooks/cluster.yaml
```

8. Get the IPv4 address of the deployed Nginx ingress controller:
```
$ kubectl get services --all-namespaces
```

| NAMESPACE     | NAME                     | TYPE         | CLUSTER-IP   | EXTERNAL-IP  | PORT(S)                    | AGE  |
|---------------|--------------------------|--------------|--------------|--------------|----------------------------|------|
| ingress-nginx | ingress-nginx-controller | LoadBalancer | 10.96.91.254 | IPv4 address | 80:31478/TCP,443:31633/TCP | 149m | 

9. Edit the hosts file on your local working station to include the path to the dashboard:
```
$ sudo vim /etc/hosts
```
```
<IPv4 address of the Nginx ingress controller> <dashboard.prefix>.<cluster.domain>

```

For example:
```
192.168.14.195 dashboard.cluster.local
```

10. During the deployment process the Ansible Playbook is generating a new admin token for the dashboard. Get it by running the following command:
```
$ cat /home/<Your username>/.kube/dashboard
```

11. Browse to the address you've just added to the hosts file.
12. Supply the created admin token in order of login into the dashboard.


ansible-playbook -i inventory/hosts.ini playbooks/cluster.yaml
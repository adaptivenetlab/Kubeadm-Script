# Install Kubernetes Cluster using kubeadm script
setup Kubernetes Version 1.21.0 cluster on CentOS 7.

# Cluster
1 master node (IP = 192.168.100.208)

1 worker node (IP = 192.168.100.209)

## On both master and worker
```bash
sudo su
yum -y update
yum -y install git
```

## Edit hostname on master node and change the name to master
```bash
sudo su
vi /etc/hostname
```

## Edit hostname on worker node and change the name to worker
```bash
sudo su
vi /etc/hostname
```

## Add known host on master node and worker node (Use your IP)
```bash
vi /etc/hosts
```
## Add this to /etc/hosts of master node and worker node
```bash
192.168.100.208 master
192.168.100.209 worker
```

## Make the system to sudo without password so when script is running, the script is not asking for the password (master node)
```bash
sudo su
sudo visudo
```
## Scroll down to the end of the file and add the following line that will allow the user “centos” to run any command with sudo without being asked for a password (master node):
```bash
centos  ALL=(ALL) NOPASSWD:ALL
```
## Clone the repository on master node and worker node
```bash
sudo su
git clone https://github.com/daus2936/kubeadm-script.git
```
## Give execute permission to the script file on master node and worker node
```bash
cd kubeadm-script
chmod +x ccluster.sh kubernetes.sh variable.sh docker.sh
```

## Edit the variable,change the the variable of MASTER_IP to your master node IP address,and make sure to change the other variable (Master node)
```bash
sudo su
vi variable.sh
```

## Install docker with docker.sh (Run it as root) (master node and worker node)
```bash
sudo su
./docker.sh
```

## After the script of docker.sh is finish,login to docker hub (master node and worker node)
```bash
docker login
```

## After login to dockerhub,run kubernetes.sh script (Run it as root) (master node and worker node)
```bash
sudo su
./kubernetes.sh
```

## After the script of kubernetes.sh is finish,create kubernetes cluster,run ccluster.sh on master node (Run it as root) (master node)
```bash
sudo su
./ccluster.sh
```

## To test if the cluster is installed (master node) :
```bash
kubectl get nodes
```
## Use this command so the worker node can join the cluster,the token will be different in your cluster (use your token),type this on worker node:
```bash
kubeadm join 192.168.100.208:6443 --token 4niztf.rc6y0xfbnih81jxv \
        --discovery-token-ca-cert-hash sha256:b44d3ac51430f7e7dad4639e4a8eeb5f3f03769ed8319c26e437ff49915659d7
```



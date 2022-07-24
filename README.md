# Kubernetes-adm-Installation
# Steps 1 through 6 should be done on all Servers
## 1 - SSH on each Servers ( master and workers)
## 2 - Install Docker on each server.
https://github.com/devopstrainingschool/docker-installation
## 3 - update, install apt-transport-https and curl packages
```
apt-get update && sudo apt-get install -y apt-transport-https curl
```
## 4 -  Download and add a trusted key
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
```
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

## 5 - update the system again for a second time
```
apt-get update
```

##  6 -  Install kubelet, kubeadm and kubectl
```
apt-get install -y kubelet kubeadm kubectl
```
# step 7 to step should be done on the Master Node only
### 7- create the cluster and generate the token to join the cluster
 
```
kubeadm init --apiserver-advertise-address=(Master Private IP Address here) 
--pod-network-cidr=192.168.0.0/16 --ignore-preflight-errors=NumCPU 
```
![image](https://user-images.githubusercontent.com/107158398/180663038-b5884eee-a61c-441e-b908-ec81d68e5be7.png)
 ### for example using the above picture , we will have
 ### kubeadm init --apiserver-advertise-address= 172.31.46.238
###--pod-network-cidr=192.168.0.0/16 --ignore-preflight-errors=NumCPU 
## The previous command will generate a t

⬡ This will generate a token to use on the nodes in order to join the cluster

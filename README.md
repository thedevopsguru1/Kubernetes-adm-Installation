# Setting up three node Kubernetes cluster
## First of all, we should have three instances created that can connect over the public network. It doesn't matter how those instances are created, for example, they can either be Digital Ocean droplets or AWS EC2 instances. Linux Ubuntu 18.04
# Prerequist: Make sure docker is running on all instances.
## Install Docker: 
https://docs.docker.com/engine/install/ubuntu/
## 1- SSH into all the instances
Once you are into those instances, the commands that are mentioned below should be run on all the instances

## Commands to run on all the nodes
### Get sudo working
```
sudo -i
```
### update packages and their version
```
sudo apt-get update && sudo apt-get upgrade -y
```

### install curl and apt-transport-https
```
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
```

### add key to verify releases

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
#### You should see OK outpot
![image](https://user-images.githubusercontent.com/107158398/221389312-d536b41a-7167-49d1-8262-393ee8697c81.png)

### add kubernetes apt repo
```
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

### install kubelet, kubeadm and kubectl
```
sudo apt-get update
```
```
sudo apt-get install -y kubelet kubeadm kubectl
```

### apt-mark hold is used so that these packages will not be updated/removed automatically
```
sudo apt-mark hold kubelet kubeadm kubectl
```
After the above commands are successfully run on all the worker nodes. Below steps can be followed to initialize the Kubernetes cluster.

# On Master Node
### Run the below command on the node that you want to make the leader node. Please make sure you replace the correct IP of the node with Private IP-of-Node
```
export MASTER_IP=<Private-IP-of-Node>
```
#### Put the Private ip Adress
![image](https://user-images.githubusercontent.com/107158398/180680492-c353019b-d75a-4518-9e64-9914e3471563.png)

## Let us Install our cluster
```
kubeadm init --apiserver-advertise-address=${MASTER_IP} --pod-network-cidr=10.244.0.0/16
```
## the output should be similar to this
![image](https://user-images.githubusercontent.com/107158398/180681465-c0013222-a2e0-4594-b8ff-78243b22d7a2.png)
### If you have docker error like this one
![image](https://user-images.githubusercontent.com/107158398/180901753-dca22550-2a72-4223-87ff-8f97fafa08c3.png)
### run these commands:
```
rm -rf /etc/containerd/config.toml
```
```
systemctl restart containerd
```

#### Setting up Kubeconfig file
#### After successful completion of kubeadm init command, like we got the kubeadm join command, we would also get details about how we can set up kubeconfig file.
![image](https://user-images.githubusercontent.com/107158398/180835327-eb520b39-4df1-4754-92b6-53f3790694c7.png)
#### Run this command to setup kubeconfig as a root user
```
export KUBECONFIG=/etc/kubernetes/admin.conf
 ```
# Join worker nodes to the Master node
### Once the command kubeadm init is completed on the Master node, below we would get a command like below in the output of kubeadm init that can be run on worker nodes to make them join the master node. Copy and paste it on all worker nodes. Also make sure yur save that token somewhere save

![image](https://user-images.githubusercontent.com/107158398/180681523-06a01af8-0ad9-43bf-93b9-89f4bf2c6291.png)
### if you run into an issue while running the join command,
![image](https://user-images.githubusercontent.com/107158398/180902268-71bf3b06-c5aa-494b-a558-1dc9a8fa5306.png)

### please run these commands before rerun join command:
```
sudo systemctl unmask docker
```
```
systemctl start docker
```
### make sure docker is running 
```
docker version
```
### run the join command again
![image](https://user-images.githubusercontent.com/107158398/180681523-06a01af8-0ad9-43bf-93b9-89f4bf2c6291.png)



# Install CNI plugin on the Master node
### The below command can be run on the leader node to install the CNI plugin
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
### Let test and make sure all good.
```
kubectl get nodes
```
#### The output should look like this:

```
kubectl get pod -A
```
#### The Above command output should look like this:




## If you want to add another server to the cluster later, you can run these commands:
### - List all the tokens
```
kubeadm token list
```
### - create a token
```
kubeadm token create --print-join-command
```
### Copy the output of the previous command and paste it



## kubectl run my-app --image=devopstrainingschool/java-maven-jenkins --port=8080
### kubectl expose pod my-app --type=NodePort --port=8080 --target-port=8080
### kubectl get svc -A
### http://3.139.76.41:30540/




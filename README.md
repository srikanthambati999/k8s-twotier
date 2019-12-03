# Build Project Using Maven

Maven is java based build tool to generate executable 

packages(jar, ear,war) for java based projects.

```bash
mvn clean package
```

## Create Docker Image
Docker is a continerization tool.Using docker we can deploy our applications as 

containers using docker images. Containers contains application code and also the softwares,

config files whatever is required for our application to run.

Create docker image using Dockerfile


```docker
docker build -t dockerhandson/spring-boot-mongo .
```

## Deploy Application Using Docker Compose 

```docker-compose 
docker-compose up -d 
```

## List Docker Containers
```docker
docker ps -a
```





## =======common for master& slave stage

sudo apt-get update -y
sudo apt-get install -y apt-transport-https
sudo su -

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

## adding kubernetes to sources
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update -y
##  disabling swap memory
swapoff -a                                    
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

## enabling the ip tables   bcoz master and cluster machine talks pods to pods communication

modprobe br_netfilter                                       
sysctl -p                                          
sudo sysctl net.bridge.bridge-nf-call-iptables=

## docker installation

apt install docker.io -y
usermod -aG docker ubuntu

systemctl restart docker
systemctl enable docker.service


apt-get install -y kubelet kubeadm kubectl kubernetes-cni  
## enable kubernetes related modules

systemctl daemon-reload
systemctl start kubelet
systemctl enable kubelet.service

## =========common for master and slaves end



## execute below commands only in master


## ==================in master node==========

## execute below command as root user

kubeadm init

#exit root user & execute as normal user

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

-----------after this run 

kubectl get nodes  =====here status will be notready

it will not comeup it need dependency required cni(container network dependency)  

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

kubectl get nodes

kubectl get pods --all-namespaces

#get token

kubeadm token create --print-join-command


================================================

sudo kubeadm join 172.31.21.250:6443 --token 20yx95.hag5b2d8dep60nlj \
    --discovery-token-ca-cert-hash sha256:44d1ce9de6bde0511302c0a6c3d274e76cb084606ed4061653d5b3d740ed7507








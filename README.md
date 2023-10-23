# Building and Deploying a Flask application in Kubernetes

## EC2 Creation Steps
It is recommended that your EC2 contains the below specifications:

### Memory:
- Minimum: A good starting point for memory is around 4GB to 8GB per node. This is suitable for small applications and testing purposes.
- Medium: For production workloads, especially those running multiple containers or services, you might want to consider instances with 16GB to 32GB of memory.
- Large: Memory-intensive applications, databases, or services might require instances with 64GB or more of memory.

### CPU:
- Minimum: For basic testing and smaller applications, instances with 1 vCPU can suffice.
- Medium: Production workloads with a moderate level of CPU usage might benefit from instances with 2 to 4 vCPUs.
- Large: For CPU-intensive applications, consider instances with 8 or more vCPUs.

## Initialize EC2
On your EC2, run the below commands.

### Git
Run:
1. Install Git : ```sudo yum install -y git```
2. Clone the repository (HTTPS) to your EC2 : ```git clone <URL>```

### Docker
Run:
1. Install Docker : ```sudo yum install -y docker```
2. Start Docker : ```sudo systemctl start docker```
3. Enable Docker : ```sudo systemctl enable docker```

#### Docker Build Image
Run:
1. Build the image with the image name you want accordingly : ```docker build -t luqman-flask-app:latest .```
2. Tag the image to the AWS ECR repository : ```docker tag luqman-flask-app:latest <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/luqman-flask-app:latest```

#### Docker Push Image
Run:
1. Push the image to the AWS ECR repository : ```docker push <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/luqman-flask-app:latest```

### Kubernetes
Run:
1. Install Kubernetes
```
sudo tee /etc/yum.repos.d/kubernetes.repo <<EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
sudo yum install -y kubelet kubeadm kubectl
```
2. Start kubelet:
```
sudo systemctl start kubelet
sudo systemctl enable kubelet
```
3. Initialize Kubernetes Cluster : ```sudo kubeadm init```
4. Configure kubectl :
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
5. Verify Kubernetes nodes are working: ```kubectl get nodes```

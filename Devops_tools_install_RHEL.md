# Devops Tools installation on RHEL

## AWS CLI:
Run below commands to install aws cli on RHEL
```
sudo yum update
sudo yum install tree unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --versionpython --version
sudo yum install gnupg*
```

## Docker:
Run below commands to install docker
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl start docker
sudo docker --version
sudo docker compose version
```
Run below commands to delete all conatiners at once
```
sudo docker stop $(sudo docker ps -aq)
```
## Kubectl:
Run below commands to install kubectl on RHEL.

Useful Link: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```

## Helm: 
Run below commands to install Helm.

Useful Link: https://helm.sh/docs/intro/install/
```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
$ helm version
```
## Terraform:
Run below commands to install Terraform.

Useful Link: https://developer.hashicorp.com/terraform/install
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
```

## Java:
Run below commands to install java.

```
sudo yum update -y
sudo yum install java-11-openjdk-devel -y
java -version
```
## Jenkins:
Run below commands to install jenkins. Java is mandatory for jenkins. Run above command to install java if not installed already.

Jenkins uses a GPG key to sign its packages. Import the key with:
```
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```
Add the Jenkins Repository:
```
sudo tee /etc/yum.repos.d/jenkins.repo <<EOF
[jenkins]
name=Jenkins-stable
baseurl=https://pkg.jenkins.io/redhat-stable/
gpgcheck=1
gpgkey=https://pkg.jenkins.io/redhat/jenkins.io.key
EOF
```
Install Jenkins
```
sudo yum install jenkins -y
```
Start and Enable Jenkins
```
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
Verify that Jenkins is running
```
sudo systemctl status jenkins
```
**Access Jenkins:**
Open a web browser and navigate to http://<your-server-ip>:8080. You should see the Jenkins setup wizard.

**Unlock Jenkins**
To complete the setup, Jenkins requires an unlock key. Retrieve the unlock key from the log file:
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Copy the key from the terminal and paste it into the setup wizard. Install Suggested Plugins and Finish setup

## EKSCTL:
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

## Metrics server in k8s cluster:

The Metrics Server is available as a Kubernetes manifest from the official repository. You can apply it with the following command:
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
Verify the Installation:
```
kubectl get pods -n kube-system
```

Verify Node and Pod Metrics
```
kubectl top nodes
kubectl top pods
```

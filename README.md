End-to-End DevOps Deployment

This guide outlines a full DevOps pipeline using **Terraform**, **Ansible**, **Jenkins**, **Docker**, **SonarQube**, **Kubernetes (EKS)**, **Prometheus**, and **Grafana** on AWS.

---

## 📌 1. Infrastructure Setup with Terraform

### Steps:

* Create VPC, Subnet, IGW (Internet Gateway), and Route Table
* Launch EC2 instances

### Terraform Setup:

* Create a folder `terraform/`
* Add file: `vpc-and-ec2.tf`

### Commands:

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

### Requirements:

* Install Terraform and AWS CLI
* Configure AWS credentials:

```bash
aws configure
```

---

## 🔧 2. Configuration Management with Ansible

### Steps:

* Launch EC2 for Ansible
* Install Ansible:

```bash
sudo apt update && sudo apt install ansible
```

* Update `/etc/ansible/hosts` with Jenkins IP
* Verify with:

```bash
ansible jenkins -m ping
```

### Playbook:

* Create folder `ansible/`
* File: `jenkins-playbook.yaml`
* Run playbook:

```bash
ansible-playbook -i /etc/ansible/hosts jenkins-playbook.yaml
```

---

## 📊 3. Jenkins Setup

### Steps:

* Install Java & Jenkins
* Access Jenkins: `http://<Public-IP>:8080`
* Create pipeline using `Jenkinsfile`
* Example App: `cfbook/app.py`

---

## 🔍 4. SonarQube Integration

### Setup with Docker:

```bash
sudo apt install docker.io
sudo docker pull sonarqube:lts-community
sudo docker run --name sonar -p 9000:9000 sonarqube:lts-community
```

* Access: `http://<IP>:9000`
* Initial login: admin/admin

### Jenkins Integration:

* Install **SonarQube Scanner** plugin
* Add SonarQube server in Jenkins system config
* Create token in SonarQube and add to Jenkins credentials
* Add webhook from SonarQube to Jenkins

---

## 🚧 5. Docker Hub Integration

### Steps:

* Create personal access token in Docker Hub
* Add credentials in Jenkins:

  * ID: `dockerdata`
  * Username & Token

---

## 🚀 6. Kubernetes (EKS) Deployment

### Requirements:

* Install: `eksctl`, `kubectl`, AWS CLI
* Configure AWS credentials
* Create IAM user and keys

### Commands:

```bash
eksctl create cluster --name my-cluster --region us-west-2
kubectl get nodes
```

### Deployment:

* Create `deployment.yaml`
* Apply with:

```bash
kubectl apply -f deployment.yaml
```

---

## 📊 7. Monitoring with Prometheus & Grafana

### Setup with Helm:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```

### Check Services:

```bash
kubectl get svc -n monitoring
```

### Access:

* Port forward: `kubectl port-forward svc/prometheus -n monitoring 9090:9090`
* Open: `http://<public-ip>:9090`

---

## 📁 Project Structure

```
learbay-devops-project/
├── terraform/
│   └── vpc-and-ec2.tf
├── ansible/
│   ├── hosts
│   └── jenkins-playbook.yaml
├── jenkins/
│   └── Jenkinsfile
├── sonar/
│   └── docker-sonar-setup.sh
├── kubernetes/
│   └── deployment.yaml
├── monitoring/
│   ├── prometheus-helm.md
│   └── grafana-helm.md
├── README.md
└── .gitignore
```

---

## 📚 Notes

* Use `apt` to install packages
* Use `nano` to edit files
* Use `chmod 600 <file>` for key files
* Use Helm as Kubernetes package manager

---

## 📃 File Formats

| Tool       | Format         |
| ---------- | -------------- |
| Terraform  | `.tf`, `.json` |
| Ansible    | `.yaml`        |
| Jenkins    | `Jenkinsfile`  |
| Kubernetes | `.yaml`        |

---

## 🚀 Authors & Contributors

Built with guidance from Learnbay classes and documentation. Uses community best practices.

---

## 🚮 License

[MIT License](LICENSE)


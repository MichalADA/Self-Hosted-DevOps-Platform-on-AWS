# Self-Hosted-DevOps-Platform-on-AWS

Project Documentation: Self-Hosted DevOps Platform on AWS
🔥 Project Title
Production-Grade DevOps & Platform Engineering Lab on AWS EC2

🎯 Purpose & Vision
This project aims to build a full-featured, production-grade DevOps/Platform Engineering environment in AWS using self-managed components on EC2 instances — without relying on managed services like EKS, RDS, or Route53. It replicates how modern infrastructure is built in real-world organizations but is optimized for cost-efficiency and full manual control.

This setup will serve:

as a learning platform for advanced DevOps/Cloud skills,

as a demo/proof-of-concept for personal branding or portfolio,

and as a realistic environment to test integrations and best practices.

🧱 Infrastructure Overview
🌐 Cloud Environment
Cloud provider: AWS

Provisioning: Terraform

Networking:

Custom VPC (for internal access only)

Public + private subnets

Internet Gateway for outbound traffic

NAT Gateway (optional)

Bastion Host or IP-restricted access

Security: Tight Security Groups, access only from my IP

Instances:

Ubuntu EC2 instances for Kubernetes + services

Windows Server EC2 for Active Directory

☸ Kubernetes Architecture
Cluster 1: rancher
Single-node RKE2 cluster

Hosts Rancher GUI for cluster management

Installed via Helm

Runs in a dedicated VM (rancher-node)

Cluster 2: k8sdev
RKE2 cluster with:

1 master node

2 worker nodes

Imported into Rancher for full management

🔐 Active Directory Setup
Windows Server 2019/2022 EC2 instance

Promoted to Domain Controller

Internal domain: corp.local or ad.local

DNS configured (Windows DNS or BIND fallback)

Optional Certificate Authority (AD CS)

AD used for:

Rancher LDAP login

GitLab + Harbor LDAP integration

Future RBAC policies for Kubernetes

📡 Internal DNS Options
Primary: Windows DNS integrated with AD

Alternative: BIND on Linux

Provides internal FQDNs:

rancher.ad.local

harbor.corp.local

gitlab.corp.local, etc.

🧰 Platform Stack Components
Component	Purpose
GitLab CE	Git hosting + CI/CD + runners
Jenkins	Optional CI engine for flexibility
ArgoCD	GitOps engine – sync from Git to K8s
Harbor	Private Docker container registry
Kafka	Distributed message queue
Prometheus	Metrics collection
Grafana	Visualization of metrics
AlertManager	Alerting engine
OpenSearch	Centralized logging (ELK alternative)
Rancher	Kubernetes GUI management + AD auth

🌐 Ingress and Load Balancing
VPS-based NGINX reverse proxy (outside AWS)

Will serve as public-facing Load Balancer

Handles SSL termination

Forwards traffic to EC2-hosted services (e.g., Rancher, GitLab)

NGINX Ingress Controller in rancher cluster

Deployed via Helm

Manages internal ingress routes

Configured with internal DNS

🔧 CI/CD and GitOps Flow
GitLab → CI/CD pipelines build + push images to Harbor

Jenkins (optional) for advanced workflows

ArgoCD pulls manifests from Git and syncs to k8sdev cluster

Apps deployed as Helm charts or Kustomize

🧪 Monitoring & Logging
Prometheus Operator for metrics

Grafana with dashboards for:

Node usage

K8s pod health

Kafka lag

OpenSearch + Dashboards for log aggregation

AlertManager configured with email/slack/webhook routing

🔐 Access Control & Security
All components accessible only via internal DNS or ingress

Public access limited to:

VPS Load Balancer

Specific ports/IPs via SG

Rancher, GitLab, Harbor, Jenkins secured with LDAP (via AD)

RBAC policies controlled from AD groups

📦 Additional Plans
Deploy certificate chain from AD CS to internal services

Automate DNS record creation (possibly with ExternalDNS)

Add backup policies for Harbor + GitLab volumes

Explore external access via WireGuard tunnel or bastion

Optionally monitor AD metrics from Prometheus via WMI exporter

📝 Blog Series Roadmap
Terraform: AWS EC2 + SG + VPC (minimum budget)

Building two RKE2 clusters from scratch

Installing Rancher with self-signed or AD-issued cert

AD on Windows EC2: full domain + DNS setup

Deploying GitLab, Jenkins, Harbor in Kubernetes

Enabling GitOps with ArgoCD and internal DNS

Full Observability: Prometheus, Grafana, OpenSearch, Alerts

Integrating Rancher and GitLab with AD

Exposing services securely with NGINX (VPS LB)

Final thoughts + lessons learned

👤 Author
This is a personal infrastructure lab project created to deepen my DevOps skills and simulate production-level platform engineering practices. Everything is self-managed and designed to reflect real-world environments without relying on cloud-managed services.

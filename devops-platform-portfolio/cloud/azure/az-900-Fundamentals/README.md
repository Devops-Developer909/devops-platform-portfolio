| Domain                                     | Weight | Topics                                                |
| ------------------------------------------ | ------ | ----------------------------------------------------- |
| Cloud Concepts                             | 15–20% | IaaS / PaaS / SaaS, Public vs Private vs Hybrid Cloud |
| Core Azure Services                        | 30–35% | VMs, Storage, Databases, Networking, AKS basics       |
| Core Solutions & Management Tools          | 10–15% | Azure Portal, CLI, PowerShell, ARM templates          |
| General Security & Network Security        | 10–15% | NSG, Firewall, Azure Security Center, RBAC basics     |
| Identity, Governance, Privacy & Compliance | 20–25% | Azure AD, Policies, Blueprints, GDPR, SLA             |
| Azure Pricing & Support                    | 10–15% | TCO, Pricing Calculator, SLA, Support Plans           |


🎯 Core Azure Services Preparation for AZ-900
Weight: 30–35% of exam
Goal: Understand services, create simple resources, and remember key concepts

1️⃣ Virtual Machines (VMs)
Key Concepts to Learn:

Azure VM types: Linux / Windows

VM sizes (CPU/RAM)

Availability sets & zones

Public vs private IP

Virtual network connection

Hands-On Lab:

Create a Linux VM in Azure portal using free account

Assign a public IP and connect via SSH

Explore VM size, OS disk, and network interface

Delete VM after lab

Tips for Exam:

Know difference between VMSS vs availability set

Understand high availability concepts

2️⃣ Storage Accounts
Key Concepts:

Blob storage, File storage, Queue storage, Table storage

Storage tiers: Hot, Cool, Archive

Access keys and shared access signatures

Hands-On Lab:

Create a storage account

Create Blob container and upload a file

Create File share and connect from VM

Explore access keys and policies

Exam Tip:

Be able to choose storage type based on use case (archival vs high performance)

3️⃣ Databases
Key Concepts:

Azure SQL Database

Azure Cosmos DB (NoSQL)

Difference between single database, elastic pool, managed instances

Hands-On Lab:

Create a SQL Database (basic tier)

Connect via Query editor in portal

Explore firewall rules & connection strings

Exam Tip:

Know which service is relational (SQL) vs NoSQL (Cosmos DB)

No deep SQL queries needed, just concepts

4️⃣ Networking Basics
Key Concepts:

VNets and subnets

Network Security Groups (NSG)

Load Balancers and public IPs

VPN Gateway (basic)

Hands-On Lab:

Create a VNet with 2 subnets

Attach your VM to subnet

Add NSG rules to allow SSH

Observe traffic flow

Exam Tip:

Understand VNets isolate resources

Difference between public IP and private IP

5️⃣ AKS (Azure Kubernetes Service) Basics
Key Concepts:

AKS = managed Kubernetes cluster

Nodes, pods, clusters

Scaling and high availability

Container orchestration basics (no deep Kubernetes knowledge needed for AZ-900)

Hands-On Lab (Optional for AZ-900, overview enough):

Go to Azure Portal → Create AKS Cluster

Review node pool, node count, version

Observe portal dashboard

Exam Tip:

Just know AKS is managed, containerized workloads

No deep commands or Kubernetes YAML needed

🔧 Recommended Practice Resources
Microsoft Learn (FREE)

Path: Explore Core Azure Services

YouTube / Video Labs

Search: “AZ-900 Core Azure Services labs”

Watch 10–15 min demo per service

Hands-On

Use free Azure account

Create minimal resources (VM, Storage, SQL, VNet)

Delete after practice to save cost

📝 Key Memorization Tips
Service	Exam Tip / Focus
VM	Availability sets vs VMSS, public/private IP
Storage	Blob vs File, Hot/Cool/Archive
Database	SQL vs Cosmos DB, managed service
Networking	VNets, Subnets, NSG, Load Balancer basics
AKS	Managed Kubernetes, cluster concept only

✅ Result:
After this, you will:

Recognize all core Azure services

Know when and why to use each

Have minimal hands-on experience

Be ready to answer ~30–35% of AZ-900 questions confidently

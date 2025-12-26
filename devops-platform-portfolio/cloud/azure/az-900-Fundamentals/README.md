| Domain                                     | Weight | Topics                                                |
| ------------------------------------------ | ------ | ----------------------------------------------------- |
| Cloud Concepts                             | 15–20% | IaaS / PaaS / SaaS, Public vs Private vs Hybrid Cloud |
| Core Azure Services                        | 30–35% | VMs, Storage, Databases, Networking, AKS basics       |
| Core Solutions & Management Tools          | 10–15% | Azure Portal, CLI, PowerShell, ARM templates          |
| General Security & Network Security        | 10–15% | NSG, Firewall, Azure Security Center, RBAC basics     |
| Identity, Governance, Privacy & Compliance | 20–25% | Azure AD, Policies, Blueprints, GDPR, SLA             |
| Azure Pricing & Support                    | 10–15% | TCO, Pricing Calculator, SLA, Support Plans           |


📊 AZ-900 Core Azure Services Preparation Table


| Service / Topic                           | Subtopics / Concepts                                                                                             | Hands-On Lab                                                                                                     | Exam Tips                                                                          | Status        |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------- |
| **Virtual Machines (VMs)**                | - VM types (Linux/Windows) <br> - VM sizes <br> - Availability sets & zones <br> - Public vs Private IP          | - Create Linux VM <br> - Assign public IP <br> - Connect via SSH <br> - Explore VM size, OS disk, NIC            | - Difference between VMSS vs availability set <br> - High availability concepts    | In Progress   |
| **Storage Accounts**                      | - Blob, File, Queue, Table <br> - Hot / Cool / Archive tiers <br> - Access keys & SAS                            | - Create Storage Account <br> - Upload files to Blob <br> - Create File share <br> - Explore access keys         | - Match storage type to use case (archive vs frequent access)                      | Not Started   |
| **Databases**                             | - Azure SQL Database <br> - Cosmos DB <br> - Single DB vs Elastic Pool vs Managed Instance                       | - Create SQL Database <br> - Connect via Query editor <br> - Configure firewall rules                            | - Know relational (SQL) vs NoSQL (Cosmos DB) <br> - Focus on concepts, not queries | Not Started   |
| **Networking Basics**                     | - VNets & subnets <br> - Network Security Groups (NSG) <br> - Load Balancer basics <br> - VPN Gateway (overview) | - Create VNet with 2 subnets <br> - Attach VM to subnet <br> - Add NSG rules for SSH <br> - Observe traffic flow | - Understand VNets isolate resources <br> - Public vs private IP                   | In Progress   |
| **Azure Kubernetes Service (AKS) Basics** | - Managed Kubernetes <br> - Cluster, Node, Pod concepts <br> - Scaling & high availability                       | - Create AKS cluster in portal <br> - Review node pool, node count <br> - Observe portal dashboard               | - AKS = managed container service <br> - No YAML / kubectl needed for AZ-900       | Completed     |

# Virtual Networking (https://learn.microsoft.com/pdf?url=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Fnetworking%2Ffundamentals%2Ftoc.json)
1. VNets
2. Subnets
3. Peering
4. DNS
5. VPN Gateway
6. ExpressRoute
7. public/private endpoints
8. Ntework security groups
9. Load balancers basics

## VNets (Vitrual Networks)

## Azure Firewall 

- Azure Firewall is a managed, cloud-native network security service that controls and inspects traffic going in and out of your Azure virtual network (VNet).

- A central security gate that decides who can talk to whom, on which port, and to which destination.

🧱 Azure Firewall Core Components

| Component               | Purpose                                   |
| ----------------------- | ----------------------------------------- |
| **Firewall Rules**      | Allow or deny traffic                     |
| **Application Rules**   | Control HTTP/HTTPS traffic (FQDN based)   |
| **Network Rules**       | Control IP/Port based traffic             |
| **NAT Rules**           | Publish internal services to the internet |
| **Threat Intelligence** | Blocks known malicious IPs                |


Your PC → Firewall Public IP → DNAT → VM Private IP → SSH
  
  ### Route table : 
       - any address (0.0.0.0/0) -> Firewall private IP
        -  Subnets ->  VM subnets 
        - We create a route table (UDR) so that traffic is forced through the Azure Firewall instead of going directly to the internet.
#### Without a route table:
➡️ Azure Firewall is bypassed
➡️ No inspection, no filtering, no logging

What the route table does
A User Defined Route (UDR) tells Azure:

“Send this traffic to the Firewall first”

Example route:

cpp
Copy code
0.0.0.0/0 → Virtual appliance → <Firewall private IP>
This forces:

nginx
Copy code
VM → Azure Firewall → Internet
✅ Why this is REQUIRED for Azure Firewall
1️⃣ Security enforcement
Firewall rules apply only if traffic passes through it

Without UDR:

VM can reach internet directly

Firewall rules are ignored

2️⃣ Centralized control
One firewall for all subnets

One place to manage outbound rules

One place to log traffic

3️⃣ Compliance & auditing
Logs for all outbound traffic

Required for regulated environments (PCI, ISO)

4️⃣ Prevent firewall bypass
Without UDR:

Anyone could add a Public IP

Traffic bypasses security

📐 Typical Azure Firewall architecture
nginx
Copy code
VM Subnet
   |
   |  (UDR: 0.0.0.0/0 → Firewall)
   ↓
Azure Firewall
   |
Internet
  Firewal
    -> Firewall policy-> Firewall rule connnection
        DNAT -> any ip address (*) -> firewall public IP -> VM private IP


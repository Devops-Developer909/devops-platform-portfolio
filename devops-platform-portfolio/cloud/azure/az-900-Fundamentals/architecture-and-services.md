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

// ...existing code...
# Virtual Networking
(Reference: https://learn.microsoft.com/azure/networking/fundamentals)

1. VNets  
2. Subnets  
3. Peering  
4. DNS  
5. VPN Gateway  
6. ExpressRoute  
7. Public / private endpoints  
8. Network security groups  
9. Load balancers (basics)

## VNets (Virtual Networks)

## Azure Firewall

Azure Firewall is a managed, cloud-native network security service that controls and inspects traffic going in and out of your Azure virtual network (VNet). It acts as a central security gate that determines which traffic is allowed, on which ports, and to which destinations.

### Azure Firewall — Core Components

| Component               | Purpose                                   |
| ----------------------- | ----------------------------------------- |
| **Firewall Rules**      | Allow or deny traffic                     |
| **Application Rules**   | Control HTTP/HTTPS traffic (FQDN-based)   |
| **Network Rules**       | Control IP/Port-based traffic             |
| **NAT Rules**           | Publish internal services to the internet |
| **Threat Intelligence** | Blocks known malicious IPs                |

### Typical flow
Your PC → Firewall Public IP → DNAT → VM Private IP → SSH

### Route table (UDR)
To ensure traffic is inspected by Azure Firewall, create a User Defined Route (UDR) on the VM subnet that directs outbound traffic to the firewall's private IP.

- Route example:
  - Destination: 0.0.0.0/0
  - Next hop type: Virtual appliance
  - Next hop address: <Firewall private IP>

This forces:
VM → Azure Firewall → Internet

### Why UDR is required for Azure Firewall

1. Security enforcement  
   - Firewall rules apply only if traffic passes through the firewall.  
   - Without a UDR, VMs can reach the internet directly and firewall rules are ignored.

2. Centralized control  
   - One firewall can protect multiple subnets.  
   - Central place to manage outbound rules and logs.

3. Compliance & auditing  
   - Logs for outbound traffic are consolidated.  
   - Useful for regulated environments (PCI, ISO).

4. Prevent firewall bypass  
   - Without UDRs, a user could assign a Public IP to a VM and bypass the firewall.

### Example architecture (ASCII)

VM Subnet
  |
  | (UDR: 0.0.0.0/0 → Firewall private IP)
  ↓
Azure Firewall
  |
Internet

Firewall configuration:
- Firewall Policy → Firewall Rules / NAT Rules  
- DNAT example: Any IP (*) → Firewall public IP → VM private IP

// ...existing code...
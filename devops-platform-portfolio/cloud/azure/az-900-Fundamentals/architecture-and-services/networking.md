# Azure Networking

  ## Networking foundation :
   ### Azure networking foundation services are :

   1. Virtual Networks (VNet)
   2. Private Link 
   3. Azure DNS 
   4. Azure Bastion
   5. Route Server
   6. NAT Gateway
   7. Trafic Manager

### Virtual network
Azure Virtual Network (VNet) is the fundamental building block for your private network in
Azure.
#### You can use VNets to:
1. Communicate between Azure resources: You can deploy virtual machines, and several other types of Azure resources to a virtual network, such as Azure App Service Environments, the Azure Kubernetes Service (AKS), and Azure Virtual Machine Scale Sets.
1. Communicate between each other: You can connect virtual networks to each other,enabling resources in either virtual network to communicate with each other, using virtual network peering or Azure Virtual Network Manager. 
3. The virtual networks you connect can be in the same, or different, Azure regions. For more information, see Virtual network peering and Azure Virtual Network Manager.
4. Communicate to the internet: All resources in a virtual network can communicate outbound to the internet, by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer.
5. You can also use Public IP addresses or public Load Balancer to manage your outbound connections.
6. Communicate with on-premises networks: You can connect your on-premises computers and networks to a virtual network using VPN Gateway or ExpressRoute.
7. Encrypt traffic between resources: You can use Virtual network encryption to encrypt traffic between resources in a virtual network.
8. Network security groups : You can filter network traffic to and from Azure resources in an Azure virtual network with a network security group. 
9. Service endpoints : Virtual Network (VNet) service endpoints extend your virtual network private address space and the identity of your virtual network to the Azure services, over a direct connection.
10. Endpoints allow you to secure your critical Azure service resources to only your virtual networks. Traffic from your virtual network to the Azure service always remains on the Microsoft Azure backbone network
#### VNet
  Azure Virtual Network provides the fundamental building block for your private network in Azure. This service enables Azure resources like virtual machines (VMs) to securely communicate with each other, the internet, and on-premises networks. Virtual networks deliver the scale, availability, and *isolation benefits of Azure infrastructure*  while maintaining the familiar networking concepts you use in traditional datacenters.

  ![alt text](image.png)

  ##### Usegaes :

  1. Communication of Azure resources with the internet.
  2. Communication between Azure resources.
  3. Communication with on-premises resources.
  4. Filtering of network traffic.
  5. Routing of network traffic.
  6. Integration with Azure services.

##### Communicate with the internet

  1. All resources in a virtual network can communicate outbound with the internet, by default. 
  2. You can also use a public IP address, NAT gateway, or public load balancer to manage your outbound connections.
  3. You can communicate inbound with a resource by assigning a public IP address or a public load balancer.

 When you're using only an internal standard load balancer, outbound connectivity isn't available until you define how you want outbound connections to work with an instance-level public IP address or a public load balancer.

##### Communicate between Azure resources
 Azure resources communicate securely with each other in one of the following ways:

##### Virtual network:
 You can deploy VMs and other types of Azure resources in a virtual network. Examples of resources include App Service Environments, Azure Kubernetes Service (AKS), and Azure Virtual Machine Scale Sets. 
 
##### Note
 To move a virtual machine from one virtual network to another, you must delete and recreate the virtual machine in the new virtual network. The virtual machine's disks can be retained for use in the new virtual machine.

##### Virtual network service endpoint: 
 1. You can extend your virtual network's private address space and the identity of your virtual network to Azure service resources over a direct connection.
 2. Examples of resources include Azure Storage accounts and Azure SQL Database. 
 3. Service endpoints allow you to secure your critical Azure service resources to only a virtual network. 

##### Virtual network peering:
 You can connect virtual networks to each other by using virtual peering. The resources in either virtual network can then communicate with each other. The virtual networks that you connect can be in the same, or different, Azure regions. 
##### Communicate with on-premises resources
 You can connect your on-premises computers and networks to a virtual network by using any of the following options:

**Point-to-site virtual private network (VPN)**: 
 Established between a virtual network and a single computer in your network. Each computer that wants to establish connectivity with a virtual network must configure its connection. This connection type is useful if you're just getting started with Azure, or for developers, because it requires few or no changes to an existing network. The communication between your computer and a virtual network is sent through an encrypted tunnel over the internet. To learn more, see About point-to-site VPN.

**Site-to-site VPN**:
 Established between your on-premises VPN device and an Azure VPN gateway deployed in a virtual network. This connection type enables any on-premises resource that you authorize to access a virtual network. The communication between your on-premises VPN device and an Azure VPN gateway is sent through an encrypted tunnel over the internet. To learn more, see Site-to-site VPN.

***Azure ExpressRoute**:
 Established between your network and Azure, through an ExpressRoute partner. This connection is private. Traffic doesn't go over the internet. To learn more, see What is Azure ExpressRoute?.
# Azure Networking

## Overview
Azure networking provides the foundation for connecting Azure resources to each other, to the internet, and to on-premises networks. This document summarizes core services, design patterns, and practical commands for common networking tasks.

---

## Networking foundation
Core Azure networking services:

- Virtual Networks (VNet)
- Azure Private Link
- Azure DNS
- Azure Bastion
- Azure Route Server
- NAT Gateway
- Traffic Manager

---

## Virtual Networks (VNets)

Azure Virtual Network (VNet) is the fundamental building block for your private network in Azure.

### VNet use cases

- Host and isolate Azure resources (VMs, AKS, App Service Environments, VM Scale Sets).
- Connect resources across VNets using peering or Azure Virtual Network Manager.
- Connect to the internet (outbound by default) and expose resources with public IPs or Load Balancers.
- Connect to on-premises networks via VPN Gateway or ExpressRoute.
- Encrypt traffic between resources using virtual network encryption.
- Filter traffic with Network Security Groups (NSGs) and Application Security Groups (ASGs).
- Provide private connectivity to Azure services with service endpoints and Private Link.

### Communicate with the internet

- Outbound connectivity is allowed by default for resources in a VNet.
- Use Public IPs, NAT Gateway, or a public Load Balancer to manage outbound connectivity.
- For inbound connectivity, assign a Public IP or use a public Load Balancer.

Note: When using only an internal standard Load Balancer, outbound connectivity isn't available until you configure instance-level public IPs or a public Load Balancer.

### Communicate between Azure resources

- Deploy resources into the same VNet and subnets to enable private communication.
- Use VNet peering to connect VNets across regions or subscriptions.
- Use service endpoints or Private Link to secure Azure service access to only your VNets.

### Communicate with on-premises resources

Options:

- Point-to-site VPN — single computer to VNet (useful for dev/test and individual access).
- Site-to-site VPN — IPSec tunnel between on-prem VPN device and Azure VPN Gateway.
- ExpressRoute — private connection through a partner; traffic does not traverse the internet.

### Filter and route network traffic

- Network Security Groups (NSGs) and Application Security Groups (ASGs) for layer 3/4 access control.
- Network virtual appliances (NVAs) for firewalling, WAN optimization, etc.
- Route tables (UDRs) to override Azure's default routing per subnet.
- BGP route propagation when using VPN Gateway or ExpressRoute.

### Integrate Azure services

- Deploy dedicated instances into your VNet for private access.
- Use Azure Private Link for private, per-resource connectivity.
- Use service endpoints to secure service resources to your VNets while keeping traffic on the Azure backbone.

### Limits and availability

- VNets and subnets span availability zones; you don't need to create per-zone VNets for zonal resources.
- Some resource limits exist — consult Azure networking limits when designing at scale.

---

## Key networking topics (quick reference)

- VNets & Subnets
- Peering
- DNS
- VPN Gateway
- ExpressRoute
- Public / Private Endpoints
- Network Security Groups (NSGs)
- Load Balancers

---

## Load balancing & content delivery

Services for traffic distribution and optimization:

- Load Balancer (L4)
- Application Gateway (L7, WAF)
- Azure Front Door (global, edge)

---

## Hybrid connectivity

Hybrid connectivity services:

- VPN Gateway
- ExpressRoute
- Virtual WAN
- Peering services

---

## Network security

Protect web applications and IaaS:

- Azure Firewall / Firewall Manager
- Network Security Groups (NSGs)
- Web Application Firewall (WAF) via Application Gateway or Front Door
- DDoS Protection

---

## Networking management & monitoring

Tools to manage and monitor networks:

- Network Watcher
- Azure Monitor
- Azure Network Manager

---

## Azure Firewall (detailed)

Azure Firewall is a managed, cloud-native network security service that inspects and controls traffic to and from your VNets.

### Core components

| Component | Purpose |
|---|---|
| Firewall rules | Allow or deny traffic |
| Application rules | Control HTTP/HTTPS traffic (FQDN-based) |
| Network rules | Control IP/port-based traffic |
| NAT rules | Publish internal services to the internet (DNAT) |
| Threat Intelligence | Block known malicious IPs |

### Typical flow

Your PC → Firewall Public IP → DNAT → VM Private IP → SSH

### User-defined routes (UDR)

To ensure traffic is inspected by the firewall, create a UDR on the VM subnet that points 0.0.0.0/0 to the firewall private IP (Next hop type: Virtual Appliance).

Route example:

- Destination: 0.0.0.0/0
- Next hop type: Virtual Appliance
- Next hop address: <Firewall Private IP>

This forces egress: VM → Azure Firewall → Internet

### Why UDRs are required

1. Security enforcement — rules apply only if traffic flows through the firewall.
2. Centralized control — one firewall can protect multiple subnets.
3. Compliance & auditing — consolidated logs for outbound traffic.
4. Prevent firewall bypass — UDRs reduce the ability to bypass inspection.

### Recommended topologies

Option A — Firewall in front (perimeter control):

Internet → Azure Firewall (PIP) → DNAT → Application Gateway (internal) → Internal Load Balancer → VMs

Option B — App Gateway (WAF) front, Firewall for egress/inspection:

Internet → Application Gateway (Public WAF) → ILB/VMs
VM egress → UDR → Azure Firewall → Internet

Design rules:

- Put each managed service in its own subnet (AzureFirewallSubnet, AppGwSubnet, ILBSubnet, VMSubnet).
- App Gateway requires its own subnet; Azure Firewall requires subnet named AzureFirewallSubnet.
- Use UDRs on VMSubnet (and AppGwSubnet if needed) to route egress via the firewall.
- Use NSGs to restrict direct access to VMSubnet; allow only ALB/AppGW or specific ports.

### Example commands

Create VNet and subnets:

```bash
az network vnet create -g RG -n VNet --address-prefix 10.0.0.0/16 \
  --subnet-name AzureFirewallSubnet --subnet-prefix 10.0.1.0/24
az network vnet subnet create -g RG --vnet-name VNet -n AppGwSubnet --address-prefix 10.0.2.0/24
az network vnet subnet create -g RG --vnet-name VNet -n ILBSubnet --address-prefix 10.0.3.0/24
az network vnet subnet create -g RG --vnet-name VNet -n VMSubnet --address-prefix 10.0.4.0/24
```

Deploy Firewall (requires public IP):

```bash
az network public-ip create -g RG -n fw-pip --sku Standard
az network firewall create -g RG -n MyFirewall --sku AZFW_VNet
# Configure firewall IPs and NAT rules via portal/az based on DNAT needs
```

Create internal Load Balancer (ILB):

```bash
az network lb create -g RG -n ilb --sku Standard --vnet-name VNet --subnet ILBSubnet \
  --frontend-ip-name ilb-fe --private-ip-address 10.0.3.10
# Add backend pool, probe, and VMs to backend
```

Add UDR to route egress via the firewall:

```bash
az network route-table create -g RG -n rtbl
az network route-table route create -g RG --route-table-name rtbl -n default-route \
  --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.1.4
az network vnet subnet update -g RG --vnet-name VNet --name VMSubnet --route-table rtbl
```

### Practical tips

- Avoid UDRs that break platform management traffic for App Gateway.
- Test step-by-step (DNAT → AppGW → ILB → single VM) before scaling.
- Use health probes and matching backend HTTP settings to keep backends healthy.

---

## Topology ASCII diagrams

Option A — Firewall in front of internal App Gateway → ILB → VMs:

```
Internet (Public)
  |
  | (PIP, DNAT on Azure Firewall)
  v
Azure Firewall (AzureFirewallSubnet) [PIP]
  |
  v
Application Gateway (AppGwSubnet) [private]
  |
  v
Internal Load Balancer (ILBSubnet)
  |
  v
VM Backend Servers (VMSubnet)
```

UDR example:

```
0.0.0.0/0 --> VirtualAppliance --> <AzureFirewallPrivateIP>
```

---

## Azure DDoS Protection

DDoS protection defends applications from volumetric and protocol attacks that aim to exhaust resources or bandwidth and make services unavailable.

Common attack outcomes:

- Service unavailability
- Exhausted bandwidth
- Application instability

---

## References

- Azure networking fundamentals: https://learn.microsoft.com/azure/virtual-network

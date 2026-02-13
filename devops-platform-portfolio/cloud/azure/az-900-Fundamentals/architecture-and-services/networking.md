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
  ## Load Balancing && Content Delivery
   ###  Services allow for management, distribution, and optimization of your applications and workloads
   1. Load Balancer 
   2. Application Gateway
   3. Azure Front Door
  ## Hybrid Connectivity 
   ### Azure hybrid connectivity services secure communication to and from your resources in Azure
   1 . VPN Gateway
   2. Express Route
   3. Virtual WAN
   4. Peering Services 
  ## Network security 
   ### Network security services protect your web applications and IaaS services from DDoS attacks and Malicious actors
    1. Firewal Manager
    2. Firewal,
    3. Web Application firewall
    4. DDoS Protection 
  ## Networking management and monitoring 
   ### Networing management and moniotoring services provides tools to manage and monitor your network resources 
    1. Netwrok watcher
    2. Azure Monitor 
    3. Azure network manager 

# Virtual Networking 
1. **VNets**  
2. **Subnets**  
3. **Peering**  
4. **DNS**  
5. **VPN Gateway**  
6. **ExpressRoute**  
7. **Public / Private Endpoints**  
8. **Network Security Groups**  
9. **Load Balancers (Basics)**  
10. **Refer : https://learn.microsoft.com/pdf?url=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Fnetworking%2Ffundamentals%2Ftoc.json**

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

### Typical Flow
Your PC → Firewall Public IP → DNAT → VM Private IP → SSH

### Route Table (UDR)
To ensure traffic is inspected by Azure Firewall, create a User Defined Route (UDR) on the VM subnet that directs outbound traffic to the firewall's private IP.

- **Route Example:**
   - Destination: 0.0.0.0/0
   - Next Hop Type: Virtual Appliance
   - Next Hop Address: `<Firewall Private IP>`

This forces:
VM → Azure Firewall → Internet

### Why UDR is Required for Azure Firewall

1. **Security Enforcement**  
    - Firewall rules apply only if traffic passes through the firewall.  
    - Without a UDR, VMs can reach the internet directly and firewall rules are ignored.

2. **Centralized Control**  
    - One firewall can protect multiple subnets.  
    - Central place to manage outbound rules and logs.

3. **Compliance & Auditing**  
    - Logs for outbound traffic are consolidated.  
    - Useful for regulated environments (PCI, ISO).

4. **Prevent Firewall Bypass**  
    - Without UDRs, a user could assign a Public IP to a VM and bypass the firewall.

### Example Architecture (ASCII)

```
VM Subnet
   |
   | (UDR: 0.0.0.0/0 → Firewall private IP)
   ↓
Azure Firewall
   |
Internet
```

**Firewall Configuration:**
- Firewall Policy → Firewall Rules / NAT Rules  
- DNAT Example: Any IP (*) → Firewall Public IP → VM Private IP

**Recommended High-Level Topologies (Choose One)**

**Option A — Firewall in Front (Perimeter Control)**
```
Internet → Azure Firewall (Public) → DNAT → Application Gateway (Internal) → Internal Load Balancer → VM Backend Pool
```

**Option B — App Gateway in Front (WAF First) + Firewall for Egress/Central Inspection**
```
Internet → Application Gateway (Public WAF) → Internal Load Balancer → VMs
VM Subnet Egress (and/or AppGW Outbound) → User Defined Route → Azure Firewall (for Logging/Inspection) → Internet
```

**Key Design Rules**

- Put each managed service in its own subnet:
   - AzureFirewallSubnet (exact name required)
   - AppGwSubnet
   - ILBSubnet (or Backend Subnet)
   - VMSubnet
- App Gateway must be in its own subnet. Internal AppGW has private IP (used with Firewall DNAT).
- Azure Firewall must be in subnet named AzureFirewallSubnet.
- Internal Load Balancer (ILB) has a private frontend IP and backend pool of VMs (or VMSS).
- Use UDRs to force egress through Azure Firewall: on VMSubnet (and AppGwSubnet if you require inspection of AppGW outbound), route 0.0.0.0/0 → Virtual Appliance → `<AzureFirewallPrivateIP>`.
- Use NSGs to restrict direct access to VMs; allow only ALB/AppGW or specific ports.
- Health Probes: Configure ILB probe and AppGW backend HTTP settings to match app health endpoint.
- DNAT: If Firewall in front, create DNAT rule on Firewall to forward public port(s) to AppGW private IP/port.
- Certificates/SSL: Terminate at AppGW (WAF) or pass-through to ILB/VMs depending on your TLS model.

### Create VNet and Subnets
```bash
az network vnet create -g RG -n VNet --address-prefix 10.0.0.0/16 \
   --subnet-name AzureFirewallSubnet --subnet-prefix 10.0.1.0/24
az network vnet subnet create -g RG --vnet-name VNet -n AppGwSubnet --address-prefix 10.0.2.0/24
az network vnet subnet create -g RG --vnet-name VNet -n ILBSubnet --address-prefix 10.0.3.0/24
az network vnet subnet create -g RG --vnet-name VNet -n VMSubnet --address-prefix 10.0.4.0/24
```

### Deploy Firewall (Requires Public IP)
```bash
az network public-ip create -g RG -n fw-pip --sku Standard
az network firewall create -g RG -n MyFirewall --sku AZFW_VNet
# Configure firewall IPs and NAT rules via portal/az based on DNAT needs
```

### Deploy Application Gateway (Internal or Public)
### (App GW v2 recommended; long command omitted — use portal or ARM template)

### Create Internal Load Balancer -> Backend Pool of VMs/VMSS
```bash
az network lb create -g RG -n ilb --sku Standard --vnet-name VNet --subnet ILBSubnet --frontend-ip-name ilb-fe --private-ip-address 10.0.3.10
# Add backend pool and probe, then add VMs to backend pool
```

### Add UDR on VMSubnet to Route Egress via Firewall
```bash
az network route-table create -g RG -n rtbl
az network route-table route create -g RG --route-table-name rtbl -n default-route --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.1.4
az network vnet subnet update -g RG --vnet-name VNet --name VMSubnet --route-table rtbl
```

**Practical Tips / Gotchas**

- App Gateway requires its own subnet and care with UDRs — avoid UDR that breaks platform management traffic.
- If Firewall is in front, use internal AppGW and DNS so Firewall DNAT maps correctly.
- For client IP preservation, AppGW injects X-Forwarded-For; backend VMs behind ILB may see AppGW/ILB SNAT — design accordingly.
- Use probes and matching backend HTTP settings to avoid unhealthy backends.
- Test step-by-step: DNAT to AppGW → AppGW → ILB → single VM before scaling.

### Topology Diagrams (ASCII)

**Option A — Firewall in Front of Internal App Gateway → ILB → VMs**
```
Internet (Public) 
   |
   | (Public IP, DNAT on Azure Firewall)
   v
Azure Firewall (AzureFirewallSubnet) [PIP]
   |
   | (DNAT -> AppGw private IP, or route to AppGW subnet)
   v
Application Gateway (AppGwSubnet) [Private IP - Internal AppGW]
   |
   | (AppGW backend = ILB frontend)
   v
Internal Load Balancer (ILBSubnet) [Private Frontend IP]
   |
   | (Backend pool, health probes)
   v
VM Backend Servers (VMSubnet)
```

**UDR (Applied to VMSubnet):**
```
0.0.0.0/0 --> VirtualAppliance --> <AzureFirewallPrivateIP>
```
(Optional) UDR on AppGwSubnet if AppGW egress must be inspected.

**Notes:**
- Firewall: DNAT rule maps PIP:443 -> AppGwPrivateIP:443.
- AppGW must be in its own subnet (AppGwSubnet).
- ILB provides private frontend and backend pool of VMs/VMSS; probes must match app endpoints.
- NSGs restrict direct access to VMSubnet; allow only ILB/AppGW as source.

**Option B — Public App Gateway (WAF) in Front, Firewall Used for Egress Inspection**
```
Internet (Public)
   |
   v
Application Gateway (AppGwSubnet) [Public WAF]
   |
   v
Internal Load Balancer (ILBSubnet) or AppGW -> Backend VMs (VMSubnet)
   |
   v
VM Backend Servers
```

**UDR (Applied to VMSubnet and/or AppGwSubnet):**
```
0.0.0.0/0 --> VirtualAppliance --> <AzureFirewallPrivateIP>
```

**Notes:**
- Use AppGW public WAF to terminate TLS and protect apps.
- Use Azure Firewall for centralized outbound inspection/logging (via UDR).
- Choose TLS termination point: AppGW (recommended for WAF) or pass-through to VMs.

**Legend:**
- PIP = Public IP
- DNAT = Destination NAT (Firewall NAT rule)
- UDR = User Defined Route (route table forcing egress to firewall)
- Subnet Names: AzureFirewallSubnet, AppGwSubnet, ILBSubnet, VMSubnet


🔥 Side-by-Side Comparison

| Feature | NSG | Load Balancer | Application Gateway | Azure Firewall |
|---------|-----|---------------|-------------------|-----------------|
| OSI Layer | L3–L4 | L4 | L7 | L3–L7 |
| Traffic filtering | ✅ | ❌ | ❌ | ✅ |
| Load balancing | ❌ | ✅ | ✅ | ❌ |
| URL routing | ❌ | ❌ | ✅ | ❌ |
| SSL termination | ❌ | ❌ | ✅ | ❌ |
| WAF | ❌ | ❌ | ✅ | ❌ |
| Central security | ❌ | ❌ | ❌ | ✅ |
| Cost | Free | Low | Medium | High |

🏗️ Real-World Architecture Example
Typical secure web app setup:

```
Internet (Public)
   ↓
Application Gateway (WAF) [AppGwSubnet]
   ↓
Azure Load Balancer (Internal) [ILBSubnet]
   ↓
Web VMs (NSG applied) [VMSubnet]
   ↓
Azure Firewall (outbound control) [AzureFirewallSubnet]
   ↓
Internet (Outbound)
```

**Flow Explanation:**
1. **Public traffic** enters via Application Gateway (WAF layer)
2. **AppGW routes** to Internal Load Balancer (L4 distribution)
3. **ILB distributes** to backend VM pool (NSGs restrict access)
4. **VM outbound traffic** forced through Azure Firewall (UDR: 0.0.0.0/0 → Firewall Private IP)
5. **Firewall inspects** and logs all outbound connections before internet egress

🧠 Simple Rule to Remember

NSG → Who can talk to whom

Load Balancer → Spread traffic

Application Gateway → Smart web traffic + security

Azure Firewall → Central enterprise security

## Azure DDoS Protection (Distribted Deniel of Service)

What is a DDoS Attack? (Quick context)
A DDoS attack floods your application with huge volumes of traffic from many sources to:

Crash your app

Exhaust bandwidth

Make services unavailable
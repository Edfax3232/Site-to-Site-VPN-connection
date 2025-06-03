# Site-to-Site-VPN-connection
This is a Site-to-Site VPN Connection (On-Premises to Cloud)
Core Components:
# 🏢 Data Center 1 (On-Premises Environment)
Users and Routers: Representing on-premises users accessing the network.

VPN Gateway (Customer Gateway - CGW): Establishes the encrypted VPN tunnel.

Private/Public Subnets:

Public Subnet: Contains a NAT Gateway and a resource (likely a Bastion Host or VPN endpoint).

Private Subnet: Houses an EC2 instance (On-Premises Test).

Routing: Routes are configured to send traffic through the NAT or VPN tunnel.

# ☁️ Data Center 2 (AWS Environment)
Virtual Private Cloud (VPC): With isolated subnets.

VPN Gateway (VGW): On the AWS side to connect the VPN tunnel.

Private Subnet: Contains an EC2 instance (App Server) in a private subnet.

# 🔐 VPN Tunnel
A Site-to-Site VPN connection (highlighted with the purple icon in the middle) securely connects both environments.

Arrows show encrypted traffic flowing between on-premises and AWS.

# 🔄 Ping Test
A “ping” line indicates ICMP communication from the on-prem EC2 instance to the cloud EC2 instance—used for connectivity testing.

# ⚠️ Challenges Faced During Deployment
Incorrect Routing Configuration

Subnet or route table misconfigurations led to unresponsive VPN connections.

Solution: Verified route propagation in the AWS route tables and ensured CIDR blocks matched both ends.

Mismatched VPN Tunnel Parameters

IKE version mismatch or pre-shared key errors between on-prem and AWS gateways.

Solution: Reconfigured both CGW and VGW settings to align with AWS documentation.

NAT Gateway Blocking Traffic

NAT Gateway in the path may block incoming VPN traffic if incorrectly routed.

Solution: Ensured correct traffic segmentation between public and private subnets, and that NAT was used only for outbound internet traffic.

Firewall/Security Group Issues

Security Groups or NACLs (Network ACLs) were not allowing ICMP or port-specific traffic.

Solution: Updated AWS security groups to allow relevant ports (ICMP, SSH, etc.) from on-prem IP ranges.

Latency and Packet Loss

Poor VPN tunnel performance due to ISP throttling or suboptimal VPN routing.

Solution: Used CloudWatch VPN metrics and improved routing by testing different ISPs or configuring BGP for better failover.

# ✅ Key Takeaways / Solutions Implemented
Problem	Solution
Routing not working	Enabled route propagation and verified route tables
VPN handshake failing	Synced IKE/PSK settings on both sides
Traffic blocked	Reviewed SGs, NACLs, and NAT config
Ping/ICMP failed	Allowed ICMP in both inbound and outbound rules
Connectivity issues	Used CloudWatch metrics to troubleshoot and optimize


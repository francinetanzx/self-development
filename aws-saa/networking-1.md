## IP Addresses
- One part of the address identifies the network and the other part identifies the host
- Two types of IP Addresses
    - IPv4: 32-bit addresses, 4 sets of 8-bits, 4.3 billion addresses = addresses must be reused and masked 
        - Note: Cannot remove IPv4 support from VPC
    - IPv6: 128-bit addresses, 8 groups of 4-hex, 340 trillion trillion trillion = addresses can be unique
        - Global Unicast Address (GUA): Unique IPv6 address assigned to a host interface

## CIDR
- IP address range for a network or a subnet 
- VPC can have up to 5 CIDR blocks and their addresses range cannot overlap
- Subnet mast defines which portion of the IP address is dedicated to network identification and which can be used for host IP address
- Count the bits left after you minus from 32 and the number of available IP addresses is 2^that_num (e.g., /28 = 16 addresses)

## VPCs
- Logical isolation in the cloud for a network environment
- Created within a region and can span across the region's AZs (just need a subnet within that AZ)
- Dedicated to an AWS account 

## Subnets
- Subsets of of VPC 
- Reside within an AZ, do not overlap with each other
- An AZ can contain multiple subnets 
- 5 addresses within a subnet is reserved 
    1. 10.0.0.0: Network Address
    2. 10.0.0.1: Reserved by AWS by VPC router
    3. 10.0.0.2: Reserved by AWS for DNS Server 
    4. 10.0.0.3: Reserved by AWS for future use 
    5. 10.0.0.255: Network broadcast address (although not supported in VPC, still reserved)

## Public Subnets
- IGW for communication between resources within VPC and the internet 
- Route table with a route to the IGW for 0.0.0.0/0 destination 
- Addresses accessible via the Internet (Public IP addresses)

## Internet Gateways (IGW)
- Poses no availability risks and bandwidth constraints 
- Serves two purposes:
    - Provides a target in the RT for internet-routable traffice
    - Protects IP addresses within the network by performing Network Address Translation (NAT)
- Supports both IPv4 and IPv6 traffic 

## Route Table (RT)
- Contains a set of rules (routes) that the VPC uses to determine where to direct network traffic 
- Destination: IP address (e.g., 10.0.0.0/16 or 0.0.0.0/0)
- Target: Local or IGW
- VPC comes with a default main route table with a single rule to allow communication for all resources within the VPC (10.0.0.0/16 -> local) and this cannot be modified 
- Each subnet has to be associated to **only one RT**, if not specified, it will take the VPC's main RT 
- Reusable across various subnets 
- Can be associated with a subnet, IGW or Virtual Private Gateway (VPG) 

## Private Subnets
- Allow indirect access to the internet via Web tier resources = Traffic remains within the VPC
- Private IP addresses allocated to for e.g., EC2 will never be re-assigned 

## Default VPC 
- Default VPC will be within the /16 subnet mask 
- Two public subnets in each AZ
- IGW
- Main route table includes a route to the IGW that is used by the public subnets 

## Elastic IP Address 
- Static IPv4 public address that can be associated with any instance or network interface 
- Can be reassigned to another instance or network interface in the same or **different** VPC
- Accessed through the IGW (So... if there is a VPN connection between the VPC and another network, the Elastic IP address cannot be reached because the VPN traffic travels through the VPG)
- Limited to 5 Elastic IP address per AWS Account per region (Best to use NAT instead and maintain Elastic IP addresses for dynamic mapping in case of instance failure)

## Elastic Network Interface
- Virtual network card
- Transferrable to another instance but still maintains the public and private IP address and MAC address (network traffic will likewise be redirected) 
    - **NOTE: Can transfer across subnets but must be within same AZ**
- Each instance in a VPC has a default network interface (cannot be detached) and more network interfaces can be attached

## Network Address Translation with NAT Gateways
- NAT maps one IP address to another
- Provide a target in the RT for internet-routable traffic 
- Used to allow private instances initiate traffic to the internet or other AWS services + Prevent private instances from receiving inbound traffic from the internet
- NAT can be placed in both private and public subnets and if more control is required, they can also be installed in an EC2 instance to create a NAT instance

## Connecting Private Subnets to the Internet 
- Route private subnet's traffic to NAT gateway in the public subnet
- Route public subnet's traffic to the IGW 

## Network Access Control List (ACL)
- Subnet-level
- Default VPC's NACL allows all inbound and outbound traffic but custom NACLs denies all inbound and outbound traffic 
- Stateless and requires explicit rules for incoming and outgoing traffic 
- Evaluates the rules starting with the lowest numbered rule (priority)
- Rule that has an asterick for its number is the rule that denies any packet if it does not match any of the numbered rules (* ALL Traffic ALL ALL 0.0.0.0/0 DENY)
- Components: Rule number, type, protocol, port range, source (for inbound), destination (for outbound), allow or deny

## Security Group 
- Network interface-level 
- Only support allow rules
- Default group allows only inbound communication from other group members of the same group and outbound communication to any destination 
- Restrict by IP protocol, service port, and by source or destination IP address (individual IP address or CIDR block)
- Stateful so even if for e.g., outgoing traffic is denied, as long as incoming traffic allows, the (synchronous) outgoing traffic will be seen as an established connection and traffic is allowed to flow through = Some traffic flows are not tracked 

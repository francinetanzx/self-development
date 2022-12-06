## IP Addresses
- One part of the address identifies the network and the other part identifies the host
- Two types of IP Addresses
    - IPv4: 32-bit addresses, 4 sets of 8-bits, 4.3 billion addresses = addresses must be reused and masked 
        - Note: Cannot remove IPv4 support from VPC
    - IPv6: 128-bit addresses, 8 groups of 4-hex, 340 trillion trillion trillion = addresses can be unique
        - Global Unicast Address (GUA): Unique IPv6 address assigned to a host interface

## CIDR
- IP Address range for a network or a subnet 
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
- Addresses accessible via the Internet (Public IP Addresses)

## Internet Gateways (IGW)
- Poses no availability risks and bandwidth constraints 
- Serves two purposes:
    - Provides a target in the RT for internet-routable traffice
    - Protects IP addresses within the network by performing Network Address Translation (NAT)
- Supports both IPv4 and IPv6 traffic 

## Route Table (RT)
- Contains a set of rules (routes) that the VPC uses to determine where to direct network traffic 
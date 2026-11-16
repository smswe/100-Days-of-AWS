# Day 03 - Create a Subnet in the Default VPC

## 1. Task Description

The Nautilus DevOps team is migrating part of its infrastructure to AWS.

The task was to create one subnet named:

`nautilus-subnet`

The subnet must be created inside the default VPC.

Region: `us-east-1`

## 2. What is AWS VPC?

VPC stands for Virtual Private Cloud.

It is a logically isolated virtual network in AWS where we can launch and organize resources such as EC2 instances.

### Apartment Analogy

Imagine AWS is a huge country.

Different companies operate in this country.

Each company wants its own private apartment complex.

- AWS Cloud – Country
- VPC – Apartment complex
- Subnet – Building or block
- EC2 instance – Individual apartment
- Private IP address – Apartment address

Each company can create its own VPC with its own network configuration.

A VPC allows us to control:
- IP address ranges
- Subnets
- Route tables
- Internet connectivity
- Network security

## 3. What is a Default VPC?

A default VPC is a VPC that AWS can create automatically for an AWS account in a Region.

It provides a ready-made network environment.

A default VPC commonly includes:
- A default IPv4 CIDR block
- Default subnets across Availability Zones
- An Internet Gateway
- A main route table
- A default network ACL

In this task, the VPC list was empty.

Therefore, we used:

**VPC → Your VPCs → Actions → Create default VPC**

The VPC created was:

`vpc-0b7aa601fabe4b428`

Its CIDR block was:

`172.31.0.0/16`

## 4. What is an IP Address?

An IP address identifies a network interface on an IP network.

IPv4 addresses are 32 bits long.

Example:

`172.31.96.10`

It contains four octets:

| Octet | Value |
|---|---:|
| First | 172 |
| Second | 31 |
| Third | 96 |
| Fourth | 10 |

Each octet contains 8 bits.

Therefore:

`8 + 8 + 8 + 8 = 32 bits`

## 5. What is Binary?

Computers represent data using binary digits.

A binary digit is called a bit.

A bit can have only two values:

- 0
- 1

Example:

Decimal 10 in binary:

`00001010`

The values of the eight bit positions are:

| Position | Value |
|---|---:|
| 1 | 128 |
| 2 | 64 |
| 3 | 32 |
| 4 | 16 |
| 5 | 8 |
| 6 | 4 |
| 7 | 2 |
| 8 | 1 |

For `00001010`:

`8 + 2 = 10`

## 6. What is CIDR?

CIDR stands for Classless Inter-Domain Routing.

It is a way to represent an IP network and its address range.

Example:

`172.31.0.0/16`

The `/16` is called the CIDR prefix length.

It tells us that the first 16 bits identify the network.

An IPv4 address has 32 bits in total.

Therefore:

`Host bits = 32 - CIDR prefix`

## 7. Understanding /16

Our default VPC CIDR:

`172.31.0.0/16`

Calculation:

`32 - 16 = 16 host bits`

Total IP addresses:

`2^16 = 65,536`

Therefore, the VPC has an address range containing 65,536 IPv4 addresses.

## 8. Understanding /20

Our new subnet CIDR:

`172.31.96.0/20`

Calculation:

`32 - 20 = 12 host bits`

Total IP addresses:

`2^12 = 4,096`

AWS reserves five IPv4 addresses in every subnet.

Therefore:

`4,096 - 5 = 4,091 usable addresses`

This explains why AWS displays 4,091 available IPv4 addresses for a newly created /20 subnet.

## 9. Difference Between /16, /20 and /24

| CIDR | Network Bits | Host Bits | Total Addresses | AWS Usable Addresses |
|---|---:|---:|---:|---:|
| /16 | 16 | 16 | 65,536 | 65,531 |
| /20 | 20 | 12 | 4,096 | 4,091 |
| /24 | 24 | 8 | 256 | 251 |
| /25 | 25 | 7 | 128 | 123 |
| /26 | 26 | 6 | 64 | 59 |

Important rule:

A larger CIDR prefix means fewer addresses.

For example:
- /16 provides more addresses than /20.
- /20 provides more addresses than /24.

## 10. What is a Subnet Mask?

A subnet mask separates the network portion and host portion of an IPv4 address.

The subnet mask for /20 is:

`255.255.240.0`

The subnet mask for /16 is:

`255.255.0.0`

The subnet mask for /24 is:

`255.255.255.0`

### How to calculate the /20 subnet mask

A /20 network uses 20 network bits.

Each octet contains 8 bits.

First octet:

`11111111 = 255`

Second octet:

`11111111 = 255`

Third octet:

`11110000 = 240`

Fourth octet:

`00000000 = 0`

Therefore:

`255.255.240.0`

### Why does 11110000 equal 240?

Binary position values:

`128 64 32 16 8 4 2 1`

For `11110000`:

`128 + 64 + 32 + 16 = 240`

## 11. How to Calculate Subnet Block Size

The block size helps us identify where one subnet ends and the next begins.

For /20:

Subnet mask:

`255.255.240.0`

The third octet is 240.

Calculation:

`256 - 240 = 16`

Therefore, /20 subnet blocks increase by 16 in the third octet.

Examples:

| Subnet | CIDR |
|---|---|
| 1 | 172.31.0.0/20 |
| 2 | 172.31.16.0/20 |
| 3 | 172.31.32.0/20 |
| 4 | 172.31.48.0/20 |
| 5 | 172.31.64.0/20 |
| 6 | 172.31.80.0/20 |
| 7 | 172.31.96.0/20 |
| 8 | 172.31.112.0/20 |

## 12. Why Did We Select 172.31.96.0/20?

When we opened the Subnets page, six existing default subnets were displayed.

Their CIDR blocks were:

- `172.31.0.0/20`
- `172.31.16.0/20`
- `172.31.32.0/20`
- `172.31.48.0/20`
- `172.31.64.0/20`
- `172.31.80.0/20`

We needed another subnet without overlapping the existing address ranges.

Using the block size calculation, the next available block was:

`172.31.96.0/20`

Its complete address range is:

`172.31.96.0 – 172.31.111.255`

Subnet CIDR blocks in the same VPC cannot overlap.

## 13. How Many /20 Subnets Fit Inside a /16 VPC?

VPC CIDR:

`/16`

Subnet CIDR:

`/20`

Difference:

`20 - 16 = 4 bits`

Number of possible equal-sized /20 subnet blocks:

`2^4 = 16`

Therefore, a /16 network can be divided into sixteen /20 networks.

Each /20 subnet contains 4,096 total addresses.

`16 × 4,096 = 65,536`

This matches the total number of addresses in the /16 VPC.

## 14. What is an Availability Zone?

An Availability Zone (AZ) is an isolated infrastructure location within an AWS Region.

For example:

- `us-east-1a`
- `us-east-1b`
- `us-east-1c`

A subnet belongs to one Availability Zone.

In our planned configuration, we selected:

`us-east-1a`

Availability Zones help distribute infrastructure across separate locations for availability and resilience.

## 15. Important VPC Components

| Component | Purpose |
|---|---|
| VPC | Main logically isolated network |
| Subnet | Smaller network inside a VPC |
| Internet Gateway | Provides a VPC connection to the internet |
| Route Table | Controls where network traffic is directed |
| Security Group | Stateful firewall associated with network interfaces |
| Network ACL | Stateless traffic filter applied at subnet level |

### Internet Gateway

An Internet Gateway allows communication between a VPC and the internet when the route table and other network configurations permit it.

Apartment analogy: Main entrance connecting the apartment complex to the outside world.

### Route Table

A route table contains rules that determine where network traffic should go.

Example:

`0.0.0.0/0 → Internet Gateway`

This means traffic destined for any IPv4 address can be routed through the Internet Gateway, subject to other network controls.

Apartment analogy: Direction board showing which road to take.

### Security Group

A stateful firewall associated with network interfaces.

Apartment analogy: Security guard checking access to an individual apartment.

### Network ACL

A stateless firewall that controls inbound and outbound traffic at the subnet level.

Apartment analogy: Security checkpoint at the entrance of a building block.

## 16. Public Subnet vs Private Subnet

### Public Subnet

A subnet is considered public when its route table has a route to an Internet Gateway.

A resource also needs suitable public IP configuration and security rules to communicate directly with the internet.

Common use:
- Public web servers
- Load balancers
- Internet-facing resources

### Private Subnet

A private subnet does not have a direct route to an Internet Gateway.

Common use:
- Application servers
- Database servers
- Internal services

A private subnet can still have outbound internet access through a NAT Gateway if configured.

## 17. Steps Performed in AWS Console

1. Logged in to the AWS Management Console.
2. Selected `us-east-1`.
3. Opened VPC → Your VPCs.
4. Found that the VPC list was empty.
5. Selected Actions → Create default VPC.
6. Created the default VPC.
7. Opened VPC → Subnets.
8. Inspected the six existing subnet CIDR ranges.
9. Identified the next non-overlapping CIDR block.
10. Clicked Create subnet.
11. Selected the default VPC.
12. Entered the subnet name `nautilus-subnet`.
13. Selected Availability Zone `us-east-1a`.
14. Entered `172.31.96.0/20` as the IPv4 CIDR block.
15. Clicked Create subnet.

## 18. Configuration

| Setting | Value |
|---|---|
| VPC | Default VPC |
| VPC ID | `vpc-0b7aa601fabe4b428` |
| VPC CIDR | `172.31.0.0/16` |
| Subnet Name | `nautilus-subnet` |
| Availability Zone | `us-east-1a` |
| Subnet CIDR | `172.31.96.0/20` |
| Subnet Mask | `255.255.240.0` |
| Total IPv4 Addresses | 4096 |
| AWS Usable IPv4 Addresses | 4091 |

## 19. Common Mistakes and Troubleshooting

### CIDR block overlaps

Reason: The selected subnet range is already used by another subnet.

Solution: Inspect existing subnet CIDRs and select a non-overlapping range.

### Invalid CIDR block

Reason: The CIDR is not a valid network boundary or is outside the VPC's CIDR range.

Solution: Check the subnet prefix and the VPC address range.

### Subnet creation fails

Possible reasons:
- Insufficient IAM permissions.
- Invalid VPC selection.
- Overlapping CIDR.
- Invalid Availability Zone selection.

### No default VPC available

Solution: Check the selected AWS Region. If there is no default VPC, use the AWS console's Create default VPC action if permitted.

## 20. Important Things to Remember

- VPC is the main private network in AWS.
- A subnet is a smaller network inside a VPC.
- IPv4 addresses contain 32 bits.
- CIDR represents the network prefix length.
- /16 has 16 host bits.
- /20 has 12 host bits.
- /24 has 8 host bits.
- Total IP addresses = 2 ^ host bits.
- AWS reserves five IPv4 addresses per subnet.
- Subnet masks identify network and host portions.
- Subnet CIDR blocks in the same VPC cannot overlap.
- Every subnet belongs to one Availability Zone.

## 21. Result

Created the default VPC and selected the non-overlapping CIDR block `172.31.96.0/20` for the subnet `nautilus-subnet`.

Confirm that the subnet is in the Available state in the AWS console.

**Key Learning:** Understood VPC architecture, subnet creation, IPv4 addressing, CIDR notation, subnet masks, IP address calculations and AWS networking components.

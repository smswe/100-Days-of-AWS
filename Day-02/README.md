# Day 02 - Create an EC2 Security Group

## 1. Task Description

The task was to create a security group named `datacenter-sg` for the Nautilus application servers.

The security group needed the following inbound rules:

- HTTP – TCP port 80 – Source `0.0.0.0/0`
- SSH – TCP port 22 – Source `0.0.0.0/0`

The default outbound rules were left unchanged.

## 2. What is a Security Group?

A security group is a virtual firewall in AWS.

It controls the network traffic allowed to reach or leave resources through their network interfaces.

Think of an apartment security guard.

The guard checks:
- Who is entering?
- Which entrance are they using?
- Are they allowed inside?

Similarly, a security group checks network traffic using rules such as:
- Protocol
- Port number
- Source or destination

## 3. What is Inbound Traffic?

Inbound traffic means network traffic coming into an AWS resource.

Example:

A customer opens a company's website.

The customer's browser sends an HTTP request to the EC2 web server.

That request is inbound traffic for the EC2 instance.

Our security group needs to allow the required traffic.

## 4. What is Outbound Traffic?

Outbound traffic means network traffic leaving an AWS resource.

Example:

An EC2 server downloads software packages from the internet.

The request and response traffic involve outbound communication from the server.

AWS security groups allow all outbound traffic by default when created with the default outbound rule configuration. This task left those rules unchanged.

## 5. Understanding Ports

A port identifies a particular service or application endpoint on a machine.

Think of an apartment building with different service counters:
- Counter 22 – SSH service.
- Counter 80 – HTTP website service.
- Counter 443 – HTTPS website service.

Important ports:

| Port | Protocol/Service | Purpose |
|---|---|---|
| 22 | SSH | Remote Linux server access |
| 80 | HTTP | Website communication |
| 443 | HTTPS | Secure website communication |
| 3306 | MySQL | Database communication |
| 5432 | PostgreSQL | Database communication |
| 8080 | Common alternate HTTP | Application/web server traffic |

A port does not automatically mean that a service is running. The application must also be installed and listening on that port.

## 6. What is TCP?

TCP stands for Transmission Control Protocol.

It is a transport-layer protocol used to establish reliable communication between applications.

For this task:
- HTTP uses TCP port 80.
- SSH uses TCP port 22.

The security group rules specify TCP because these services use TCP communication.

## 7. Understanding `0.0.0.0/0`

This is an important CIDR notation.

`0.0.0.0/0` means all IPv4 addresses.

When we configure:

| Type | Port | Source |
|---|---:|---|
| HTTP | 80 | `0.0.0.0/0` |

It means HTTP traffic from any IPv4 address is allowed by this security group rule.

Similarly:

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | `0.0.0.0/0` |

It allows SSH connection attempts from any IPv4 address, subject to other network and instance controls.

**Security warning:** Opening SSH to the entire internet is not recommended for production environments. Restrict access to trusted IP addresses or use a secure administrative access service.

## 8. What Does Stateful Mean?

Security groups are stateful.

Imagine you visit a website.

1. Your browser sends a request to the server.
2. The security group permits the request.
3. The server sends a response.
4. The response traffic is automatically allowed because it belongs to the established connection.

You do not need to create a separate inbound rule just to permit the response to an allowed outbound connection.

This is called stateful filtering.

## 9. Steps Performed in AWS Console

1. Logged in to AWS Management Console.
2. Selected the `us-east-1` region.
3. Opened EC2.
4. Navigated to Network & Security → Security Groups.
5. Clicked Create security group.
6. Entered the security group name `datacenter-sg`.
7. Entered the description `Security group for Nautilus App Servers`.
8. Selected the default VPC.
9. Added the HTTP inbound rule:
   - Type: HTTP
   - Protocol: TCP
   - Port: 80
   - Source: `0.0.0.0/0`
10. Added the SSH inbound rule:
    - Type: SSH
    - Protocol: TCP
    - Port: 22
    - Source: `0.0.0.0/0`
11. Left the default outbound rules unchanged.
12. Created the security group.

## 10. Configuration

### General Details

| Setting | Value |
|---|---|
| Security Group Name | `datacenter-sg` |
| Description | Security group for Nautilus App Servers |
| VPC | Default VPC |
| Region | `us-east-1` |

### Inbound Rules

| Type | Protocol | Port | Source |
|---|---|---:|---|
| HTTP | TCP | 80 | `0.0.0.0/0` |
| SSH | TCP | 22 | `0.0.0.0/0` |

### Outbound Rules

Default outbound rules were unchanged.

## 11. Security Group vs Network ACL

Both control network traffic, but they work differently.

| Feature | Security Group | Network ACL |
|---|---|---|
| Applied to | Network interfaces | Subnets |
| Stateful | Yes | No, stateless |
| Allow rules | Yes | Yes |
| Deny rules | No explicit deny rules | Yes |
| Rule evaluation | All applicable rules | Rules evaluated by number |

Real-world analogy:

- Security Group: Security guard assigned to an individual apartment.
- Network ACL: Security checkpoint controlling traffic entering or leaving the entire building block.

## 12. Common Troubleshooting

### Website not accessible

Check:
- Is the EC2 instance running?
- Is the web server installed and running?
- Is the application listening on port 80?
- Does the security group allow inbound HTTP?
- Are the subnet routes configured correctly?
- Does the instance have suitable internet connectivity?

### SSH connection fails

Check:
- Is port 22 allowed?
- Is the source IP permitted?
- Is the correct private key being used?
- Is the correct username being used?
- Is the instance reachable through its network configuration?

## 13. Important Things to Remember

- Security groups act as virtual firewalls.
- Inbound rules control incoming connections.
- Outbound rules control outgoing connections.
- Port 22 is used for SSH.
- Port 80 is used for HTTP.
- Port 443 is used for HTTPS.
- `0.0.0.0/0` means all IPv4 addresses.
- Security groups are stateful.
- Security groups are associated with network interfaces.

## 14. Result

Created the security group `datacenter-sg` with inbound HTTP and SSH rules.

**Key Learning:** Understood how security groups control access to AWS resources using protocols, port numbers and source IP addresses.

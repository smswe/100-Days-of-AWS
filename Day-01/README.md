# Day 01 - Create an EC2 Key Pair in AWS

## 1. Objective

The task was to create an EC2 Key Pair named `nautilus-kp` in the AWS `us-east-1` (N. Virginia) region.

An EC2 Key Pair is used to securely connect to an EC2 instance using SSH (Secure Shell).

**Task requirements:**
- Region: `us-east-1` (N. Virginia)
- Key pair name: `nautilus-kp`
- Key pair type: RSA
- Private key file format: `.pem`

**Method used:** AWS Management Console (GUI). The downloaded key file was secured using a command in my local Linux terminal.

---

## 2. What is Amazon EC2?

Amazon EC2 stands for **Amazon Elastic Compute Cloud**.

It is an AWS service that allows us to create and use virtual servers in the cloud.

Instead of purchasing a physical server, we can launch a virtual machine on AWS and use it to run applications, host websites, or perform other computing tasks.

### Real-world example

Imagine a company needs a server to host its website.

It has two choices:

1. Purchase a physical server, install it in a data center, and maintain it.
2. Launch a virtual server using Amazon EC2.

With EC2, the company can create a server when needed, configure it, and pay according to its usage and pricing model.

---

## 3. What is an EC2 Key Pair?

An EC2 Key Pair is a set of two cryptographic keys used to authenticate a user when connecting to an EC2 instance.

It contains:

| Key | Purpose | Where it is kept |
|---|---|---|
| Public Key | Used to verify the identity of the connecting user | AWS stores/configures it for the EC2 instance |
| Private Key | Used by the user to prove their identity | Downloaded and kept securely by the user |

The two keys are mathematically related, but the private key cannot practically be calculated from the public key.

### Why do we need a Key Pair?

When we connect to a Linux EC2 instance using SSH, AWS uses public-key authentication.

The private key proves that we are authorized to connect. We do not need to send our private key to the server.

Without the correct private key, someone cannot authenticate using that key pair.

---

## 4. Understanding Public Key and Private Key

Think of a public key as a lock and a private key as the matching key.

- The public key can be placed on the server.
- The private key stays with the authorized user.
- When the user connects, SSH verifies that the user has the matching private key.

**Important:** Never share your private key with anyone.

If someone obtains your private key and has access to the corresponding server, they may be able to connect as you.

### Authentication flow

1. AWS configures the public key for the EC2 instance during launch.
2. The user starts an SSH connection using the private key.
3. The SSH server checks whether the user possesses the matching private key.
4. If authentication succeeds and other access rules allow it, the connection is established.

---

## 5. What is RSA?

RSA is a public-key cryptographic algorithm.

It is used in cryptographic systems to support secure communication and authentication.

When creating the EC2 Key Pair, we selected **RSA** as the key pair type.

RSA uses a mathematically related public and private key.

For this task:
- Key pair type: RSA
- Private key format: `.pem`

---

## 6. What is a PEM file?

PEM stands for **Privacy-Enhanced Mail**.

It is a text-based encoding format commonly used to store cryptographic certificates and private keys.

AWS provides the private key as a `.pem` file when we create the key pair using the PEM format.

Example filename:

```text
nautilus-kp.pem
```

This file contains sensitive private-key information.

**Important points:**
- Download the private key when AWS provides it.
- Store it in a secure location.
- Do not upload it to GitHub.
- Do not send it through email or messaging applications.
- AWS does not allow you to download the same private key again later.

If the private key is lost, you cannot recover that original downloaded private key from AWS.

---

## 7. Steps Performed – AWS Management Console

The key pair was created using the AWS Management Console, not the AWS CLI.

### Step 1: Sign in to AWS

1. Open the AWS Management Console.
2. Sign in to your AWS account.
3. Search for **EC2** in the AWS search bar.
4. Open the EC2 service.

### Step 2: Select the AWS Region

At the top-right corner of the AWS Console, select:

```text
US East (N. Virginia)
us-east-1
```

**What is a Region?**

An AWS Region is a geographical area where AWS operates data centers.

Examples:
- `us-east-1` – N. Virginia
- `ap-south-1` – Mumbai
- `eu-west-1` – Ireland

Resources such as EC2 instances and key pairs are associated with a region. Always check that you are working in the intended region.

### Step 3: Open Key Pairs

In the EC2 dashboard:

1. Find **Network & Security** in the left navigation panel.
2. Select **Key Pairs**.
3. Click **Create key pair**.

### Step 4: Configure the Key Pair

Enter the following details:

| Configuration | Selected value |
|---|---|
| Name | `nautilus-kp` |
| Key pair type | RSA |
| Private key file format | `.pem` |

Click **Create key pair**.

### Step 5: Download the Private Key

After creating the key pair, AWS automatically downloads the private key file.

The downloaded file is:

```text
nautilus-kp.pem
```

Keep this file safe. It is required when connecting to a Linux EC2 instance using this key pair.

---

## 8. Secure the Private Key Using Linux

After downloading the `.pem` file, I used my local Ubuntu/Linux terminal to change its permissions.

### Command used

```bash
chmod 400 nautilus-kp.pem
```

### What is chmod?

`chmod` stands for **change mode**.

It is a Linux command used to change the permissions of files and directories.

Linux permissions control who can:
- Read a file
- Write or modify a file
- Execute a file

### Understanding 400

Linux permissions can be represented using numbers.

| Number | Permission |
|---|---|
| 4 | Read |
| 2 | Write |
| 1 | Execute |
| 0 | No permission |

The numbers are added to represent combinations:

| Number | Meaning |
|---|---|
| 0 | No permissions |
| 1 | Execute |
| 2 | Write |
| 3 | Write + Execute |
| 4 | Read |
| 5 | Read + Execute |
| 6 | Read + Write |
| 7 | Read + Write + Execute |

The three digits in `chmod 400` represent permissions for:

1. Owner
2. Group
3. Others

So:

```bash
chmod 400 nautilus-kp.pem
```

means:

| User category | Permission | Number |
|---|---|---|
| Owner | Read only | 4 |
| Group | No permission | 0 |
| Others | No permission | 0 |

In simple words, only the file owner can read the private key. Nobody else gets permission through the file's group or other-user permissions.

### Verify the permission

Run:

```bash
ls -l nautilus-kp.pem
```

Expected permission format:

```text
-r--------
```

Explanation:

- First `-` means it is a regular file.
- `r--------` means only the owner has read permission.
- Group and others have no permissions.

**Why is this important?**

SSH may reject a private key if its file permissions are too open. Restricting permissions protects the private key from being read by other local users.

---

## 9. How is the Key Pair Used to Connect to EC2?

Creating a key pair does not automatically create an EC2 instance or connect us to one.

The key pair can be selected when launching a compatible EC2 instance.

For example, if we launch a Linux EC2 instance and select `nautilus-kp`, AWS configures the corresponding public key for SSH authentication.

A typical SSH command is:

```bash
ssh -i nautilus-kp.pem ec2-user@<EC2-PUBLIC-IP>
```

Explanation:

| Part | Meaning |
|---|---|
| `ssh` | Starts a secure remote connection |
| `-i` | Specifies the identity/private-key file |
| `nautilus-kp.pem` | Private key file |
| `ec2-user` | Example login username for Amazon Linux |
| `<EC2-PUBLIC-IP>` | Public IPv4 address of the EC2 instance |

The username depends on the operating system. For example, Ubuntu instances commonly use `ubuntu`, while Amazon Linux commonly uses `ec2-user`.

**Note:** This is an example of how the key pair is used. In this task, I created the key pair and secured the downloaded file; an EC2 SSH connection was not part of the confirmed steps.

---

## 10. Important Concepts to Remember

| Concept | Meaning |
|---|---|
| EC2 | AWS service for virtual servers |
| Key Pair | Public and private key combination |
| Public Key | Key configured on the server for authentication |
| Private Key | Secret key kept by the user |
| RSA | Public-key cryptographic algorithm |
| PEM | Text-based format used to store the private key |
| SSH | Secure Shell, used to connect to remote servers |
| Port 22 | Default SSH port |
| chmod | Linux command to change file permissions |
| chmod 400 | Gives owner read-only permission |
| Region | Geographical AWS location |

---

## 11. Common Mistakes and Troubleshooting

### Problem 1: Permission denied (publickey)

Possible reasons:
- Wrong private key selected.
- Incorrect SSH username.
- Public key on the instance does not match the private key.
- The instance was launched with a different key pair.

Check the key pair selected during instance launch and confirm the correct username.

### Problem 2: Unprotected private key file

You may see an SSH warning similar to:

```text
WARNING: UNPROTECTED PRIVATE KEY FILE!
```

This means the private key file permissions are too open.

Fix:

```bash
chmod 400 nautilus-kp.pem
```

### Problem 3: Permission denied while accessing the file

Check the file permissions:

```bash
ls -l nautilus-kp.pem
```

Make sure the file exists in the current directory. If necessary, move to the directory where it was downloaded or provide the full file path.

### Problem 4: Lost the private key

AWS does not let you download the same private key again.

Depending on the instance and access available, you may need to use another authorized access method to restore access or configure a replacement key.

---

## 12. Security Best Practices

1. Never share your `.pem` private key.
2. Never upload private keys to GitHub.
3. Keep private keys in a secure local directory.
4. Use `chmod 400` to restrict access to the key file.
5. Use the correct key pair when launching an EC2 instance.
6. Restrict SSH access through security groups to trusted IP addresses where possible.
7. Avoid storing private keys in publicly accessible folders.

**GitHub safety:** Document the filename and command, but never upload the actual `.pem` file.

---

## 13. Final Result

Successfully created the EC2 Key Pair using the AWS Management Console.

| Item | Result |
|---|---|
| AWS Service | Amazon EC2 |
| Region | `us-east-1` |
| Key Pair Name | `nautilus-kp` |
| Key Type | RSA |
| File Format | `.pem` |
| Private Key | Downloaded |
| Local Linux Command | `chmod 400 nautilus-kp.pem` |

**Day 01 Status: Completed**

---

## 14. What I Learned

- What Amazon EC2 is and why cloud servers are used.
- What an EC2 Key Pair is.
- The difference between public and private keys.
- How RSA-based key authentication works at a high level.
- How to create a key pair using the AWS Management Console.
- What a PEM file contains and why it must be protected.
- How to use `chmod 400` to secure a private key on Linux.
- How SSH uses a private key to authenticate a user.

**Key takeaway:** The public key is configured on the server, while the private key stays securely with the user. SSH uses this key pair to authenticate access to an EC2 instance.

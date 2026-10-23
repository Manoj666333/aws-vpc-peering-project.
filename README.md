# AWS VPC Peering with Public and Private Subnets

## 1. Project Overview

This project demonstrates how to create two Amazon Virtual Private Clouds (VPCs) and establish private communication between them using VPC Peering.

The project uses public and private subnets, EC2 instances, route tables, an Internet Gateway, and security groups.

**Project Status:** Connectivity testing and troubleshooting in progress.

## 2. AWS Services Used

- Amazon VPC
- Amazon EC2
- VPC Peering
- Internet Gateway
- Route Tables
- Public and Private Subnets
- Security Groups

## 3. Network Architecture

### VPC A

- VPC Name: `ewb-vpc-a`
- CIDR Block: `10.10.0.0/16`
- Public Subnet: `ewb-a-public` (`10.10.1.0/24`)
- Private Subnet: `ewb-a-private` (`10.10.2.0/24`)
- Internet Gateway: `ewb-igw-a`
- Public Route Table: `ewb-rt-a-public`
- Private Route Table: `ewb-rt-a-private`

### VPC B

- VPC Name: `ewb-vpc-b`
- CIDR Block: `10.20.0.0/16`
- Private Subnet: `ewb-b-private` (`10.20.1.0/24`)
- Private Route Table: `ewb-rt-b-private`

### EC2 Instances

| Instance | Private IP | Purpose |
|---|---|---|
| `ewb-bastion` | `10.10.1.10` | SSH entry point |
| `ewb-app-a` | `10.10.2.10` | Private instance in VPC A |
| `ewb-app-b` | `10.20.1.10` | Private instance in VPC B |

## 4. VPC Peering

- Peering Connection: `ewb-a-b-peer`
- Requester VPC: `ewb-vpc-a`
- Accepter VPC: `ewb-vpc-b`

VPC Peering allows resources in the two VPCs to communicate using private IP addresses when routing and security rules permit the traffic.

## 5. Route Table Configuration

| Route Table | Destination | Target |
|---|---|---|
| `ewb-rt-a-public` | `10.10.0.0/16` | Local |
| `ewb-rt-a-public` | `0.0.0.0/0` | Internet Gateway |
| `ewb-rt-a-public` | `10.20.0.0/16` | VPC Peering |
| `ewb-rt-a-private` | `10.10.0.0/16` | Local |
| `ewb-rt-a-private` | `10.20.0.0/16` | VPC Peering |
| `ewb-rt-b-private` | `10.20.0.0/16` | Local |
| `ewb-rt-b-private` | `10.10.0.0/16` | VPC Peering |

The VPC Peering routes must be added after the peering connection is created.

## 6. Security Configuration

- SSH access to the bastion is restricted to the administrator's IP address.
- Private EC2 instances do not require public IP addresses.
- Security groups restrict SSH and ICMP traffic to the intended sources.
- Private communication between VPCs uses the peering connection.

## 7. Testing and Results

The initial connectivity tests were performed from the bastion instance.

Commands used:

```bash
ping -c 4 10.10.2.10
ping -c 4 10.20.1.10
```

**Initial result:** Both tests reported 100% packet loss.

Connectivity is not yet verified. Route tables, peering status, security groups, network ACLs, and instance firewall settings need to be checked before confirming successful communication.

## 8. Troubleshooting Checklist

- Verify that the peering connection is Active.
- Check route table associations.
- Confirm that all required peering routes exist.
- Verify security group inbound rules.
- Check network ACLs.
- Confirm the private IP addresses of the EC2 instances.
- Check whether the operating system permits ICMP traffic.

## 9. Learning Outcomes

- Understanding VPCs and IPv4 CIDR blocks.
- Creating public and private subnets.
- Configuring route tables and an Internet Gateway.
- Establishing VPC Peering.
- Configuring security groups.
- Testing and troubleshooting network connectivity.

## 10. Cost Management

Resources should be deleted after testing to avoid unnecessary charges. Check for EC2 instances, EBS volumes, snapshots, Elastic IPs, and other billable resources before finishing.

## 11. Future Improvements

- Resolve connectivity failures.
- Capture successful connectivity test results.
- Document final route tables and security group rules.
- Add final screenshots after successful validation.

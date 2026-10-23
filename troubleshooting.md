# AWS VPC Peering Troubleshooting

## Issue Observed

Initial ping tests from the bastion instance failed:

```bash
ping -c 4 10.10.2.10
ping -c 4 10.20.1.10
```

Both tests reported 100% packet loss.

## Troubleshooting Steps

### 1. Check EC2 Instances

Confirm that the bastion, App-A, and App-B instances are running and that their private IP addresses are correct.

### 2. Check VPC Peering

Open VPC → Peering connections and verify that `ewb-a-b-peer` has status Active.

### 3. Check Route Tables

Verify the following routes:

- `ewb-rt-a-public`: `10.20.0.0/16` → VPC Peering
- `ewb-rt-a-private`: `10.20.0.0/16` → VPC Peering
- `ewb-rt-b-private`: `10.10.0.0/16` → VPC Peering

Also verify that each route table is associated with the intended subnet.

### 4. Check Security Groups

Confirm that the security groups attached to App-A and App-B permit the intended ICMP traffic from the appropriate source CIDR ranges.

For SSH, check that TCP port 22 is permitted only from the intended trusted sources.

### 5. Check Network ACLs

Confirm that network ACL rules permit both inbound requests and outbound responses.

### 6. Check the Instance Firewall

The operating system may block ICMP traffic. A failed ping does not by itself prove that VPC Peering is broken.

## Current Status

Connectivity requires further troubleshooting and successful retesting before the project can be marked complete.

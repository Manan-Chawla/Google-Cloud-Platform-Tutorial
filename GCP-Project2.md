# **Project2 : Connected VPC with VM Instances**

**Task we need to perform in this project**
1. Create two VPC network
2. Create 1 VM Instances for each VPC network
3. Region can be different for each VPC network
4. Connect the two VPC networks with a VPC peering connection

-------

## **Phase 1: Create the Two Islands (VPCs)**
We need two networks with different IP ranges. If the ranges overlap, peering will fail.
```1. Create Network A```
Name: vpc-alpha
Subnet Mode: Custom
Subnet Name: subnet-alpha
Region: us-central1
IPv4 Range: 10.1.0.0/24 

```2. Create Network B```
Name: vpc-beta
Subnet Mode: Custom
Subnet Name: subnet-beta
Region: us-central1
IPv4 Range: 10.2.0.0/24 

------------------

## **Phase 2: Create the Firewall "Passes"**
You must tell each VPC to allow traffic from the other one.
```1. For VPC Alpha: Create a rule named allow-beta-in.```
Network: vpc-alpha
Source Filter: 10.2.0.0/24 
Protocols: Allow all (or ICMP for pinging).

```2. For VPC Beta: Create a rule named allow-alpha-in.```
Network: vpc-beta
Source Filter: 10.1.0.0/24
Protocols: Allow all.

----------------

## **Phase 3: Build the Bridge (The Peering)**
Peering is a "handshake." Both sides must agree.
Go to VPC Network Peering > Create Connection.

```1. From Alpha to Beta:```
Name: alpha-to-beta
Your VPC: vpc-alpha
Peered VPC: vpc-beta

```2. From Beta to Alpha:```
Name: beta-to-alpha
Your VPC: vpc-beta
Peered VPC: vpc-alpha


---------------------

## **Phase 4: The Proof (The "Ping" Test)**
Let's put one person on each island and see if they can shout to each other.

```Create VM-Alpha: Put it in vpc-alpha. (Give it an External IP so you can SSH into it).```

```Create VM-Beta: Put it in vpc-beta. (No External IP needed! This makes it a real challenge).```

The Test:
SSH into VM-Alpha.
Find the Internal IP of VM-Beta (it will start with 10.2.0.x).
In the terminal of VM-Alpha, type: ping -c 4 [INTERNAL_IP_OF_VM_BETA]
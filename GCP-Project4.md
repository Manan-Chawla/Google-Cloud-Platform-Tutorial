# **Secure Private Storage Gateway**
Remember we are using following parameters for this project -->
* Region : us-central1, us-east1  or us-west1
* Machine : e2-micro (standard)
* Disk : 30GB (standard presistent)
* Storage : 5GB (standard storage)


## **Phase 1 : Create a VPC Network**
1. Click on VPC Network from sidebar
2. Click on VPC Network
3. Click Create
4. Name : byteedu-network
5. Subnet creation mode : Custom
6. New Subnet Configuation :-
   Name : private-storage-subnet
   Region : us-central1 (lowa)
   IPV4 Range : 10.0.1.0/24
7. Click Done and Create


## **Phase 2 : Setup Firewall Rules**
1. Go to VPC Network > Firewall 
2. Click on Firewall
3. Click on Create Firewall Rule
4. Name : allow-ssh-ingress
5. Network : byteedu-network
6. Direction : Ingress
7. Target : All instances in the network
8. Secure IPV4 Range : 0.0.0.0/0
9. Protocols and Ports : Check TCP and type 22
10. Click on Create


## **Phase3 : Create a Bucket or Vault with Versioning**
1. Go to Google Cloud Storage
2. Click on Buckets
3. Click on Create
4. Name : byteedu-vault-maxius
5. Location Type : Region > us-central1
6. Storage class : Standard
7. Protection : Under "Data Protection", check Object Versioning.
   Also check Add Recommended LifeCycle rules so you dont pay for 100 version of the same file.
8. Click on Create


## **Phase4 : Launch the VM Instance**
1. Go to Compute Engine
2. Click on VM Instances
3. Click on Create Instances
4. Name : storage-gateway
5. Region : us-central1
6. Machine Configuration : e2-micro
7. Boot Disk : Click Change
   OS : Debian 12
   Disk Type : Standard Persistent Disk
   Size : 30GB
8. Networkin : Expand the "Advanced Options" > Networking
   Select Network Interface : change to our VPC we created eariler
9. Click On Create


## **Phase5 : Connecting VM to Bucket**
1. Click on SSH
2. Install GCS Fuse tool using following command :-
   sudo gcloud auth application-default login  (follow the link in the terminal to authorize)
3. Create a folder and mount the bucket :-
   mkdir ~/my-vault
   gcsfuse byteedu-vault-maxius ~/my-vault
4. Create a new file in that vault -->
   echo "This is my byteedu secret file" > ~/my-vault/secert.txt
5. Move an existing file from VM into vault :-
   mv xyz.jpg ~/my-vault/
6. Verify it's there :-
   ls ~/my-vault
7. once we done with this let's delete folder :-
   rm -rf ~/my-vault/your-folder-name
8. To remove the link between VM and Bucket using following code :-
   fusermount -u ~/my-vault

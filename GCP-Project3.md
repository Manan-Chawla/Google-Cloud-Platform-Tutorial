# **PROJECT3 : Secure Document Drop Box**

## **The Architecture Plan**
1. Storage --> A GCP Bucket
2. Worker --> A VM Instance
3. Identity --> Service Account with Secert Role in the IAM Key


## **Phase 1 : Creating a Cloud Storage or Bucket**
1. Go to Google Cloud Storage
2. Click on Bucket > Click on Create
3. Name : secure-drop-box
4. Location : region : us-central1 (lowa)
5. Access Control : Keep it Uniform and click on Create
6. Wait for 2-3 min

## **Phase 2 : Create A IAM Identity**
1. Go to IAM & Admin
2. Then click on Service Accounts
3. Click on +Create Service Account
4. Name : drop-box-uploader
5. Click on Create and Continue
6. Now Assign the Role :- 
  Search for **Storage Object Creator**
  This will allow someone to write files only but not list or read them, more like it's a one way street
7. Click on Done.

## **Phase 3 : Creating VM**
1. Click on Compute Engine > VM Instances and click Create
2. Name : uploader-vm
3. Machine-Type : e2-micro
4. Click on Identity and API Access
   Under service account, change **default** to the **drop-box-uploader**
5. Click on Create

## **Phase 4 : Testing our project**
1. Open SSH of our vm-instance (uploader-vm)
2. Let's first perform "the forbidden action" intentionally:-
   Type : ```gcloud storage ls gs://secure-drop-box```
   Result : Access Denied/Forbidden Error
3. Let's do allowed action :-
   Type : ```echo "My Secert File" > test.txt```
   Them type : ```gcloud storage cp test.txt gs://secure-drop-box```
   Result : Success Message
4. Let's do one more forbidden action :-
   Type : ```gcloud storage ls gs://secure-drop-box```

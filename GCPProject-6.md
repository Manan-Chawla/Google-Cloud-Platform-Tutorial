# **Creating a container image to cloud run**
1. Open Cloud run from Google Cloud Console
2. Click on **Deploy Container**
3. Click on **Service**
4. Insert ```ngnix``` in the container image url
5. Service name : **nginx**
6. Region : **us-central1**
7. Uncheck "Use IAM to authenticate incoming requests"
8. Make sure Billing request has to be "Request Based"
9. Click on Container(s),Volumes,Networking,Security
   Set Container port : 80 
10.Click on Create 
11. As soon you see Green ticks of **Creating Service, Creating Revision and Routing Traffic**
    It will provide a url like this :-
    ```https://ngnix-2650656912.us-central1.run.app```
    Here's our app is located

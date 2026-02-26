# **Project 1 : Create a Native Static Media Serverk**
This project work both as frontend and backend where our media will be in a bucket and frontend is in our vm instance.
This mimic as Instagram or netlifx working.

## **Step1 : Create A Cloud Bucket**
* Go to Storage > Bucket
* Name : my-gallery-storage-bucket1
  Location : us-central1(lowa)
  Access Control : fine-grained
  Enforce Public Access Preventaion : False (uncheck it)
* Upload a image from your device in this bucket
* Now click on three dots 
  Edit Access : Add Entry (click on it)
  Entity :  User
  Name : your-email@gmail.com 
  Access : Reader
* Now copy the public url of that image, we will use it later in our vm instance


## **Step2 : Create A VM Instance**
* Open Cloud Shell '[>_]' at top of the console
* Authorize and wait for 2 mins
* Write down following code :- 
```
  gcloud compute instances create gallery-server\
  --zone=us-central1-a\
  --machine-type=e2-micro\
  --image-family=debian\
  --image-project=debian-cloud\
  --tags=http-server
```
* Press enter and then write this code too :- 
```
  gcloud compute firewall-rules create allow-http\
  --direction=INGRESS\
  --priority=1000\
  --network=default\
  --action=ALLOW\
  --rules=tcp:80\
  --source-ranges=0.0.0.0/0\
  --target-tags=http-server
```
* Press enter and then wait for 2 mins.

## **Step3 : Setting Up A Server**
* Open VM instances
* You will see our vm instance 'gallery-server'
* Click on 'SSH' to connect to our vm instance
* Write down code to install apache server :-
  sudo apt update && sudo apt install apache2 -y
* Move into folder :- 
  cd /var/www/html
* Create a file :-
  sudo nano index.html
* There will be a exisitng code, remove that by pressing ```ctrl+k```, untill screen's content become blank
* Now write following code :- 
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello World!</h1>
    <img src="https://storage.googleapis.com/my-gallery-storage-bucket1/your-image.jpg" alt="Your Image">
</body>
</html>
```
* Press ```ctrl+o``` to save and then press 'enter'
* Press ```ctrl+x``` to exit   

**NOW OPEN THE EXTERNAL IP ADDRESS OF OUR VM INSTANCE IN BROWSER**
* You will see our image in browser
* You can also see our 'Hello World!' text in browser with our image

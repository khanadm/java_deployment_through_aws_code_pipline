#### Java_deployment_through_aws_code_pipline

#### Description:
 
- Take sample  application 
- Create a aws pipeline and deploy application
- Build JAVA application from jenkins
- JAVA application need web application to deploy (we use tomcat as web application)

#### First step: Create an 2 EC2 server one for jenkins and second is for tomcat .

  - Firstly, log in to the AWS management console.
  - After login, navigate to the search bar, type EC2, and select EC2
  - Now, the EC2 dashboard will appear. Click Instances
  - Click the Launch instances button.
  - To launch an EC2 instance, a few details are required, i.e., instance name, OS image (AMI), instance type, etc.
  - Select a key pair to attach with the instance to log in with that key.
  - Click the Launch instance button
  - Take the public IP from the EC2 dashboard and use it to login inside the instance using ssh.
  - Finally, it will be logged in successfully if everything is configured correctly.

let's insatll jenkins 


First login to by ssh &  run this command


 ```sh
    sudo apt-get update
   ``` 
 
Jenkins require jdk to run so let's install jdk using ```sudo apt install default-jdk -y```


To check jdk is successfully installed or not use ```java -version```


Now to install jenkins, follow below steps these are provided on jenkins official website also:


1. Add the key to your system using:
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```


2. Then add a Jenkins apt repository entry:
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
```


3. Update your local package index, then finally install Jenkins:
```
sudo apt-get update
  sudo apt-get install fontconfig openjdk-11-jre
  sudo apt-get install jenkins
```


To check jenkins is successfully installed or not use ```jenkins --version```


Now, check the status of jenkins service using ```sudo systemctl status jenkins```


As jenkins, by default runs on port number 8080 so we need to edit inbound rule in  Security groups  to enable port 8080. So that jenkins 
can be accessed using **INSTANCE_PUBLIC_IP:8080**


Now try to hit ```<INSTANCE_PUBLIC_IP>:8080```

In this location we will get admin password 


  ```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```































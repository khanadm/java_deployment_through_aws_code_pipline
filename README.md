#### Java_deployment_through_aws_code_pipline

#### Description:
 
- Take sample JAVA application .
- Create a aws pipeline and deploy application .
- Build JAVA application from jenkins .
- JAVA application need web application to deploy (we use tomcat as web application to deploy the .war files).

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

#### let's Insatll jenkins & configure it. 


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


Now, its asking to install plugins but we don't want to install so click on cancel


then click **start using jenkins**


As we have cancelled to install jenkins plugins so it also cancelled setting admin password. So we need to set admin password else if we 
logout we won't be able to login again.


So let's create/change admin password. For this click **Manage Jenkins** then in Security section click **Manage Users**


Now click on setting button of admin user


Scroll down and look for password section then enter password that you want and click **Save**


Now again search for **INSTANCE_PUBLIC_IP:8080** form any browser and enter username and password


![jenkins](https://user-images.githubusercontent.com/106643382/218965016-90d15201-a411-44b8-8891-70aca2a50133.png "jenkins")



We need plugin while using aws codepipeline


Go to Manage jenkkins then Manage Plugin in there Avaliable Plugin in search bar type aws code pipeline

In my case i have allready install so it is showing in Installed Plugin 

![aws codepipeline](https://user-images.githubusercontent.com/106643382/219024446-c2099a61-b8d4-48a1-b2f3-df3001ec4b62.png "aws codepipeline")


Also we need Maven Build the application 

![Maven plugin](https://user-images.githubusercontent.com/106643382/219026145-bd9303f2-5007-4dfb-9279-483cb0143bc1.png "Maven plugin")


Create a free style project 

![Screenshot from 2023-02-15 18-11-12](https://user-images.githubusercontent.com/106643382/219030171-6edc755c-d4cb-4ca8-9343-afdce9b90633.png)



![Screenshot from 2023-02-15 18-11-39](https://user-images.githubusercontent.com/106643382/219030210-4a63f84e-620e-4de4-8bdd-889921b49596.png)



![Screenshot from 2023-02-15 18-12-38](https://user-images.githubusercontent.com/106643382/219030375-a1af371c-a8b8-4987-b9a7-7c1d6fba8329.png)


![Screenshot from 2023-02-15 18-12-50](https://user-images.githubusercontent.com/106643382/219030397-5053f4ce-2c1b-476d-b7e1-97162f10586f.png)



![Screenshot from 2023-02-15 18-13-35](https://user-images.githubusercontent.com/106643382/219030499-dfbe0d94-0cd8-4c6c-b71a-dd7a11c4ed09.png)



![Screenshot from 2023-02-15 18-16-20](https://user-images.githubusercontent.com/106643382/219031081-5e40bbce-e824-4f53-9457-ddcb86bdbcf6.png)



![Screenshot from 2023-02-15 18-16-54](https://user-images.githubusercontent.com/106643382/219031190-cf691afd-8b35-4185-91d8-fd4611aa4798.png)



![Screenshot from 2023-02-15 18-17-29](https://user-images.githubusercontent.com/106643382/219031329-1596206a-8e0c-4a1b-a24c-70781c658a9a.png)



![Screenshot from 2023-02-15 18-18-14](https://user-images.githubusercontent.com/106643382/219031496-628d6ad5-a295-4268-882b-0be094baddf4.png)



![Screenshot from 2023-02-15 18-18-42](https://user-images.githubusercontent.com/106643382/219031582-51c0647d-8675-49d6-82b9-f3a9572aa035.png)



![Screenshot from 2023-02-15 18-19-29](https://user-images.githubusercontent.com/106643382/219031751-7b23ecab-a285-44e8-930c-e7b1f1b63849.png)




#### Now let's come on to next server and Insatll & Configure Tomcat




First login to by ssh &  run this command


 ```sh
    sudo apt-get update
   ``` 

Then we need to Download tar.gz of Tomcat from offical page for that we need copy the url run this command 


```
wget https://downloads.apache.org/tomcat/tomcat-8/v8.5.85/bin/apache-tomcat-8.5.85.tar.gz.sha512
```
 
we need to extract under /opt/ directory

```
tar xvzf apache-tomcat-8.5.84.tar.gz -C /opt/
```

To see go to
 
 ```
cd /opt/
```
Then we need to create a user for tomcat 

```
useradd tomcat
```
 
 ```
passwd tomcat
```
then change the ownership

```
chown -R tomcat:tomcat /opt/apache-tomcat-8.5.84
```

Now it's time to login in tomcat user
 
```
su - tomcat
``` 
 
Under the main confiuration file we need create a application user for that fisrt take a backup of file 


Go to the particular directory by running this command 


 ```
cd /opt/apache-tomcat-8.5.84/conf
```

Now take a backup of file 



```
cp tomcat-users.xml tomcat-users.xml.bck
```

We need to define manager-gui role for deployment & also created a user "tomcat". To do this  open tomcat-users.xml  file delete everything  
and past this .  


```<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users>

  <role rolename="manager-gui"/>
  <user username="tomcat" password="tomcat" roles="manager-gui"/>
</tomcat-users>
```

Ok Now we need to allow localhost to access tomcat using gui method .


This will allow tomcat to be accessed from any machine, if we want to grant access to specific IP then use the below value instead 
of allow="^.*$"/>


Go to this location 


```
cd /opt/apache-tomcat-8.5.84/conf/Catalina/localhost
```

In this location we have create a file 

```touch manager.xml
```

Now open file add this  

```
vim manager.xml
```


```
<Context privileged="true" antiResourceLocking="false"
          docBase="${catalina.home}/webapps/manager">
         <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$"/>
        </Context>
```

To save type esc then : then wq  then Enter it will save 

Now it's time to run tomcat 

Change directory 

```
cd /opt/apache-tomcat-8.5.84/bin
```

```
./startup.sh
```

![Tomcta](https://user-images.githubusercontent.com/106643382/219023072-9cc732be-e637-481d-a35b-c376b707bcff.png "Tomcat")



 #### Now create a pipeline in aws 

Go aws console type in search bar code pipeline you will get create pipeline 



![codepipeline](https://user-images.githubusercontent.com/106643382/219033944-4321000f-5c8a-40d5-8fb0-13f9db6fb5b0.png "codepipeline")



![source code](https://user-images.githubusercontent.com/106643382/219034343-90157aab-4485-4b6f-9291-0297ce420242.png "source code")



![code build](https://user-images.githubusercontent.com/106643382/219034969-49304f53-7f26-4b18-b177-19606b14872f.png "code build")



![code deploy](https://user-images.githubusercontent.com/106643382/219035389-d5fd4a45-7cde-44e5-b187-e01bb7a71f43.png "code deploy")



![codecommit](https://user-images.githubusercontent.com/106643382/219035856-841065b3-e44c-4b04-be91-fc147494c8a2.png "codecommit")



![code build](https://user-images.githubusercontent.com/106643382/219036008-06f255bf-47be-48d4-a06d-27e554bb77ec.png "code build")



![code deploy](https://user-images.githubusercontent.com/106643382/219036408-99bed298-55cb-4d15-9f52-be51e54af552.png "code deploy")



Now release the pipeline. When your pipeline is success it will show like this 

![Successful pipeline](https://user-images.githubusercontent.com/106643382/219276474-6751635a-561f-4d6c-8f85-3d9e87d36cbf.png "Successful pipeline")


 #### THANK YOU :)

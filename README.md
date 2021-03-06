# **CICD-Git-Jenkins-Docker**

![CICD](https://user-images.githubusercontent.com/60909862/166105137-66a46c43-46c4-46e8-a034-62f836d0f70d.jpg)


1. Login to Jenkins UI using your jenkins IP address: http://your_ip_address:8080

2. Launch an EC2 instance for Docker host

3. Install the necessary package such as Java
```
sudo apt update
sudo apt install openjdk-11-jdk
```

4. Generate an ssh key on Jenkins user
```
ssh-keygen
```

5. Add the public key to the authorized_keys file of the jenkins user on the agent node (Docker host).

6. Add the Jenkins agent node via the Jenkins UI.
   
   Click on "Manage Jenkins" -> "Manage nodes and clouds" -> "New node".
   
   Enter a Node name -> Click on "Create".
   
   Enter a Remote Work Directory: "/home/ubuntu/jenkins" -> Select Launch method as "Launch agents via SSH" -> Enter Host as "Docker Host Private IP4 address" 
      -> Select Credentials -> Select Host Key Verfification Strategy as "Non verifying Verification Strategy" -> Click on "Save".
      
![Agent1](https://user-images.githubusercontent.com/60909862/166110802-81753741-e554-45da-999f-e7c2ee933d5b.png)

7. Back to Dashboard -> Click on "New Item" at the top left-hand side of your dashboard.

8. Enter the name of the item you want to create -> Select "Freestyle project" -> Click Okay.

9. Enter Project Details
   
   In General 
   
   Click on "GitHub project" and Enter the github repository URL 
   
   Click on "Restrict where this project can be run" and Enter "Agent Name" 
      
![General](https://user-images.githubusercontent.com/60909862/166112589-fa43b780-c5d0-4225-94cc-106afc91243e.png)

   
   In Source Code Management

   Click on Git -> Enter the Github repository URL in "Repository URL" -> Select Branch 
      
![SCM](https://user-images.githubusercontent.com/60909862/166112525-7a693586-7fcf-4ff3-be81-96026d365b16.png)


   In Build Triggers

   Click on "GitHub hook trigger for GITScm pollin" for Event-based Triggers like SCM Actions, WebHooks etc

![Build Triggers](https://user-images.githubusercontent.com/60909862/166112972-0e365fa5-2da9-4bd2-b699-5d1abcc2152e.png)


   In Build

   Select "Execute Shell" -> Enter the below commands
```
#Docker Installation
sudo apt install docker.io -y

#Docker-Compose Installation
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

#To check version of Docker & Docker-Compose 
sudo docker --version
sudo docker-compose --version

#To Delete all container existed 
#sudo docker rm -f $(sudo docker ps -a -q)


#Docker Compose has an -f flag where you can pass in the location of the docker-compose.yml file
sudo docker-compose -f /home/ubuntu/jenkins/workspace/CICD-Pipeline/docker-compose.yml up -d

#This command will ensure all currently running containers will be restarted unless stopped.
#sudo docker update --restart unless-stopped $(sudo docker ps -q)

```

10. When you have entered all the data -> Click Apply -> Save the project.

11. Now, in the main screen, Click the "Build Now" button on the left-hand side to build the source code.

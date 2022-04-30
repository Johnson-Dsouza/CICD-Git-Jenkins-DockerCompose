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
   
   Click on "Manage Jenkins" -> "Manage nodes and clouds" -> "New node". /n
   Enter a Node name -> Click on "Create".
   Enter a Remote Work Directory: "/home/ubuntu/jenkins" -> Select Launch method as "Launch agents via SSH" -> Enter Host as "Docker Host Private IP4 address" 
      -> Select Credentials -> Select Host Key Verfification Strategy as "Non verifying Verification Strategy" -> Click on "Save".
      
![Agent1](https://user-images.githubusercontent.com/60909862/166110802-81753741-e554-45da-999f-e7c2ee933d5b.png)

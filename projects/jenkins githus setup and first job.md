# Set up two server of ubuntu on digital ocean
# tag name dev-server
# tag name jenkins-server
# give ssh key moba
# get access of both servers
# jenkins server
# How To Install Java
    sudo apt update
    java -version
    sudo apt install default-jre
    java -version
    sudo apt install default-jdk
    javac -version
# install Jenkins
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt update
    sudo apt install jenkins
    sudo systemctl start jenkins
    sudo systemctl status jenkins
    ip:8080
# cat commands for password
# copy password
# paste password
# set user password name and email
    admin
    admin
    admin
    admin
    admin@gmail.com
# save
# install initial plugins
# github jenkins connection
# copy ip:8080
# go to -> git repository setting -> Webhooks -> add webhook -> in Payload URL paste ip:8080/github-webhook/ -> Content type json application -> Secret
# for secret key go to jenkins server -> admin -> configure -> generate token -> copy token -> paste on github sercret -> save
# create job manually
    First-Job
    FreeProject
# in Source Code Management -> git -> paste project repository -> Branches to build -> main -> save -> build Job -> inspect Status Changes Console Output
# -> First-Job -> configure -> Build periodically * * * * * -> save -> wait build run each minute
# -> First-Job -> configure -> Poll SCM * * * * * -> job run after changes in code
# dev-server
    git clone https://github.com/actuaryhafeez/Java.git
    cd Java
    ls
    nano simple.java
    Welcome To Jenkins server for Devops
# save
    git add simple.java
    git commit -m "Devops Server" simple.java
    git push origin main
    actuaryhafeez
    paste key
# go to jenkins server -> First-Job -> Job would be built
   
    
   

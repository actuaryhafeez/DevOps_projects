# create 4 servers on digital ocean
# Jenkins-server -> ansible-server -> docker-server -> dev-server
# get access with mobaxterm
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
    hostname -I
    ip:8080
# cat commands for password
# copy passwordifconfig
# paste password
# install customised plugins
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

# set passwordless connection between jenkins and ansible server
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
    ssh-keygen
    hostname -I # ansible-server
    ssh-copy-id -i root@ansibleip # denied set root access allow on ansible
    # check connection
    ssh root@ipansible
    exit
# install plugin  Publish Over SSH
    -> manage jenkins -> manage plugins -> -> availbe -> Publish Over SSH -> install without restart
 # add connection of jenkins on jenkins
     manage jenkins -> configure system -> at end of page Publish Over SSH is avai;able -> SSH Servers add
     name-> jenkins
     hostname-> ip
     user->root
     -> advanced setting
     -> password authentication give password of root

 # add connection of ansible on jenkins
     manage jenkins -> configure system -> at end of page Publish Over SSH is avai;able -> SSH Servers add
     name-> ansible
     hostname-> ip
     user->root
     -> advanced setting
     -> password authentication give password of root
 # after configuring servers
 # now create job on jenkins
 -> create new job -> name: repo name -> Free style -> Source Code Management: git: paste repo link: check branch name -> Build Triggers: GitHub hook trigger for GITScm polling -> Build: Send files or execute commands over SSH : choose jenkins serve : exec command : rsync -avh /var/lib/jenkins/workspace/docker-project/Dockerfile root@ansibleip:/opt/
-> Post-build Actions: Send build artifacts over SSH: name ansible: exe command:
cd /opt
docker image build -t $JOB_NAME:v1.$BUILD_ID
docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:V1.$BUILD_ID 
docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:latest
docker image push hafeezabdul/$JOB_NAME:v1.$BUILD_ID
docker image push hafeezabdul/$JOB_NAME:latest
docker image rmi $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:V1.$BUILD_ID hafeezabdul/$JOB_NAME:latest

-> Post-build Actions: Send build artifacts over SSH: name ansible: exe command: ansible-playbook /sourcecode/docker.yml
# save and build
# check in ansible serve 
    ls /opt/ # Dockerfile file 

# https://dockerserverip
# now go go dev server and change source code file
# check https://dockerserverip


# Ansible Server configuration
    sudo apt update
    sudo apt install ansible
    sudo nano /etc/ansible/hosts
    #[webservers]
    [dohostcker]
    165.22.193.211 # private ip public ip both accesible
    ansible-inventory --list -y
    # ssh-keygen connection between docker-ansible
    ssh-keygen
    ssh-copy-id -i root@ipwebserver
    # set root password and allow root access on webserver
    # go to webserver
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
    # check access make sure ansible is also installed on webser
    ansible all -m ping -u root
# write playbook
    mkdir /sourcecode
    cd /sourcecode
    nano docker.yml
    - hosts: all
      tasks:
         - name: stop container
           shell: docker container stop docker-container
         - name: remove container
           shell: docker container rm docker-container
         - name: remove docker image
           shell: docker image rm hafeezabdul/docker-project:latest
         - name: create container
           shell: docker container run -itd --name docker-container -p 80:80 hafeezabdul/docker-project
      
# docker server configuration
    sudo apt update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
    sudo apt update
    apt-cache policy docker-ce
    sudo apt install docker-ce
    sudo systemctl status docker
    sudo usermod -aG docker ${USER}
    su - ${USER}
    id -nG
    docker
    # install ansible 
    sudo apt install ansible
    udo apt install apache2
    sudo systemctl status apache2
# set root access and ssh passwordless seting
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
# docker login
    docker login
    hafeezabdul
    Zigzag2510
   
   # dev-server
     mkdir /devops
     cd /devops
     git clone https://github.com/actuaryhafeez/docker-project.git
     cd docker-project/
     ls
     nano Dockerfile
 
    FROM centos:7
    MAINTAINER ahdatascientist@gmail.com
    RUN yum install -y httpd \
    	zip \
    	unzip
    ADD https://www.free-css.com/assets/files/free-css-templates/download/page277/royal.zip /var/www/html
    WORKDIR /var/www/html
    RUN unzip royal.zip
    RUN cp -rvf royal/* .
    RUN rm -rf royal.zip
    CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    EXPOSE 80
    # save
     git status
     git add Dockerfile
     git config --global user.name "ah"
     git config --global user.email "ahdatascientist@gmail.com"
     git commit -m "new code"
     git push origin main
     actuaryhafeez
     token
 # update source code
 nano Dockerfile
 
 FROM centos:7
    MAINTAINER ahdatascientist@gmail.com
    RUN yum install -y httpd \
    	zip \
    	unzip
    ADD https://www.free-css.com/assets/files/free-css-templates/download/page254/photogenic.zip /var/www/html
    WORKDIR /var/www/html
    RUN unzip photogenic.zip
    RUN cp -rvf photogenic/* .
    RUN rm -rf photogenic photogenic.zip
    CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    EXPOSE 80

 # go to jenkins server demo-project -> new job would run
 # go to http://dockerserverip

    

# create 7 servers on digital ocean
# -> k8s-control k8s-worker-1 k8s-worker-2 -> Jenkins-server -> ansible-server -> docker-server -> dev-server
# get access with mobaxterm
# jubertnetes control panel
    nano /opt/deployment.yml
    
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: kuber
spec:
  selector:
    matchLabels:
      app: kuber
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate: 
      maxSurge: 1
      maxUnavailable: 1

template:
  metadata: 
   labels:
    app: kuber
  spec:
    containers:
    - name: kuber 
      image: hafeezabdul/kubernetesproject
      imagePullPolicy: Always
      ports:
      - containerPort: 80
  
      nano /opt/service.yml
      
apiVersion: v1
kind: Service
metadata:
  name: kuber
  labels:
    app: kuber
spec:
  selector:
    app: kuber
  type: LoadBalancer
  ports:
    - port: 8080
      targetPort: 80 
      nodePort: 31200
      
# passwd setting
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
    
      
    
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
-> Post-build Actions: Send build artifacts over SSH: name jenkins: exe command: rsync -avh /var/lib/jenkins/workspace/kubernetesproject/Dockerfile root@ansibleip:/opt/
 # now create job on ansible
 -> create new job -> name: repo name -> Free style -> Source Code Management: git: paste repo link: check branch name -> Build Triggers: GitHub hook trigger for GITScm polling -> Build: Send files or execute commands over SSH : choose jenkins serve : exec command : rsync -avh /var/lib/jenkins/workspace/docker-project/Dockerfile root@ansibleip:/opt/
-> Post-build Actions: Send build artifacts over SSH: name ansible: exe command:
cd /opt
docker image build -t $JOB_NAME:v1.$BUILD_ID
docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:V1.$BUILD_ID 
docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:latest
docker image push hafeezabdul/$JOB_NAME:v1.$BUILD_ID
docker image push hafeezabdul/$JOB_NAME:latest
docker image rmi $JOB_NAME:v1.$BUILD_ID hafeezabdul/SJOB_NAME:V1.$BUILD_ID hafeezabdul/$JOB_NAME:latest

-> Post-build Actions: Send build artifacts over SSH: name ansible: exe command: ansible-playbook /opt/ansible.yml
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
    [kube]
    165.22.193.211 # private ip public ip both accesible
    ansible-inventory --list -y
    # ssh-keygen connection between docker-ansible
    ssh-keygen
    ssh-copy-id -i root@k8sip # 
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

    nano /opt/ansible.yml
    - hosts: all
      become: true
      
    - tasks:
      #- name: delete old deployment
        #command: kubectl delete -f /opt/deployment.yml
      #- name: create service
       # command: kubectl delete -f /opt/service.yml
      - name: create new deployment
        command: kubectl apply -f /opt/deployment.yml
      - name: create new service
        command: kubectl apply -f /opt/service.yml
      
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

    

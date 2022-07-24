# create 4 servers on digital ocean
# Jenkins-server -> ansible-server -> web-server -> dev-server
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
    ip:8080
# cat commands for password
# copy password
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
    ssh-keygen
    ssh-copy-id -i root@ansibleip # denied set root access allow on ansible
    # check connection
    ssh root@ipansible
    exit
    # go to ansible
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
  
 # root and ssh seting on jenkins
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
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
 -> create new job -> name: repo name -> Free style -> Source Code Management: git: paste repo link: check branch name -> Build Triggers: GitHub hook trigger for GITScm polling -> Build: Send files or execute commands over SSH : choose jenkins serve : exec command : rsync -avh /var/lib/jenkins/workspace/demo-project/*.html root@ansibleip:/opt/index.html
-> Post-build Actions: Send build artifacts over SSH: name ansible: exe command: ansible-playbook /sourcecode/playbook.yml
# save and build
# check in ansible serve 
    ls /opt/ # index.html file 
# check in web server
    ls /var/www/html/ # index.html
# https://ipweb
# now go go dev server and change source code file
# check https://ipweb
 
     
# Andible Server configuration
    sudo apt update
    sudo apt install ansible
    sudo nano /etc/ansible/hosts
    #[webservers]
    [web]
    165.22.193.211 # private ip public ip both accesible
    ansible-inventory --list -y
    # ssh-keygen connection between webserver-ansible
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
    nano playbook.yml
   - hosts: all
     tasks:
        - copy:
             src: /opt/index.html
             dest: /var/www/html
        
# Web Server
    sudo apt update
    sudo apt install apache2
    sudo systemctl status apache2
    hostname -I
    sudo apt install ansible
    # set root access and ssh passwordless seting
    passwd root
    set passwd
    nano /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    sudo systemctl restart sshd
 
# webserver
     mkdir /devops
     cd /devops
     git clone https://github.com/actuaryhafeez/demo-project.git
     cd demo-project/
     ls
     nano index.html
     I am new devops engineer from Pakistan
     git status
     git add index.html
     git config --global user.name "ah"
     git config --global user.email "ahdatascientist@gmail.com"
     git commit -m "new code"
     git push origin main
     actuaryhafeez
     token
 # go to jenkins server demo-project -> new job would run
 # go to http://webip

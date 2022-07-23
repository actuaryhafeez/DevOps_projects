# setup three server
# 1 master server
# 2 client server
# git server 
    nmcli connection add con-name dhcp ifname ens33 type ethernet autoconnect yes
    nmcli connection up dhcp
    ping www.google.com
    yum install git
    mkdir /project
    cd /project
    git init --bare
    
    
# git client
    nmcli connection add con-name dhcp ifname ens33 type ethernet autoconnect yes
    nmcli connection up dhcp
    ping www.google.com
    yum install git
    mkdir /dev-1
    cd /dev-1
    git init
    vim code
    Welcome to DevOps
    :wq
    git status
    git add code
     git config --global user.email "dev-1@gmail.com"
     git config --global user.name "dev-2"
    git commit -m "first commit"
    git remote add origin root@ip:/project
    git push origin master
    ssh-keygen
    nano .ssh/authorised_keys
     copy key
 # go to git server
# got git server
    mkdir .ssh
    cd .ssh
    nano authorized_keys
    paste rsa pub key
# git client dev-1
    git push origin main
# git server
     git log
     git show
# git client dev-2
    
     mkdir /dev-2
     cd /dev-2
     git init
     git config --global user.email "dev-2@gmail.com"
     git config --global user.name "dev-2"
     git remote add origin root@ip:/project
     ssh-keygen
     cat /root/.ssh/id_rsa.pub
     copy key
 # go to git server
     cd
     nano .ssh/authorised_keys
     paste key
     save
 # back to dev-2
     git pull origin main
     ls
     
     
    
    
    
    
    
    
    

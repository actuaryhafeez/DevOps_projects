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
    git branch
    
    
# git dev-1
    #nmcli connection add con-name dhcp ifname ens33 type ethernet autoconnect yes
    #nmcli connection up dhcp
    #ping www.google.com
    #yum install git
    ssh-keygen
    nano .ssh/authorised_keys
     copy key
 # go to git server
    #mkdir .ssh
    #cd .ssh
    cd
    nano .ssh/authorized_keys
    paste rsa pub key
# git dev-2
    #nmcli connection add con-name dhcp ifname ens33 type ethernet autoconnect yes
    #nmcli connection up dhcp
    #ping www.google.com
    #yum install git
    ssh-keygen
    nano .ssh/authorised_keys
     copy key
 # go to git server
    #mkdir .ssh
    #cd .ssh
    cd
    nano .ssh/authorized_keys
    paste rsa pub key
# git devops engineer 
    #nmcli connection add con-name dhcp ifname ens33 type ethernet autoconnect yes
    #nmcli connection up dhcp
    #ping www.google.com
    #yum install git
    ssh-keygen
    nano .ssh/authorised_keys
     copy key
 # go to git server
    #mkdir .ssh
    #cd .ssh
    cd
    nano .ssh/authorized_keys
    paste rsa pub key
 # now dev-1
    mkdir /dev-1
    cd /dev-1
    git clone root@ip:/project
    ls
    cd project
    git branch
    git config --global user.email "dev-1@gmail.com"
    git config --global user.name "dev-1"
    nano index.html
    Welcome to software version 1
    # save
    git status
    git add index.html
    git commit -m "first" index.html
    git branch
    git push origin master
# git server
    cd /prject
    git branch
    git log
    git show
# git dev-2
     mkdir /dev-2
     cd /dev-2
     git clone root@ip:/project
    ls
    cd project
    git config --global user.email "dev-2@gmail.com"
    git config --global user.name "dev-2"
    git branch
    ls
    nano index.html
    welcome to software version 2
    # save
    git status
    git add index.html
    git commit -m "second" index.html
    git push origin master
  # git server
    cd /prject
    git branch
    git log
    git show
 # now make new branch scenrio staging
 # new branch can be made by anyone but normally devops engineer make it
 # git server
     git branch
     git branch staging
     git branch 
     git log
     git log staging
 # dev-1 server
     git branch
     git fetch
     git branch -a
     git checkout staging
     git branch
     nano ver1.index
     welcome to software 1.1
     # save
     git status
     git add ver1.index
     git commit -m "dev-1" ver1.index
     git push origin staging
 # git server 
     git branch
     git log
     git log staging
     git show staging
     git checkout staging # switching to staging not allowed
     git merge staging maste # not allowed
 # devops server
     mkdir /devops
     cd /devops
     git clone root@ip:/project
     git branch
     git checkout master
     git merge staging
     git push origin master
 # git server
    git log master
    git log staging
 # dev-2
     git branch
     git fetch
     git branch -a
     git checkout staging
     git branch
     nano ver1.index
     welcome to software 1.2
     # save
     git status
     git add ver1.index
     git commit -m "dev-2" ver1.index
     git push origin staging
 # git server
       git log master
       git log staging
  # devops server
     git fetch
     git branch
     git checkout master
     git merge staging
     git push origin master
 # git server
       git log master
       git log staging
 # dev-1
     git branch
     git branch checkout staging
     git pull origin staging
     cat ver1.index
# scenrio each dev works in their own dev branch
# dev-1
     git branch 
     git branch dev-1
     git checkout dev-1
     nano vlc1.html
     Welcome to software vlc version 1.1
     git add vlc1.html
     git commit -m "vlc-1-dev-1" vlc1.html
     git push origin dev-1
 # git server 
     git branch
     git log master
     git log staging
     git log dev-1
 # dev-1 
     git branch 
     nano vlc1.html
     Welcome to software vlc version 1.2
     Welcome to software vlc version 1.3
     # save
     git add vlc1.html
     git commit -m "vlc-1.2-1.3-dev-1" vlc1.html
     git push origin dev-1
 # git server 
     git branch
     git log master
     git log staging
     git log dev-1
 # git devops engineer
     git checkout staging
     git merge dev-1
     git push origin staging
 # git server 
     git branch
     git log master
     git log staging
     git log dev-1
 # git devops engineer
     git checkout master 
     git merge staging
     git push origin master
  # git server 
     git branch
     git log master
     git log staging
     git log dev-1
 # dev-2 
     git fetch or git clone root@188.166.5.237:/project
     git branch -a
     git checkout dev-1
     ls
     cat vlc1.html
     git branch dev-2
     git checkout dev-2
     nano vlc2.html
     Welcome to 
     Welcome to software vlc version 2.1
     Welcome to software vlc version 2.2
     Welcome to software vlc version 2.3
     # save
     git add vlc2.html
     git commit -m "vlc-2.1-2.2-2.3-dev-2" vlc2.html
     git push origin dev-2
  # git server 
     git branch
     git log master
     git log staging
     git log dev-2
 # git devops engineer
     git fetch
     git branch -a
     git checkout dev-2
     git checkout staging 
     git merge dev-2
     git push origin staging
  # git server 
     git branch
     git log staging
     git log dev-2
 # git devops engineer
     git checkout master 
     git merge staging
     git push origin master
   # git server 
     git log master
     git log staging
 # git devops engineer
     git fetch
     git branch -D dev-1
     git branch -D dev-2
     git branch -D staging
     git branch
     git push origin --delete dev-1
     git push origin --delete dev-2
     git push origin --delete staging
     git branch -a
   # git server
       git branch

update:     

    yum -y update
    
Ansible is part of the Extra Packages for Enterprise Linux (EPEL) repository so you need to install epel-release package first:

    yum install epel-release

or

    yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

So now you can go ahead and install ansible:

yum install ansible -y

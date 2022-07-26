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


# install docker on jenkins
# docker installation on jenkins
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
    chmod 777 /var/run/docker.sock
    sudo systemctl start docker
    sudo systemctl enable docker
   
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
    chmod 777 /var/run/docker.sock
    sudo systemctl start docker
    sudo systemctl enable docker

# create job -> scripted-pipeline-demo -> pipeline



node{
stage("Pull Source Code From GITHUB"){
     git branch: 'main', url: 'https://github.com/actuaryhafeez/scripted-pipeline-demo.git'
   }

stage("Build Docker File"){
     sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
     sh 'docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:v1.$BUILD_ID' 
     sh 'docker image tag $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:latest'
   }    
   
stage("Push Image To Docker Hub"){
     withCredentials([string(credentialsId: 'dockerhubpasswd', variable: 'dockerhubpasswd')]) {
    // some block
     sh 'docker login -u hafeezabdul -p ${dockerhubpasswd}'
     sh 'docker image push hafeezabdul/$JOB_NAME:v1.$BUILD_ID'
     sh 'docker image push hafeezabdul/$JOB_NAME:latest'
     sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:v1.$BUILD_ID hafeezabdul/$JOB_NAME:latest'
}

   }
stage("Deployment of Docker Container"){
     def dockerrun = 'docker container run -itd --name docker-container -p 80:80 hafeezabdul/scripted-pipeline-demo:latest'
     def dockerrm = 'docker container rm -f docker-container'
     def dockerimgrm = 'docker image rmi hafeezabdul/scripted-pipeline-demo:latest'
     sshagent(['docker-server-passwd']) {
    // some block
     sh "ssh -o StrictHostKeyChecking=no root@104.248.204.150 ${dockerrm}"
     sh "ssh -o StrictHostKeyChecking=no root@104.248.204.150 ${dockerimgrm}"
     sh "ssh -o StrictHostKeyChecking=no root@104.248.204.150 ${dockerrun}"
    
}
   }


}


# generete secrete key for docker hub login
withCredentials: Bind credential to varaible -> variable: dockerhubpasswd -> add: secrete text -> variable: dockerhubpasswd -> add:jenkins -> kind: secrete text -> secrete: docker hub passwd -> ID: dockerhubpasswd -> description: dockerhubpasswd -> add -> generete -> copy

# install plugin ssh agent for deplyment 
# generate code for ssagent
  -> sshagent -> add: jenkins -> Kind: ssh with key -> id: docker-server-passwd -> user: root -> private key add : mobxterm private key copy paste-> generate script 

    
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

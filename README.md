# Valexy_devops_project
Step 1: Create one Linux instance.
Step 2: In Linux instance install Jenkins and GIT.
Step 3: setup maven
Step 4: Install plugins: git, maven on jenkins
Step 5: install maven:
copy maven url from below link 
https://maven.apache.org/download.cgi
#sudo su 
#cd /opt 
#wget https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
Un-zip the package.
# tar -xvzf apache-maven-3.8.4-bin.tar.gz
#ls 
#mv apache-maven-3.8.4 maven maven 
#cd maven
#cd bin 
#ll
# ./mvn -v (to start the maven)

Step 6: make it available for the root user. update under bash_profile  
#cd ~
#pwd
#ll -a
NEED TO ADD THE JAVA PATH AS WELL AS THE  M2  AND M2_HOME PATHS.
#bash_profile (we need to edit it)

#vi bash_profile  
=============================================
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH

=================
### fi k niche paste it ###
M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64
# User specific environment and startup programs

PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

Step 7: 
Now set path for java also 
#sudo su 
#cd jvm 
#find / -name jvm
#cd /usr/lib/jvm
#ll
#find / -name java-11*
copy path (/usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64)
============================
SAVE THE ABOVE FILE USING  :WQ

Step 8:
https://aws.amazon.com/amazon-linux-2/
[ec2-user@jenkins-server ~]$ sudo su
[root@jenkins-server ec2-user]# source .bash_profile
[root@jenkins-server ec2-user]# echo $PATH
/sbin:/bin:/usr/sbin:/usr/bin:/root/.local/bin:/root/bin
[root@jenkins-server ec2-user]# pwd
/home/ec2-user
[root@jenkins-server ec2-user]# sudo su -
Last login: Fri Mar 11 06:48:26 UTC 2022 on pts/0
[root@jenkins-server ~]# pwd
/root
[root@jenkins-server ~]# source .bash_profile
[root@jenkins-server ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64:/opt/maven:/opt/maven/bin:/root/bin:/usr/lib/jvm/
java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64:/opt/maven:/opt/maven/bin
[root@jenkins-server ~]# mvn --version
Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
Maven home: /opt/maven
Java version: 11.0.13, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.102-99.473.amzn2.x86_64", arch: "amd64", family: "unix"
[root@jenkins-server ~]# pwd
/root
Go to Jenkins UI > download maven plugin 
> manage jenkin > jenkins installation > maven installation 
unchecked the box , install automatically 
Name - java-11
JAVA_HOME - /usr/share/doc/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64

Now add maven on jenkin UI
Name - maven
MAVEN_HOME - /opt/maven 
Apply and save 

Step 9: 
Tomact installation 

Step 10:
Integration of tomcat with jenkins:::::
Install plugin "deploy to container"
manage jenkins > manage credentials > jenkins > global credentials > add credentials 
kind - username and pw 
id & desc - tomcat_deployer 
uname - deployer 
pw - deployer 
save it 

Step 11: To deploy war file on tomcat
Now, create new job name it - buildanddeploy (maven project)
add git repo 
branch - master 
clean install 
post build action - deploy war/ear to a container 
war file - webapp/
                              ======================== DOCKER SETUP ========================

STEP 1:
>>LINUX INSTANCE >>>INSTALL DOCKER >>START DOCKER CONTAINER>>NORMAL DOCKER COMMANDS FOR PRACTICE 

STEP 2: 
>>Docker image form the docker hub. Initially using docker hub for the image.
>>To rename your host name: 

# vi /etc/hostname

>>Edit the name in the file and :wq
>>In-order to make it active we need to re-boot the system : 
#init6 (instance was not able to start after running this command not recommended to run)

DOCKER PULL: 
>>FIRST WE NEED TO START THE DOCKER SERVICE
#service docker start
#docker pull tomcat
#docker images  >>> tomcat image and image id in O/P
#docker run -d --name tomcat container -p 8081:8080 tomcat
>>Container runs inside your docker host. Need to map it to external port:8081, internally running on port 8080
#docker ps -a (to list containers)
>>To access this container outside form the container: 
>>Take public id: 8081 (need to open this port on the security group: select 8081-9000 to open more posts until 9000)
>>NOW TAKE THE PUBLIC IP :8081

>>FIXING TOMCAT ACCESS ISSUE ON THE BROWSER THAT WE GOT:  "HTTP Status 404 – Not Found"
>>TO CONNECT TO THE CONTAINER: 
#docker exec -it tomcat-container /bin/bash
#ls
#cd webapps.dist
#ls   
>>(The content which is inside this we need to copy it to our "webapps" folder than only we will be able to access our app)
# cp -R * ../webapps
cp=copy
-R=recursive
* in the current directory whatever is there 
.. 1 folder above in webapps directory

NOW WE WILL BE ABLE TO IT FROM THE BROWSER: 
http://3.109.203.23:8081/
You'll be able to see the tomcat webpage by refreshing this page which was throwing an error earlier. 

The changes will be temporary. if the instance is re-created.

STEPS FROM THE TERMINAL ITSELF: 

root@c5d0a32da6b5:~# exit
exit
[root@docker_host ec2-user]# docker ps -a

CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS                                       NAMES
c5d0a32da6b5   tomcat    "catalina.sh run"   10 minutes ago   Up 10 minutes   0.0.0.0:8081->8080/tcp, :::8081->8080/tcp   tomcat-container

[root@docker_host ec2-user]# docker stop tomcat-container
tomcat-container
[root@docker_host ec2-user]# docker ps -a
CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS                       PORTS     NAMES
c5d0a32da6b5   tomcat    "catalina.sh run"   11 minutes ago   Exited (143) 4 seconds ago             tomcat-container
[root@docker_host ec2-user]# docker ps 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES


WE HAVE UPDATED THE CHANGES ONLY ON THE DOCKER CONTAINER. 
WHENEVER WE LAUNCH A NEW CONTAINER WITH THE IMAGE. CHANGES WILL NOT COME UP ON NEW IMAGES. YOU WILL GET THE SAME ERROR : HTTP 404 NOT FOUND.
http://3.109.203.23:8082/
FOR THIS WE LAUNCHED A NEW CONTAINER USING PORT 8082 THIS TIME/EARLIER WE RAN IT ON 8081 (TOMCAT-CONTAINER): 
NEW CONTAINER: TOMCAT-2
docker run -d --name tomcat-2 -p 8082:8080 tomcat:latest

>>TO RESOLVE THIS : 
WE ARE GOING TO CREATE A DOCKER FILE >>PULL TOMCAT IMAGE>>CUSTOMIZATION: COPYING THE FILE FROM WEB-APP DIST TO WEB-APPS LIKE WE DID EARLIER. CONTENT WILL BE ACCESIBLE UNDER WEB-APPS DIRECTORY. FROM HERE IF WE LAUNCH NEW CONTAINER WILL NOT SEE THE SAME ERROR. 

>>>IMPORTANT DOCKER INSTRUCTIONS: 
FROM, RUN, CMD , ENTRYPOINT, WORKDIR, COPY, ADD, EXPOSE, ENV
SEE SIMPLE DEVOPS PROJECT GIT HUB REPO FOR DETAILED INFORMATION.

INSTALLING TOMCAT ON CENTOS: 
1. PULL CENTOS FROM DOCKER HUB                           >>>>>FROM 
2. INSTALL JAVA                                                                >>>>>>RUN
3. create this directory: /opt/tomcat DIRECTORY               >>>>>>RUN
4. CHANGE WORK DIR TO /opt/tomcat (going inside dir) >>>>WORKDIR
5. Download tomcat packages : tomcat download   >>>>ADD OR RUN COMMAND
6. Need to extract: taz.gz file                                     >>>>>RUN
7. re-name the tomcat directory.                               >>>>>RUN
8. Port no:8080                                                          >>>>>EXPOSE
9. Start the tomcat services.                                     >>>>>CMD


COMMANDS: 
vi Dockerfile (EVERYTHING IN UPPER CASE)

FROM centos:latest
RUN yum install java -y
RUN mkdir /opt/tomcat
WORKDIR /opt/tomcat
ADD https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.59/bin/apache-tomcat-9.0.59.tar.gz .
RUNtaz-xvzfapache-tomcat-9.0.59.tar.gz
RUNmvapache-tomcat-9.0.59 /*  /opt/tomcat
EXPOSE8080
CMD ["/opt/tomcat/bin/catalina.sh", "run" ]

NOW TO CREATE IMAGE OUT OF IT: 
#docker build -t mytomcat .     (dot for the current directory)
#docker images






---===============DOCKER 2==================

SCRIPT TO LAUNCH AN INSTANCE: 
#!/bin/bash
# sudo yum update -y
# sudo yum install docker.io
# sudo service docker start
# service docker status


STEP 2: 
>> To rename your host name >>
# vi /etc/hostname

>>Edit the name in the file and :wq
>>In-order to make it active we need to re-boot the system : 
#init6 (instance was not able to start after running this command not recommended to run)
>>refresh the page.

STEP 3:
>>DOCKER PULL >>

>>FIRST WE NEED TO START THE DOCKER SERVICE:
#service docker start
#docker pull tomcat
#docker images  >>> tomcat image and image id in O/P

NEED TO CREATE A DOCKER USER:
#useradd dockeradmin
#passwd dockeradmin   ~~docker
# usermod -aG docker dockeradmin
RUN THE BELOW COMMAND TO EDIT THE PERMISSIONS: (Commenting and un-commenting)
#vi /etc/ssh/sshd_config
#NEED TO RE-START THE SERVICES: 
#service sshd reload    (Redirecting to /bin/systemctl reload sshd.service will show up in O/P)

NOW OPEN THE DUPLICATE SESSION: 
# su dockeradmin
IT WILL ASK FOR A PASSWD FOR THE DOCKER ADMIN 

NOW NEED TO INTEGRATE THIS DOCKER HOST WITH THE JENKINS, NEED TO INSTALL A PLUGIN FOR THAT: 
>GO TO JENKINS DASHBOARD  
> GO TO MANAGE JENKINS  
> MANAGE PLUGINS: LOOK FOR "Publish Over SSH"
> INSTALL WITHOUT RESTART

ADDING SSH SERVER:
MANAGE JENKINS >>> CONFIGURE SYSTEM 
-SCROLL DOWN TO MAKE THE CHANGES. 
-WE NEED TO PUT THE HOST NAME WHICH WE SELECTED EARLIER IN THE STEP 2 >>> SSH SERVER NAME : DOCKER
- NOW PUT THE PRIVATE IP OF YOUR DOCKER INSTANCE IN HOST NAME
- USERNAME: dockeradmin 
- NOW SELECT THE ADVANCED OPTION ON RIGHT SIDE.
- Passphrase / Password: docker (this passwd was set in earlier steps)
- SCROLL DOWN AND LOOK FOR "TEST CONNECTION"
- SUCCESS

CREATING A DOCKER DIRECTORY: 
#sudo su 
#cd /opt
# mkdir docker

RUN: 
#chown -R dockeradmin:dockeradmin docker

NOW, CREATE NEW JOB IN JENKINS "BuildAndDeployJobOnContainer"
Source Code Management: GIT URL 
Branch Specifier: /MASTER
Build Triggers: Build whenever a SNAPSHOT dependency is built (SELECT IT)
SELECT POLL SCM AND PUT 5 STARS.  *****
Build: pom.xml
Goals and options: clean install
**Post-build Actions: (post steps)
- Name: DOCKER

**** Transfers: 
- Source files: webapp/target/*.war
- Remove prefix: webapp/target
- Remote directory: //opt//docker

**Post-build Actions:


Name: DOCKER

****Exec command: 

cd /opt/docker;
docker build -t regapp:v1 .;    (MAKE SURE YOU ARE GIVING SPACE HERE OTHERWISE YOU'LL GET ERRORS WHILE BUILD)
docker stop registerapp;
docker rm registerapp;         
docker run -d --name registerapp -p 8087:8080 regapp:v1


CREATE A DOCKERFILE: 

cd /opt/docker/Dockerfile

FROM tomcat:latest
RUN cp-R/usr/local/tomcat/webapps.dist/*/usr/local/tomcat/webapps
COPY./*.war /usr/local/tomcat/webapps


BUILD THE JOB. 

CHECK THE WEBSITE ON YOUR DOCKER SERVER. 




                                        
---===============ANSIBLE==================
UNTIL NOW WE WERE DOING THE DEPLOYMENT USING DOCKER. NOW WE ARE IMPLEMENTING ANSIBLE INTO THIS PIPELINE.
ANSIBLE PLAYBOOKS WILL CREATE A CONTAINER OUT OF THE IMAGE.

1. LAUNCH AN EC2 INSTANCE AND INSTALL ANSIBLE ON IT. 
2. Sec group: All Traffic  
3. Launch Instance
# sudo amazon-linux-extras install ansible2
# vi /etc/hostname  (Change the hostname to: ansible-server)
# init 6  (to apply the changes in the hostname)
# useradd ansadmin
# passwd ansadmin   (set a password : ansible)
#  visudo
{
---Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
ansibleadmin    ALL=(ALL)        NOPASSWD: ALL
}
**We have added the user which we created in the last step.
# vi /etc/ssh/sshd_config 
>>>>
To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication no
#PermitEmptyPasswords no
PasswordAuthentication yes
>>>>  :wq
# systemctl restart sshd
# su - ansadmin
# ssh-keygen     (to generate keys)

======================    *****  INTEGRATION OF ANSIBLE WITH DOCKER  *****  =============================

# START your docker server. 
# # useradd ansadmin
# passwd ansadmin   (set a password : ansible)
#  visudo
{
---Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
ansibleadmin    ALL=(ALL)        NOPASSWD: ALL
}
# grep Password /etc/ssh/sshd_config 

 ===========GO TO YOUR ANSIBLE SERVER==============
# vi /etc/ansible/hosts
# CLEAR THE CONTENT IN THIS FILE AND WE NEED TO PASTE THE PRIVATE IP OF THAT INSTANCE HERE. 
commands for vi: gg >> up      clear >> d+G 
# su ansadmin
# ssh-copy-id 172.31.38.12
# ssh 172.31.38.12      (You'll enter into the docker server using this command)  
#exit
#ansible all -m ping
[WARNING]: Platform linux on host 172.31.38.12 is using the discovered Python interpreter at /usr/bin/python, but future installation of another
Python interpreter could change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
172.31.38.12 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
# ansible all -m command -a uptime


==========================   INTEGRATING ANSIBLE WITH JENKINS   =======================

To add an Ansible server into Jenkins:

# Go to manage Jenkins  >> configure system 
# scroll down and select ADD TAB : (SSH SERVER)
Name: Ansible server
Hostname:  PRIVATE IP OF ANSIBLE SERVER
Username: ansadmin
SCROLL DOWN AND SELECT ADVANCED SETTINGS AND PUT PASSWORD FOR THE ANSIBLE SERVER. 
Passphrase / Password: ansible
>> TEST CONFIGURATION  >>>  APPLY >> SAVE
INTEGRATION OF ANSIBLE AND JENKINS IS SUCCESSFULLY DONE.

# GO TO JENKINS AND CREATE A NEW JOB. (WE ARE COPYING WAR FILE ON ANSIBLE SERVER, JUST LIKE WE DID WHILE INTEGRATING DOCKER)

#NAME: Copy_Artifacts_onto_Ansible
# SCROLL DOEN AND SELECT COPY FROM OPTION AT VERY LAST. 
#COPY FORM: BuildAndDeployJobOnContainer

GO TO POST-BUILD ACTIONS IN THE JOB 
CHANGE NAME OF THE SSH SERVER TO ANSIBLE SERVER

NOW GOT O YOUR ANSIBLE SERVER (CMD)
>  cd /opt
> mkdir docker

Change the ownership of the docker directory bu running this command

# chown ansadmin:ansadmin docker
remove everything from : Exec command
apply and save
# BUILD THE JOB AND CHECK ON THE ANSIBLE SERVER FOR THE WAR FILE  . 
>> GO TO ANSIBLE SERVER
#cd /opt
# cd docker
#ll

OUTPUT  : -rw-rw-r-- 1 ansadmin ansadmin 2868 Mar 22 15:27 webapp.war

================== NOW WE NEED TO INSTALL DOCKER ON THIS ANSIBLE SERVER =======================

# yum install docker -y
NOW ADDING ANSADMIN USER TO THE DOCKER GROUP SO THAT THIS USER WILL BE ABLE TO PERFORM DOCKER OPERATIONS AS WELL. 
# sudo usermod -aG docker ansadmin
# id ansadmin to check 
# START THE DOCKER SERVICE AND CREATE A DOCKER FILE IN THE ANSIBLE SERVER
#cd /opt/docker
# ll
# vi Dockerfile

================== DOCKERFILE CONTENT =======================

FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps

=============================================================

FROM ANSIBLE INSTRUCT DOCKER GO AND PULL THE IMAGE FROM THE HUB AND CREATE THE CONTAINER. 
APP IS RUNNING ON THE DOCKER HOST. AND USER CAN ACCESS THE APPLICATION.

#ON ANSIBLE SERVER WE WILL BE CREATING A PLAYBOOK WHICH WILL COPY ARTIFACTS AND RUN THE DOCKERFILE ON TARGET HOST THAT IS ON DOCKER SERVER. 

===================================

# su - ansadmin
# cd /opt/docker
# ll

===============O/P===============
-rw-r--r-- 1 root     root      127 Mar 22 15:36 Dockerfile
-rw-rw-r-- 1 ansadmin ansadmin 2868 Mar 22 15:27 webapp.war
===================================
# ROOT
# su - ansadmin
# vi /etc/ansible/hosts
SHOULD LOOK LIKE:
==============PRIVATE IP'S OF DOCKER AND ANSIBLE SERVERS========================= 
[dockerhost]
172.31.38.12
[ansible]
172.31.36.254
=======================================
# :wq
# su - ansadmin
# cd /opt/docker
# ssh-copy-id 172.31.36.254  (Private IP OF ANSIBLE SERVER ITSELF)
# ansible all -a uptime  (YOU'LL SEE BOTH THE IPS OF ANSIBLE AND DOCKER SERVERS)
# cd /opt/docker
# ll
# vi regapp.yml
============================================

---
- hosts: ansible 
  tasks:
- name: create a docker image
command: docker build -t regapp:latest
args:
chdir: /opt/docker

=============================================

# ansible-playbook regapp.yml    >>>>>>to run the playbook
# LOGIN INTO YOUR DOCKER HUB ACCOUNT
# LOGIN INTO THE ANSIBLE SERVER
# docker login 
# docker images
# docker tag <docker_id> <user_name> /regapp:latest     >>>>>   docker tag dab33f7f933b vj0555/regapp:latest
# docker images
# docker push vj0555/regapp
======================================
ADD BELOW COMMANDS IN THE regapp.yml TO PUSH THE IMAGE TO DOCKER HUB.
START DOCKER ON SERVER. 
==============BELOW IS THE UPDATE/ADDED PART AND OLD ONE AS WELL===============
---
- hosts: ansible 

tasks:
   - name: create a docker image
command: docker build -t regapp:latest .
args:
chdir: /opt/docker

- name: create tag to push image onto dockerhub
command: docker tag regapp:latest vj0555/regapp:latest

- name: puch docker image
command: docker push vj0555/regapp:latest
-----------------------------------
CONFIGURE Copy_Artifacts_onto_Ansible IN JENKINS.
GO TO THIS JOB >> CONFIGURE>>>SCROLL DOWN AND LOOK FOR EXEC COMMAND OPTION AND ADD BELOW COMMAND:
#ansible-playbook /opt/docker/regapp.yml

SELECT: BUILD TRIGGER  >> POLL SCM  >>>  5 STARS *****
WILL BUILD EVERY MINUTE BY APPLYING THIS.

NOW GO ON YOUR REPO AND DO SOME CHANGES IN THE CODE. 
BY DOING THIS IT WILL AUTOMATICALLY TRIGGER JENKINS JOB>>WHICH WILL AUTOMATICALLY CREATE A LATEST DOCKER IMAGE ONTO ANSIBLE SERVER. 

GO TO ANSIBLE SERVER: TO CHECK WEATHER LATEST IMAGE IS CREATED OR NOT.
#su - ansadmin
# cd/opt/dcker
# ll
# docker images

===================
NOW WE WILL BE CREATING ANOTHER ANSIBLE PLAYBOOK WHICH WILL PULL AN IMAGE FROM THE HUB AND CREATE A CONTAINER ON THE DOCKER HOST.

GO TO YOUR ANSIBLE SERVER
#su - ansadmin
# cd /opt/docker
# vi deploy_regapp.yml
===========ADD THIS CONTENT IN THE YML FILE===============
---
- hosts: dockerhost

tasks:
  - name: create container
    command: docker run -d --name regapp_server -p 8082:8080 vj0555/regapp:latest

==============================================================

NOW GO TO YOUR DOCKER INSTANCE AND REMOVE ALL THE IMAGES AND STOPPED CONTAINERS AS WELL: 
# docker rm -f <container_id>
# docker rmi <image_id>
#docker images
#docker ps -a 
NOTHING SHOULD SHOW UP HERE AS WE HAVE DELETED EVERYTHING. 
===============================================================================
NOW GO TO YOUR ANSIBLE SERVER AND TRY TO RUN THE YML FILE.
IT WILL THROW AN ERROR IN WHICH WE HAVE TO GET THE PATH: /var/run/docker.sock
THEN COPY THAT PATH AND ON DOCKER SERVER GIVE PERMISSION TO THIS LOCATION AS BELOW: 
# chmod 777 /var/run/docker.sock
=================

NOW GO TO YOUR ANSIBLE SERVER AND TRY TO RUN THE YML FILE WHICH WE CREATED AGAIN .
++++++++

AGAIN GO TO YOUR DOCKER SERVER AND CHECK FOR THE NEW IMAGES: 
#docker images
#docker ps -a 

YOU WILL SEE A IMAGE AND A CONTAINER ON THE DOCKER SERVER. 

>>TAKE THE PUBLIC IP OF THE DOCKER SERVER AND 3.110.161.83:8082/webapp   CHECK IT ON THE 8082 PORT
>>YOUR APP WILL BE DEPLOYED SUCCESSFULLY. 
>>WE HAVE DONE THIS MANUALLY, NOW WE WILL AUTOMATE THIS TASKS AS WELL WITH THE HELP OF JENKINS.
------------------------------------------
NOW, IF WE RUN SAME PLAYBOOK AGAIN THEN IT WON'T WORK AS THE NAME IS ALREADY TAKEN BY THE CONTAINER WHICH WE CREATED EARLIER. 
SO, NOW WE WILL EDIT deploy_regapp.yml FILE SO THAT IT WILL DELETE EXISTING CONTAINER AND THE IMAGE. AND IT WILL CREATE NEW IMAGE AND THE CONTAINER.
+++++++
---
- hosts: dockerhost

tasks:
  - name: create container
    command: docker run -d --name regapp_server -p 8082:8080 vj0555/regapp:latest
++++++++
---
- hosts: dockerhost

tasks:
- name:stopexistingcontainer
    command:dockerstopregapp-server

  - name: removethecontainer
    command:dockerrmregapp-server

  - name: remove the image 
    command:vj0555/regapp:latest

  - name: create container
    command: docker run -d --name regapp_server -p 8082:8080 vj0555/regapp:latest

============================================================================
 # ansible-playbook deploy_regapp.yml --check
 #  ansible-playbook deploy_regapp.yml 

ABOVE COMMAND WILL THROW ERROR, NOW MAKE CHANGES TO YOUR YML FILE AND THEN RUN THESE COMMANDS. 

+++++++++++++
---
- hosts: dockerhost

tasks:
  - name: stop existing container
    command: docker rm -f regapp_server
    ignore_errors: yes

  - name: remove the container
    command: docker rm regapp-server
    ignore_errors: yes

  - name: remove the image
    command: docker rmi vj0555/regapp:latest
    ignore_errors: yes

  - name: create container
    command: docker run -d --name regapp_server -p 8082:8080 vj0555/regapp:latest

=============

GO TO YOUR DOCKER HOST AND THEN CHECK WEATHER A NEW CONTAINER IS CREATED OR NOT: 
# docker ps -a 

=============
YOU CAN USE THIS WEBSITE TO USE THE ALREADY CREATED ANSIBLE YML FILES CONTENT: 
docker_image ansible.com
===============
NOW WE WILL DO THE ABOVE STEPS USING AUTOMATION USING JENKINS: 

GO TO JENKINS 
SELECT THE JOB AND GO TO >>>CONFIGURE 
EXEC COMMAND: (EDIT)
ansible-playbook /opt/docker/regapp.yml;
sleep 10;
ansible-playbook /opt/docker/deploy_regapp.yml

===================================KUBERNETES============25/03/2022===================================

NOW OUR APP IS DEPLOYED ON A CONTAINER. BUT WHAT IF THAT CONTAINER FAILS??? 
kubernetes ceates a new pod in case of any failures.
SO, FOR THAT WE ARE USING KUBERNETES FOR THE DEPLOYMENT OF OUR WEBSITE.
Elastic Kubernetes Service (Amazon EKS)

PRE-REQUSITE: 
We will be creating one ec2 instance to setup our kubernetes on amazon eks with the help of AWS CLI
1. AN EC2 INSTANCE.
2. INSTALL AWSCLI LATEST VERSION.

ADD CLUSTER>>CREATE
Name: 
- Not editable after creation.

Prerequisites

Before starting this tutorial, you must install and configure the following tools and resources that you need to create and manage an Amazon EKS cluster.

    AWS CLI – A command line tool for working with AWS services, including Amazon EKS. This guide requires that you use version 2.4.9 or later or 1.22.30 or later. For more information, see Installing, updating, and uninstalling the AWS CLI in the AWS Command Line Interface User Guide. After installing the AWS CLI, we recommend that you also configure it. For more information, see Quick configuration with aws configure in the AWS Command Line Interface User Guide.

    kubectl – A command line tool for working with Kubernetes clusters. This guide requires that you use version 1.21 or later. For more information, see Installing kubectl.

    Required IAM permissions – The IAM security principal that you're using must have permissions to work with Amazon EKS IAM roles and service linked roles, AWS CloudFormation, and a VPC and related resources. For more information, see Actions, resources, and condition keys for Amazon Elastic Kubernetes Service and Using service-linked roles in the IAM User Guide. You must complete all steps in this guide as the same user.


AWS DOC LINK: https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

LAUNCH A NORMAL EC2 INSTANCE
PORT: 22 AND 80 REQ


# aws --version
We have to update our aws cli version to work with kube cluster. Follow the steps to update it:

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
Installing past releases of the AWS CLI version 2:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html
https://aws.amazon.com/premiumsupport/knowledge-center/eks-cluster-creation-errors/

Amazon EKS troubleshooting: 
https://docs.aws.amazon.com/eks/latest/userguide/troubleshooting.html#unauthorized
BLOG ON EKS: https://cloudgeometry.io/blog/amazon-eks/
---------------------------------------------------------------------------------------------------------------------
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

"You can now run: /usr/local/bin/aws --version "
 # /usr/local/bin/aws --version
# aws --version
=================
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
/usr/local/bin/aws --version
ls -l /usr/local/bin/aws
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
=======INSTALLING KUBECTL==========
#curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
# ll
# chmod +x kubectl   (https://linuxtect.com/what-is-chmod-x-command-in-linux/)
# mv kubectl /usr/local/bin
# echo $PATH  (Whenever we execute any command it will go and validate here as well)
# kubectl version

INSTALLING: eksctl

# curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
# cd /tmp
# ll
# mv eksctl /usr/local/bin
# eksctl version
===================NOW CREATING AN IAM ROLE =========
>> GO TO IAM CONSOLE.
>> CREATE ROLE
>> IAM FULL ACCESS, EC2 FULL ACCESS, CLOUDFORMATION FULL ACCESS.
>> CREATE ROLE
>> ROLE NAME :  EKS_CTL_ROLE
>>  GO TO EC2 INSTANCE AND ASSIGN THE ROLE CREATED TO THIS INSTANCE.
 ----->>GO TO ACTIONS>>SECUTY>>EDIT ROLE>>SELECT THE ROLE FROM THE DROPDOWN MENU

======================================================================================================

NOW GO TO GIT HUB REPO : kubernetes_setup_using_eksctl.md
>>>Create your cluster and nodes.
>>>Go to cd tmp and run below commands:

======================================================================================================

#cd /tmp
# eksctl create cluster --name devcluster --region us-east-1 --node-type t2.small 

======================================================================================================

DELETE THE CLUSTER: 
FOR DELETING IT FIRST WE NEED TO DELETE: 
VPC / SUBNETS ETC IF DOING IT FROM THE CONSOLE.
eksctl delete cluster --name devcluster --region us-east-1 --node-type t2.small 

GO TO CLOUDFORMATION TO CHECK IF SOMETHING IS BEING CREATED OR NOT....... THIS PROCESS WILL TAKE ALMOST 20-25 MINUTES TO BE COMPLETED.

AFTER CREATING THE CLUSTER WE NEED TO CHECK IF THE NODES ARE CREATED OR NOT.
2:27 mins lec 46
# kubectl get nodes
# kubectl create deployment  demo-nginx --image=nginx --replicas=2 --port=80
>deployment.apps/demo-nginx created

TO CHECK IF THE DEPLOYMENT IS CREATED OR NOT: 
# kubectl get deployment

TO CHECK IF REPLICA SET IS CREATED OR NOT
# kubectl get replicaset
# kubectl get pods

TO DELETE THE PODS:
# kubectl delete pod <pod_name>

THIS COMMAND IS USED TO SEE EVERYTHING: 
# kubectl get all

NOW NEED TO EXPOSE THE DEPLOYMENT AS A SERVICE:
#kubectl expose deployment demo-nginx --port=80 --type=LoadBalancer
>service/demo-nginx exposed
# kubectl get all

TO GET THE EXTERNAL IP OF LOAD BALANCER SERVICE:

a415e5629ec124dafafc675479515b40-1857107097.us-east-1.elb.amazonaws.com
o/p: Welcome to nginx!
ALSO CHECK IN THE LOAD BALANCERS TO SEE WETHER IT IS CREATED OR NOT.
NOW WE HAVE DONE EVERYTHING MANUALLY. WE WILL RUN IT AUTOMATICALLY BY CREATING A MANIFEST FILE JUST LIKE ANSIBLE PLAYBOOKS.

NOW DELETE THE RESOURCES THAT WE HAVE CREATED MANUALLY.
#kubectl delete deployment demo-nginx 
# kubectl delete service/demo-nginx
# kubectl get pods
TO CHECK
>>LOAD BALANCER DELETED.
# vi pod.yml
#
https://kubernetes.io/docs/reference/
select: One-page API Reference for Kubernetes v1.23
select Pod v1 core from the left hand side menu
refer the example
GOOGLE: 
"pod manifest file kubernetes"
select create link on google page..
look for : Create a static pod 

Now create vi pod.yml file and below content in yml file.
CREATE FIRST MANIFEST FILE.
====================================  vi pod.yml  ===============================
kind: Pod
metadata:
  name: demo-pod
  labels:
 app:demo-app

spec:
containers:
- name: web
image: nginx
ports:
 - name: web
 containerPort: 80
================================================================================================
NOW WE WILL BE CREATING SERVICE USING MANIFEST
GOOGLE IT: service manifest file kubernetes
SELECT: Service | Kubernetes
Defining a Service : 

------------------------------
apiVersion: v1
kind: Service
metadata:
  name: demo-service
    
spec:
 ports:
 - name: nginx-port     
   port: 80
   targetPort: 80
     
 type: LoadBalancer  

-------------------------------

NOW GO TO POD.YML AND COMMENT 2 LINES AS SHOWN BELOW: 
------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
#  labels:
#   app: demo-app


spec:
  containers:
    - name: web
image: nginx
ports:
- name: web
containerPort: 80

-------------------------------------------------
TO RUN POD.YML  USE BELOW COMMAND IT WILL CREATE YOUR POD

# kubectl apply -f pod.yml
#kubectl get pods
#kubectl apply -f service.yml
>>>service/demo-service created
# kubectl get all
------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------
>O/P:
NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP                                                              PORT(S)        AGE
service/demo-service   LoadBalancer   10.100.163.5   a5630d4fa22d34a88a301409238aecaf-967050777.us-east-1.elb.amazonaws.com   80:32588/TCP   25s
service/kubernetes     ClusterIP      10.100.0.1     <none>                                                                   443/TCP        122m
-----------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------
USING LABELS AND SELECTOR: LECTURE 49

IF WE CHECK OUR WEBSITE ON BROWSER WITH THE HELP OF ALB ENDPOINT WE WILL NOT BE ABLE TO SEE OUR APP BECAUSE WE HAVE COMMENTED THE LABELS. 
IN REAL SCENARIO THERE CAN BE SEVERAL PODS. HOW WILL SERVICE DECIDE WHICH POD IT HAS TO SEND THE TRAFFIC. SO FOR SENDING TRAFFIC TO A PARTICULAR POD WE NEED TO GIVE THEM LABELS.

POD CREATED AND CREATED SERVICE IN LAST STEPS.

NOW WE WILL HAVE TO UPDATE THE POD.YML FILE.
UN-COMMENT THE 2 LINES WE HAVE COMMENTED IN THE EARLIER STEPS. (LABELS)

now vi service.yml
==========
apiVersion: v1
kind: Service
metadata:
name: demo-service

spec:
  ports:
  - name: nginx-port
    port: 80
    targetPort: 80

  selector:                   {changed part}
app: demo-app
  type: LoadBalancer     

=========
# kubectl apply -f pod.yml
To update the POD.YML FILE
>>pod/demo-pod configured
# kubectl get all
# kubectl apply -f service.yml
>> To update the service.yml file 
>> service/demo-service configured

{TO KNOW THE IP OF A POD RUN BELOW COMMAND}:
# kubectl get pod -o wide   
TO KNOW ALL THE INFO ABOUT THE SERVICE WE HAVE CREATED RUN BELOW COMMAND: 
# kubectl describe service/demo-service
 RUN THIS ON THE BROWSER: 
a5630d4fa22d34a88a301409238aecaf-967050777.us-east-1.elb.amazonaws.com
>>THE APP SHOULD BE VISIBLE ON THE PAGE NOW: 

{
Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
}

LECTURE 50:  WRITE A DEPLOYMENT FILE
NOW WE ARE DELETING OUR PODS and services BY RUNNING BELOW COMMAND: 
# kubectl delete pod demo-pod 
O/P: pod "demo-pod" deleted
# kubectl delete service/demo-service
# vi regapp-deployment.yml
-----------------------------------------------------------------------------------------------------
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: regapp
  name: vj0555-regapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: regapp 
strategy:
rollingUpdate:
maxSurge: 1
maxUnavailable: 1
type: RollingUpdate
  template: 
metadata: 
      labels:
app: regapp
    spec:
      containers:
        -
          image: vj0555/regapp
          imagePullPolicy: Always
          name: regapp
          ports: 
            -
containerPort: 8080
------------------CHECK VIDEO FOR THE ALIGNMENT OF THE ABOVE YML FILE--------------------------------------------------------
WE NEED A SERVICE FILE AS WELL NOW. 
-------------------------------------------

VALIDATE YOUR YML FILE: 
https://onlineyamltools.com/validate-yaml

http://www.yamllint.com/
-----------------------------------------
apiVersion: apps/v1
kind: Service

metadata: 
name: vj0555-service
labels: 
app: regapp
spec: 
selector: 
app: regapp

ports: 
 port: 8080
targetPort: 8080
type: LoadBalancer

----------------LECTURE 51-------------------------
# go to cd/tmp
# ls
# kubectl apply -f regapp-deployment.yml
# kubectl apply -f regapp-service.yml

------===============================
BELOW YML FILE WAS NOT RUNNING SO CHANGED THE VERSION:
https://stackoverflow.com/questions/62108860/kubectl-no-matches-for-kind-service-in-version-apps-v1
--------===============================-------
apiVersion: v1   
kind: Service

metadata:
  name: vj0555-service
  labels:
    app: regapp
spec:
 selector:
   app: regapp

 ports:
  - port: 8080
    targetPort: 8080

 type: LoadBalancer
-----------------------
# kubectl get all
----------------------------
NOW ACCESS THE ENDPOINT ON THE BROWSER ON PORT 8080. YOU'LL BE ABLE TO SEE THE TOMCAT DEFAULT PAGE
http://a8cff168144bd4fd2967386c2b4595cf-310760245.us-east-1.elb.amazonaws.com:8080
--------------------------
# kubectl get pod
#YOU WILL SEE 3 PODS ARE RUNNING AS WE MENTIONED IN THE DEPLOYMENT FILE. NOW WE WILL BE DELETING ONE PPOD MANUALLY. 
# kubectl delete pod <pod_name>
AND NOW IF WE CHECK USING BELOW COMMAND
# kubectl get pods
#WE WILL SEE THt there are still three pods running. because after deleting one pod. another pod is created automatically as we mentioned in the file.

LECT: 52
NOW WE WILL INTEGRATE THIS WITH JENKINS AND ANSIBLE.

HOW TO INEGRATE KUBE CLUSTER WITH ANSIBLE:
>>CREATE AND ADMINUSER
>>ADD ANSADMIN USER TO SUDOERS FILE
>>ENABLE PASROD BASED LOGIN
ON ANSIBLE NODE:
> ADD TO HOSTS FILE
> COPY SSH KEYS
> TEST THE CONNECTION
ONCE THIS IS DONE WE WILL BE WRITING THE ANSIBLE PLAYBOOK .
NOW, GO TO THE BOOTSTRAP SERVER AND ADD THE ANSADMIN USER
# adduser ansadmin
# passwd ansadmin      << ansible is the password>
# visudo
EDIT THIS SECTION IN THAT FILE:  AND :WQ
## Allow root to run any commands anywhere 
root    ALL=(ALL)ALL
ansadminALL=(ALL)      ALL
# vi /etc/ssh/sshd_config
## To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication no
#PermitEmptyPasswords no
PasswordAuthentication yes
# systemctl restart sshd

NOW GO TO YOUR ANSIBLE SERVER: 
# sudo su - ansadmin
# cd /opt/docker
# ls
# mv regapp.yml create_image_reapp.yml
# ls
You will see the changes here. NOW RENAME ANOTHER FILE.
# mv deploy_regapp.yml docker_deployment.yml
# ls
NOW GOT TO THE ANSIBLE SERVER AND MAKE SURE YOU ARE LOGGED IN AS A ROOT USER NOT ANS ADMIN USER. 
WE WILL NOW EDIT THE FILE.
# vi /etc/ansible/hosts
NOW ADD THE KUBERNETES GROUP, WITH IT'S PRIVATE IP: 
[kubenetes]
172.31.34.193
:wq

ANSIBLE SERVER: 
# ssh-copy-id 172.31.34.193   (IP ADDRESS OF EKS BOOTSTRAP SERVER. PRIVATE IP)
NOW TRY TO LOGIN INTO KUBERNETES SERVER VIA ANSIBLE SERVER: 
# ssh-copy-id 172.31.34.193    <private ip of the kubernetes server)
NOW TO CHECK THE CONNECTION WE NEED TO RUN THE BELOW COMMAND: 
#  ansible all -a uptime   
O/P: (THREE IP'S SHOULD SHOW UP HERE)
ONLY 2 IPS WERE SHOWING UP EARLIER BECAUSE OUR DOCKER SERVER WAS STOPPED. ONCE WE STARTED THE SERVER ALL THREE SHOWED UP.
#
#
LEC:53
EXECUTE DEP AND SERVICE FILES USING ANSIBLE. 
LOGIN TO ANSIBLE SERVER. 
LOGIN VIA ANSADMIN.
# cd /opt/docker
# vi kube_deploy.yml
---------
---
- hosts:kubernetes
  user: root 
    
  tasks:
  - name: deployregapponkubernetes
    command: kubectlapply -f regapp-deployment.yml
~                                                              
-----------
# vi kube_service.yml
-------
---
- hosts:kubernetes
  user: root
    
  tasks:
  - name: deployregapponkubernetes
    command: kubectlapply -f regapp-service.yml
~                                                        
=====================================================================================
NOW DELETE THE RESOURCES CREATED PREVIOUSLY ON EKS_BOOTSTRAP_SERVER
#cd /tmp
# kubectl delete -f regapp-service.yml && kubectl delete -f regapp-deployment.yml
# kubectl get all   <to check the deletion process>

On EKS_Server 
move all the .yml files from /tmp to root with the help of following commands
#mv <.yml> ~

Now, Run the playbooks to build resources 
#ansible-playbook -i /opt/docker/hosts kube_deploy.yml
>> it will create all the pods and deployment on eks server
# ansible-playbook -i /opt/docker/hosts kube_service.yml
>>it will create service on eks server 
=====================================================================================

NOW WE ARE CREATING JENKINS DEPLOYMENT JOB FOR KUBERNETES: 

DEPLOYING AS APOD. 
USING JENKINS NOW RATHER THAN USING ANSIBLE FOR RUNNING THE PLAYBOOKS. 

JUMP IN TO YOUR JENKINS SERVER. 

CREATE NEW
NAME: Deploy_On_kubernetes
Select free style job.
Got to Post build action.
select : send files or execute commands over SSH
NAME: ANSIBLE SERVER
EXEC COMMANDS: 
cd /opt/docker;
ansible-playbook kube_deploy.yml
ansible-playbook -i /opt/docker/hosts kube_deploy.yml;
ansible-playbook -i /opt/docker/hosts kube_service.yml
APPLY AND SAVE

GOT TO KUBE SERVER
kubectl delete deployment.apps/vj0555-regapp
TO DELETE THE RESOURCES.

NOW DELETE THE SERVICE: 
kubectl delete service/vj0555-service

=====================================================================================

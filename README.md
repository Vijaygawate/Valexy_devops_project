# Valexy_devops_project
Step 1: Create one Linux instance.
Step 2: In Linux instance install Jenkins and GIT.
Step 3: setup maven
Step 4: Install plugins: git, maven on jenkins
Step 5: install maven:
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



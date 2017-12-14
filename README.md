# Configure Tomcat on EC2

This document describes the steps to install, configure and run Tomcat server on an AWS EC2 instance. This involves:

* Step 1: Install Java (JDK) 8 on the AWS EC2 instance
* Step 2: Install Tomcat 8 on the AWS EC2 instance
* Step 3: Configure Tomcat to start server upon instance boot

# Install Java (JDK) 8 on AWS EC2

wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u71-b15/jdk-8u71-linux-x64.rpm

sudo rpm -i jdk-8u71-linux-x64.rpm

# Install Tomcat 8 on AWS EC2

wget http://ftp.cixug.es/apache/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.tar.gz

tar zxpvf apache-tomcat-8.0.32.tar.gz

# Configure Tomcat to start server upon instance boot

sudo nano /etc/rc.d/init.d/tomcat

!/bin/sh
# Tomcat init script for Linux.
#
# chkconfig: 2345 96 14
# description: The Apache Tomcat servlet/JSP container.
JAVA_HOME=/usr/java/jdk1.8.0_71/
CATALINA_HOME=/home/ec2-user/apache-tomcat-8.0.30
export JAVA_HOME CATALINA_HOME
exec $CATALINA_HOME/bin/catalina.sh $*

sudo chmod 755 /etc/rc.d/init.d/tomcat
sudo chkconfig --level 2345 tomcat on

sudo nano ./apache-tomcat-8.0.32/conf/tomcat-users.xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<role rolename="admin-gui"/>
<user username="tomcat" password="tomcatadmin" roles="manager-gui,manager-status,admin-gui"/>
<user username="tomcattools" password="toolsadmin" roles="manager-jmx,manager-script"/>

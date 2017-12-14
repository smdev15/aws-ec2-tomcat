# Configure Tomcat on EC2

This document describes the steps to install, configure and run Tomcat server on an AWS EC2 instance. This involves:

* Step 1: Install Java (JDK) 8 on the AWS EC2 instance
* Step 2: Install Tomcat 8 on the AWS EC2 instance
* Step 3: Configure Tomcat to automatically start server on system restart

Once setup is complete, reboot EC2 instance to start Tomcat automatically.

# Install Java (JDK) 8 on AWS EC2

wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u71-b15/jdk-8u71-linux-x64.rpm

sudo rpm -i jdk-8u71-linux-x64.rpm

# Install Tomcat 8 on AWS EC2

Download Tomcat:
```
wget http://ftp.cixug.es/apache/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.tar.gz
```

Install Tomcat:
```
tar zxpvf apache-tomcat-8.0.32.tar.gz
```

# Configure Tomcat to automatically start server on system restart

### Create a Tomcat init script 

The init script is executed to start Tomcat server when the AWS EC2 instance is started.

Open the following file:

```
sudo nano /etc/rc.d/init.d/tomcat
```

Use the following code for the init script:

```
!/bin/sh
# Tomcat init script for Linux.
#
# chkconfig: 2345 96 14
# description: The Apache Tomcat servlet/JSP container.
JAVA_HOME=/usr/java/jdk1.8.0_71/
CATALINA_HOME=/home/ec2-user/apache-tomcat-8.0.30
export JAVA_HOME CATALINA_HOME
exec $CATALINA_HOME/bin/catalina.sh $*
```

Need to change permissions on /etc/rc.d/init.d/tomcat to allow everyone to read and execute the file (but only the file owner should be allowed to write to the file as well). Execute the following command:
```
sudo chmod 755 /etc/rc.d/init.d/tomcat
```

### Configure automatic startup on Linux

Use the chkconfig command, as the root user, to make the tomcat service start in the run level(s) of your choice:
```
sudo chkconfig --level 2345 tomcat on
```
Note: By default Linux boots either to runlevel 3 or to runlevel 5. The former permits the system to run all services except for a GUI. The latter allows all services including a GUI. 0 represents System halt (no activity) and 6 represents Reboot. In addition to the standard runlevels, users can modify the preset runlevels or even create new ones if desired. Runlevels 2 and 4 are usually used for user defined runlevels.

### Create users for Tomcat server management

Open the tomcat-users.xml file:
```
sudo nano ./apache-tomcat-8.0.32/conf/tomcat-users.xml
```

Create roles and users as following:
```
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<role rolename="admin-gui"/>
<user username="tomcat" password="tomcatadmin" roles="manager-gui,manager-status,admin-gui"/>
<user username="tomcattools" password="toolsadmin" roles="manager-jmx,manager-script"/>
```

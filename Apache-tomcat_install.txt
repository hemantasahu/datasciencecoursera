apache tomcat installation in rhel-7
------------------------------------
IMP** : Note Tomcat 9 need jdk 8
Tomcat 8 can run on jdk 7 same methods can be used for rhel6 tomcat 8

[root@rhel7-base bin]# rpm -qa|grep openjdk
java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64
java-1.8.0-openjdk-headless-1.8.0.65-3.b17.el7.x86_64
java-1.8.0-openjdk-devel-1.8.0.65-3.b17.el7.x86_64
[root@rhel7-base bin]# tail -4 /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64
export JRE_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64/jre
export CATALINA_HOME=/opt/apache-tomcat-9.0.0.M13
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
[root@rhel7-base bin]# alternatives --config java

There is 1 program that provides 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64/jre/bin/java


[root@rhel7-base bin]# . /etc/profile

if you want to add new java environment

[root@rhel7-base bin]# alternatives --install /usr/bin/java java  /opt/jdk1.8.0_65/bin/java 2
then again run alternatives --config java.

[root@rhel7-base opt]# tar xvzf apache-tomcat-9.0.0.M13.tar.gz
[root@rhel7-base bin]# mv ~/Downloads/commons-daemon-1.0.15-native-src.tar.gz /opt/apache-tomcat-9.0.0.M13/bin/
[root@rhel7-base bin]# cd /opt/apache-tomcat-9.0.0.M13/bin/
[root@rhel7-base bin]# tar xvzf commons-daemon-1.0.15-native-src.tar.gz

[root@rhel7-base unix]# pwd
/opt/apache-tomcat-9.0.0.M13/bin/commons-daemon-1.0.15-native-src/unix
[root@rhel7-base unix]# ./configure 
[root@rhel7-base unix]# pwd
/opt/apache-tomcat-9.0.0.M13/bin/commons-daemon-1.0.15-native-src/unix
[root@rhel7-base unix]# ./configure 
gcc   jsvc-unix.o libservice.a -ldl -lpthread -o ../jsvc
make[1]: Leaving directory `/opt/apache-tomcat-9.0.0.M13/bin/commons-daemon-1.0.15-native-src/unix/native'
[root@rhel7-base unix]# ls -l jsvc 
-rwxr-xr-x 1 root root 170635 Nov 20 07:58 jsvc
[root@rhel7-base unix]# 
[root@rhel7-base unix]# cp jsvc ../..
[root@rhel7-base unix]# cd ../..
[root@rhel7-base bin]# pwd
/opt/apache-tomcat-9.0.0.M13/bin
[root@rhel7-base bin]#


***openjdk did not work so I had to download and install oracle jdk**
refer
-----
http://tomcat.apache.org/tomcat-9.0-doc/RUNNING.txt

[root@rhel7-base opt]# alternatives --config java

There is 1 program that provides 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64/jre/bin/java

Enter to keep the current selection[+], or type selection number: ^C
[root@rhel7-base opt]# alternatives --install /usr/bin/java java  /opt/jdk1.8.0_111/bin/java 2
[root@rhel7-base opt]# alternatives --config java

There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64/jre/bin/java
   2           /opt/jdk1.8.0_111/bin/java

Enter to keep the current selection[+], or type selection number: 2
[root@rhel7-base opt]# which java
/usr/bin/java
[root@rhel7-base opt]# java -version
java version "1.8.0_111"
Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)
[root@rhel7-base opt]# alternatives --config java

There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*  1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64/jre/bin/java
 + 2           /opt/jdk1.8.0_111/bin/java

Enter to keep the current selection[+], or type selection number: 
[root@rhel7-base opt]#


[root@rhel7-base opt]# tail -n 5 /etc/profile

export JAVA_HOME=/opt/jdk1.8.0_111
export JRE_HOME=/opt/jdk1.8.0_111/jre
export CATALINA_HOME=/opt/apache-tomcat-9.0.0.M13
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
[root@rhel7-base opt]# . /etc/profile
[root@rhel7-base opt]# echo $PATH
/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/opt/puppetlabs/bin:/root/bin:/usr/local/git/bin:/opt/jdk1.8.0_111/bin:/opt/jdk1.8.0_111/jre/bin
[root@rhel7-base opt]#

[root@rhel7-base opt]# ps -ef|grep apache
root     11040  6288  0 08:34 pts/1    00:00:00 grep --color=auto apache
[root@rhel7-base opt]# cd $CATALINA_HOME/bin
[root@rhel7-base bin]# cd commons-daemon-1.0.x-native-src/unix
bash: cd: commons-daemon-1.0.x-native-src/unix: No such file or directory
[root@rhel7-base bin]# cd commons-daemon-1.0.15-native-src/unix/
[root@rhel7-base unix]# ./configure 
[root@rhel7-base unix]# make
(cd native; make  all)
make[1]: Entering directory `/opt/apache-tomcat-9.0.0.M13/bin/commons-daemon-1.0.15-native-src/unix/native'
gcc   jsvc-unix.o libservice.a -ldl -lpthread -o ../jsvc
make[1]: Leaving directory `/opt/apache-tomcat-9.0.0.M13/bin/commons-daemon-1.0.15-native-src/unix/native'
[root@rhel7-base unix]# cp jsvc ../..
cp: overwrite ‘../../jsvc’? yes
[root@rhel7-base unix]# cd ../..
[root@rhel7-base bin]# CATALINA_BASE=$CATALINA_HOME
[root@rhel7-base bin]# $CATALINA_HOME/bin/catalina.sh start

[root@rhel7-base conf]# vi $CATALINA_HOME/conf/server.xml

  <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL/TLS HTTP/1.1 Connector on port 9080
    -->
    <Connector port="9080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="9080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

[root@rhel7-base conf]# vi $CATALINA_HOME/conf/server.xml
[root@rhel7-base conf]# $CATALINA_HOME/bin/catalina.sh start
[root@rhel7-base conf]# netstat -ano |grep 9080
tcp6       0      0 :::9080                 :::*                    LISTEN      off (0.00/0/0)
[root@rhel7-base conf]# 


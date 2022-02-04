# prepare start/stop services, check logs all(environments) #
___
## create vagrant vm's (env)
* Prerequisites: physical server with ubuntu 18+
* vagrant VMs can not be created on cloud VMs like EC2 instances

  > __Preparing Vagrant__
     
     * Clone the repository
     * If it is already installed skip following vagrant installation
     * install vagrant and virtual box
    
  > __setup VM using vagrant file__
  * Make sure you are initiating VM setup at desired directory,
  * copy vagrantfile template.
  * modify template as per desired configuration.
  * create VM with vagrant

  > __Accesing vagrant VM's__
  * Connect to the host server using server credentials consider it as jump server.
  * make sure required VM is up and running, verify status.
  * If vagrant VM is not running, run VM with   __vagrant up__
  * Login to vagrant VM with __vagrant ssh__
  
  ```
      vagrant ssh <ip/hostname>
 
  ```

## start/stop vagrant VM 
* cd [to-the-vagrantfile-location] ; __vagrant up__  - to start the vagrant VM
* cd [to-the-vagrantfile] ; __vagrant stop__ - to stop the vagrant VM
* cd [to-the-vagrantfile] ; __vagrant status__ - to check the status.
  
# check logs in all env (QA,UAT,LT,E2E)

  ### Install tomcat (by following commands)
  
  ```
   sudo apt update -y
   sudo apt install default-jdk -y     #install java
   java --version   # check java version 
   sudo update-java-alternatives -l  
   # update the java Currently /usr/lib/jvm/java-1.11.0-openjdk-amd64
  ```

* [then install tomcat](https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz) (using this link)
  
  ### setup tomcat as a service
  ```
   cd /etc/systemd/system
   sudo touch tomcat.service
   sudo nano tomcat.service 
   # copy the code between the lines into tomcat.service file 
  -------------------------------------------------------- 
   [Unit]
   Description=Apache Tomcat Web Application Container
   After=network.target
   [Service]
   Type=oneshot
   Environment=JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64        #make sure this is correct
   Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
   Environment=CATALINA_HOME=/opt/tomcat
   Environment=CATALINA_BASE=/opt/tomcat
   Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
   Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
   ExecStart=/opt/tomcat/bin/startup.sh
   ExecStop=/opt/tomcat/bin/shutdown.sh
   User=root
   Group=root
   UMask=0007
   RestartSec=10
   RemainAfterExit=yes
   [Install]
   WantedBy=multi-user.target
   ``` 

* after updating the tomcat.service file with above content save and close with ctrl+o , Enter, ctrl+x   
* make sure the java path is updated properly.
```
 sudo systemctl daemon-reload  
 sudo systemctl enable tomcat  
 ```

>## check the log files of tomcat

```
cd /opt/tomcat  #tomcat root diretory
cd logs         #tomcat logs directory
ls -ltr         # list all files in logs directory 
cat catalina.out   # this is main tomcat file
```
 ### Do The Same Process For Apache Server 
 ```
 install apache server :

 sudo apt update -y   # update the repositories
 sudo apt install apache2 -y   #install apache httpd serve
 apache2 -v            # check apache version
 ```
 >## check the log files for apache server 
 * apache and nginx has similar log files. __access.log__ and __error.log__
  
  ```
  cd /var/log/apache2 # go to the path 
  ls -ltr  # list all the log files related to apache2
  cat access.log      # check the access.log
  cat error.log       # check error log to troubleshoot errors
  ```

  * ## NOTE : this is for the process to check env logs 
  * ### repeat the same process with proper credentails of related env.   
















 


 
  
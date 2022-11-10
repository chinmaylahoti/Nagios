Nagios Configuration with
AWS


•	Amazon Linux 2 Kernel 5.10 AMI EC2
•	Provide network group for SSH, HTTP, HTTPSl, TCP
•	Create 2 instances as below
•	Master
•	Slave


•	Download Putty
•	Copy Public IP of master
•	Connect with authentication


•	Username: ec2-user 

Nagios requires the following packages are installed on your server prior to installing nagios:
•	Apache
•	PHP
•	GCC compiler collection (glibc GNU C library, glibc-common: common libraries)
•	GD development libraries (Graphic libraries)
You can use yum to install these packages by running the following commands(as ec2-user):
sudo yum install httpd php
sudo yum install gcc glibc glibc-comman
sudo yum install gd gd-devel


You need to set up a Nagios user. Run the following commands:
sudo adduser -m nagios			//creates the user’s home directory -m
sudo passwd nagios
//Type the new password twice.
sudo groupadd nagcmd
sudo usermod -a -G nagcmd nagios		//-a append  and -G group
sudo usermod -a -G nagcmd apache


Create a directory for storing the downloads. 
mkdir ~/downloads
cd ~/downloads

wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.0.8.tar.gz
wget http://nagios-plugins.org/download/nagios-plugins-2.0.3.tar.gz


Extract the Nagios source code tarball.
tar zxvf nagios-4.0.8.tar.gz
cd nagios-4.0.8

Run the configuration script with the name of the group which you have created in the above step.
./configure --with-command-group=nagcmd  


Compile the Nagios source code. 
make all
Install binaries, init script, sample config files and set permissions on the external command directory.
sudo make install
sudo make install-init
sudo make install-config
sudo make install-commandmode 

 
Change E-mail address with the nagiosadmin contact definition you’d like to use for receiving Nagios alerts.
sudo vim /usr/local/nagios/etc/objects/contacts.cfg  


sudo make install-webconf
Create a nagiosadmin account for logging into the Nagios web interface. Note the password you need it while login to nagios web console.
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin     
//Type the new password twice.
sudo service httpd restart       //Restart service  


Extract the Nagios plugins source code tarball. 
cd ~/downloads
tar zxvf nagios-plugins-2.0.3.tar.gz
cd nagios-plugins-2.0.3 


Compile and install the plugins.
./configure --with-nagios-user=nagios --with-nagios-group=nagios
sudo make install 


Add Nagios to the list of system services and have it automatically started when the system boots.
sudo chkconfig –add nagios
sudo chkconfig nagios on 


Verify the sample Nagios configuration files.
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
If there are no errors, start Nagios.
sudo service nagios start 


Access the Nagios web interface to do this you will need to know the Public DNS or IP for your instance, you can get this from the Instance section of the EC2 Console if you do not already know it. You’ll be prompted for the username (nagiosadmin) and password you specified earlier.
(Public ip of master)/nagios 

# Nagios_MonitoringTool
This document describes how to install Nagios Core from source.

This guide is broken up into several sections and covers different operating system (OS) distributions. If your OS Distribution is not included in this guide then please contact us to see if we can get it added. Some distributions may be missing as we don't have access to a test environment that allows us to develop the documentation.

Nagios Core 4.4.14 and Nagios Plugins 2.4.6 are the versions used in this guide, however future versions should also work fine with these steps.

NOTE: As of Nagios Core 4.4.7, openssl is a required prerequisite. This command has been added in the documentation below for the more popular distributions (CentOS, RHEL, Ubuntu, etc...). If you are using other distributions (Solaris, Gentoo, etc...) you will need to run the appropriate command to install openssl in the prerequisite section.

This documentation is broken up into two distinct sections:

Install Nagios Core

Install Nagios Plugins

This separation is to make a clear distinction as to what prerequisite packages are required by the OS it is being installed on. For example the SNMP packages are installed as part of the Nagios Plugins section, as SNMP is not required by Nagios Core.

Security-Enhanced Linux
This guide is based on SELinux being disabled or in permissive mode. Steps to do this are as follows

>  sed -i 's/SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
>  setenforce 0

Perform these steps to install the pre-requisite packages
> yum install -y gcc glibc glibc-common wget unzip httpd php gd gd-devel perl postfix
> 
> yum install openssl-devel
> dnf install -y gcc glibc glibc-common perl httpd php wget gd gd-devel
> dnf install openssl-devel
> dnf update -y

Downloading the Source
> cd /opt
> wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.14.tar.gz
> tar xzf nagioscore.tar.gz

Compile
> cd /opt/nagioscore-nagios-4.4.14/
> ./configure
> make all

Create User And Group
This creates the nagios user and group. The apache user is also added to the nagios group
> make install-groups-users
> usermod -a -G nagios apache

Install Binaries
This step installs the binary files, CGIs, and HTML files.
> make install

Install Service / Daemon
This installs the service or daemon files and also configures them to start on boot. The Apache httpd service is also configured at this point.
> make install-daemoninit
> chkconfig --level 2345 httpd on
> make install-daemoninit
> systemctl enable httpd.service

Install Command Mode
> make install-commandmode

Install Apache Config Files
> make install-webconf

Configure Firewall
You need to allow port 80 inbound traffic on the local firewall so you can reach the Nagios Core web interface.
> iptables -I INPUT -p tcp --destination-port 80 -j ACCEPT
> service iptables save
> ip6tables -I INPUT -p tcp --destination-port 80 -j ACCEPT
> service ip6tables save
> firewall-cmd --zone=public --add-port=80/tcp
> firewall-cmd --zone=public --add-port=80/tcp --permanent

Create nagiosadmin User Account
You'll need to create an Apache user account to be able to log into Nagios.
The following command will create a user account called nagiosadmin and you will be prompted to provide a password for the account.

> htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

Start Apache Web Server
> service httpd start
> systemctl start httpd.service

Start Service / Daemon
> service nagios start
> systemctl start nagios.service

Test Nagios
Nagios is now running, to confirm this you need to log into the Nagios Web Interface.

Point your web browser to the ip address or FQDN of your Nagios Core server, for example:

http://10.25.5.143/nagios

http://core-013.domain.local/nagios

You will be prompted for a username and password. The username is nagiosadmin (you created it in a previous step) and the password is what you provided earlier.

Once you have logged in you are presented with the Nagios interface. Congratulations you have installed Nagios Core.

Installing The Nagios Plugins

Nagios Core needs plugins to operate properly. The following steps will walk you through installing Nagios Plugins.

> yum install -y gcc glibc glibc-common make gettext automake wget openssl-devel net-snmp net-snmp-utils epel-release
> yum install -y perl-Net-SNMP
> cd /opt
> wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.60.tar.gz
> tar xzf autoconf-2.60.tar.gz 
> cd /opt/autoconf-2.60
> ./configure 
> make
> make install

Further conf
> cd /opt
> wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
> rpm -ihv epel-release-latest-7.noarch.rpm
> subscription-manager repos --enable=rhel-7-server-optional-rpms
> yum install -y gcc glibc glibc-common make gettext automake autoconf wget openssl-devel net-snmp net-snmp-utils
> yum install -y perl-Net-SNMP

Downloading The Source
cd /opt
wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.4.6.tar.gz
tar zxf nagios-plugins.tar.gz

Compile + Install
> cd /tmp/nagios-plugins-release-2.4.6/
> ./tools/setup
> ./configure
> make
> make install

Test Plugins
Point your web browser to the ip address or FQDN of your Nagios Core server, for example:

http://10.25.5.143/nagios

http://core-013.domain.local/nagios

Go to a host or service object and "Re-schedule the next check" under the Commands menu. The error you previously saw should now disappear and the correct output will be shown on the screen.


Service / Daemon Commands
Different Linux distributions have different methods of starting / stopping / restarting / status Nagios.

===== RHEL 5/6 | CentOS 5/6 | Oracle Linux 5/6 =====

> service nagios start
> service nagios stop
> service nagios restart
> service nagios status
 

===== RHEL 7/8 | CentOS 7/8 | Oracle Linux 7/8 | CentOS Stream 8=====

> systemctl start nagios.service
> systemctl stop nagios.service
> systemctl restart nagios.service
> systemctl status nagios.service
 
 
 

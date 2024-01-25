# Nagios_MonitoringTool
This document describes how to install Nagios Core from source.

This guide is broken up into several sections and covers different operating system (OS) distributions. If your OS Distribution is not included in this guide then please contact us to see if we can get it added. Some distributions may be missing as we don't have access to a test environment that allows us to develop the documentation.

Nagios Core 4.4.14 and Nagios Plugins 2.4.6 are the versions used in this guide, however future versions should also work fine with these steps.

NOTE: As of Nagios Core 4.4.7, openssl is a required prerequisite. This command has been added in the documentation below for the more popular distributions (CentOS, RHEL, Ubuntu, etc...). If you are using other distributions (Solaris, Gentoo, etc...) you will need to run the appropriate command to install openssl in the prerequisite section.

This documentation is broken up into two distinct sections:

Install Nagios Core

Install Nagios Plugins

This separation is to make a clear distinction as to what prerequisite packages are required by the OS it is being installed on. For example the SNMP packages are installed as part of the Nagios Plugins section, as SNMP is not required by Nagios Core.

> sed -i 's/SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
> 

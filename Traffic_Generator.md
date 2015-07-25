# Install Virtual Box #

This section describes the steps necessary to install virtual box for the creation of the web spider VM’s

**Step 1:** Download the RHEL repository:

> ```
# cd /etc/yum.repos.d
# wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo```

**Step 2:** Install RPMForge Repository:

> ```
# wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.i386.rpm
# wget http://dag.wieers.com/rpm/packages/RPM-GPG-KEY.dag.txt```

> ```
# rpm --import RPM-GPG-KEY.dag.txt```
> ```
# rpm -Uvh rpmforge-release-0.5.2-2.el5.rf.i386.rpm```

**Step 3:** Install Development Tools

Install dkms and the kernel source. You will be unable to start VM’s until the kernel module is installed. If kernel version is PAE then replace “kernel-devel” with “kernel-PAE-devel”.

> ```
# yum install kernel-devel```
> ```
# yum –enablerepo=rpmforge install dkms```
> ```
# yum groupinstall “Development Tools”```

**Step 4:** Install Virtual Box

> ```
# yum install VirtualBox```

Now virtual box has been installed create a VM and install the guest OS.

# Install OpenWebSpider v0.7 #

This section assumes virtual box has been installed and the guest virtual machine has been created.

**Step 1:** Install the prerequisite packages for openwebspider

> ```
# yum install mysql mysql-server mysql-devel httpd```

**Step 2:** Download open web spider version 0.7. Unpack the file and compile with “make”

> ```
# wget http://downloads.sourceforge.net/project/openwebspider/OpenWebSpider/0.7/openwebspider-0.7.zip?r=http%3A%2F%2F```

> ```
# unzip openwebspider-0.7.zip
# cd openwebspider-0.7```
> ```
# make```

**Step 3:**
Enable apache on startup and run Apache web server:

> ```
# chkconfig –levels 235 httpd on
# /etc/init.d/httpd start```

Open a web browser and visit http://127.0.0.1/ to verify that apache webserver is running

**Step 4:** Install PHP5 and restart apache:

> ```
# yum install php
# /etc/init.d/httpd restart```

**Step 5:**

Enable mysql startup on boot and run the server:
> ```
# chkconfig –levels 235 mysqld on
# /etc/init.d/mysqld start```

  * Set root password:

> ```
# mysqladmin –u root password <enter desired root password here>```

  * Login to MySQL

> ```
# mysql –u root –p```

  * List current databases

> ```
mysql> show databases;```

Expected Output:

> ```

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| test               |
+--------------------+
3 rows in set (0.00 sec)```

Exit out of mysql:

> ```
mysql> quit```

  * Create the mysql database for openwebspider:

> ```
# wget http://www.openwebspider.org/public/source-code/ows/v0.7/sql/sql_struct.txt```

**This SQL script creates the databases required to run openwebspider**

  * Execute the mysql script:

> ```
# mysql –u root -p < sql_struct.txt```

  * Create a user account and grant all priveleges to the account for the created databases

  * Log in to mysql:

> ```
# mysql –u root –p```

  * Verify the creation of the se\_hosts and se\_spiderdb databases

> ```
mysql> show databases;```

Expected Output:

> ```

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| se_hosts           |
| se_spiderdb        |
| test               |
+--------------------+
5 rows in set (0.01 sec)```

  * Create user and grant all privileges to the account for se\_hosts and se\_spiderdb

Change **(user)** and **<password string>** to a desired value. This account will be utilized by openwebspider which dumps fetched web content to the created mysql databses.

> ```
mysql> CREATE USER ‘(user)’@’localhost’ INDENTIFIED BY ‘<password string’;```
> ```
mysql> GRANT ALL PRIVILEGES ON se_hosts.* TO ‘(user)’@’localhost’ IDENTIFIED BY ‘<password string>’;```
> ```
mysql> GRANT ALL PRIVILEGES ON se_spiderdb.* TO ‘(user)’@’localhost’ IDENTIFIED BY ‘<password string>’;```

Exit out of mysql:

> ```
mysql> quit```

**Step 6:** Configure OpenWebSpider

  * Edit openwebspider.conf:

Change **(user)** and **<password string>** to the sql user and password created in the previous step.

> ```
#server1 is the server with the DB ‘DB1′
#db1 is the database of the hosts
db1=hosts
mysqlserver1=localhost
userdb1=(user)
passdb1=<password string>
#server2 is the server with the DB ‘DB2′
#db2 is the database of the indexed pages
db2=spiderdb
mysqlserver2=localhost
userdb2=(user)
passdb2=<password string>
#password for the OWS Server
ows_server_password=<password string> # Must set even if OWS is not being setup as a server
#EOF#```


**Step 7:** Test and Run OpenWebSpider:
### Read First! ###

Depending on the capabilities and resources of the flow exporter box, it may be necessary to limit the number of threads and crawl delay of openwebspider. By default, the rate at which web traffic is generated can increase exponentially in only a matter of seconds. Using the defaults, openwebspider allocates 20 threads to the web crawler. Without limiting the crawl rate, this may cause a denial of service condition to the flow exporter box. This can be remedied by using the –t option which allows one to specify the number of threads to allocate to the web crawler. Additionally, -d allows one to modify the crawl delay in milliseconds. In the command below, we allocate one thread to the web crawler and issue a crawling delay of 1000ms.

> ```
# ./openwebspider –t 1 –d 1000 –i http://<desired URL to crawl>```

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>
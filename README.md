Puppet Modules - WSO2 API Manager 2.0.0 distributed setup

This puppet module creates a single node setup as follows.

192.168.100.2	  am.dev.wso2.org

Add the below entry to /etc/hosts and save.

192.168.100.2	  am.dev.wso2.org

Steps

1. Copy "wso2am-2.0.0.zip" (GA pack) to /puppet-modules/modules/wso2am/files.

2. Set $PUPPET_HOME environment variable.
export PUPPET_HOME=<WhereThePuppetIsCheckedOut>/puppet-modules

3. Download and copy "jdk-7u80-linux-x64.tar.gz" to <PUPPET_HOME>/modules/wso2base/files directory.

4. Install Vagrant. (sudo apt-get install vagrant)

5. Install VirtualBox and open it (sudo apt-get install virtualbox).

6. Goto <PUPPET_HOME>/puppet-modules/vagrant

7. Type "vagrant up".

8. Steps to log in will be as follows.

Goto <PUPPET_HOME>/puppet-modules/vagrant (command line window)

Type the following commands.

vagrant ssh am.dev.wso2.org

sudo su

cd /mnt/<IP of the am.dev.wso2.org>/wso2am-2.0.0/

9. To view the logs : "cat repository/logs/wso2carbon.log" or "tail -f repository/logs/wso2carbon.log"

Publisher URL : https://am.dev.wso2.org:9443/publisher
Store URL : https://am.dev.wso2.org:9443/store

10. To cleanup all, type "vagrant destroy -f"

More information :
[1] https://docs.wso2.com/display/PM200/Setting+up+the+Development+Environment

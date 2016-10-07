Puppet Modules - WSO2 API Manager 2.0.0 distributed setup

This puppet module creates a distributed setup as follows.

API Store (ST) = 192.168.100.3	store.dev.wso2.org

API Publisher (PB) = 192.168.100.4	publisher.dev.wso2.org

API Gateway (GW) = 192.168.100.5	gw.dev.wso2.org

API Key manager (KM)  = 192.168.100.2	km.dev.wso2.org

API Traffic manager (TM) = 192.168.100.6 tm.dev.wso2.org

Add the below entries to /etc/hosts and save.

192.168.100.2	km.dev.wso2.org

192.168.100.3	store.dev.wso2.org

192.168.100.4	publisher.dev.wso2.org

192.168.100.5	gw.dev.wso2.org

192.168.100.6	tm.dev.wso2.org

Steps

1.Run the following commands to create a keystore with the hostname of KM and include its certificate in the client trust store. Use “wso2carbon” when prompted for passwords.

keytool -genkey -alias wso2carbon -keyalg RSA -keysize 2048 -keystore wso2carbon.jks -dname "CN=km.dev.wso2.org,OU=Home,O=Home,L=SL,S=WS,C=LK" -storepass wso2carbon -keypass wso2carbon

(km.dev.wso2.org is the domain of the key manager.)

keytool -export -keystore wso2carbon.jks -alias wso2carbon -file wso2carbon.cer

keytool -import -alias wso2carbon -file wso2carbon.cer -keystore client-truststore.jks -storepass wso2carbon

2.Copy client-truststore.jks and wso2carbon.jks files to "puppet-modules/modules/wso2am/files/configs/repository/resources/security" folder. (if no such folder, create it)

3.Copy "wso2am-2.0.0.zip" (GA pack) to /puppet-modules/modules/wso2am/files.

4.Set $PUPPET_HOME environment variable.

export PUPPET_HOME= WhereThePuppetIsCheckedOut/puppet-modules

5.Download and copy "jdk-7u80-linux-x64.tar.gz" to PUPPET_HOME/modules/wso2base/files directory.

6.Install Vagrant. (sudo apt-get install vagrant)

7.Install VirtualBox and open it (sudo apt-get install virtualbox).

8.Goto PUPPET_HOME/puppet-modules/vagrant

9.Run MySQL_DB_ScriptForAPIM200_DistributedSetup.sql in a MySQL server. (credentials : username = root/password = root)

10.Type "vagrant up".

Note :

With the newly introduced "traffic manager" profile in API Manager 2.0.0, there is a requirement to support file deletion in puppet scripts. Once this new profile is introduced, it is needed to remove existing webapps and jaggeryapps from the /repository/deployment/server directory. Otherwise it will throw exceptions at the startup of the traffic manager node.
If we remove the required files manually (by logging in to traffic manager node, execute "rm -r webapps" and "rm -r jaggeryapps" and restarting it), those exceptions are not going to be there.

Commands

cd $PUPPET_HOME/puppet-modules/vagrant


vagrant ssh tm.dev.wso2.org

sudo su

cd /mnt/ IP of the tm.dev.wso2.org /wso2am-2.0.0/

rm -r repository/deployment/server/jaggeryapps/

rm -r repository/deployment/server/webapps/

sh bin/wso2server.sh stop

sh bin/wso2server.sh start -Dprofile=traffic-manager

Please refer [1] for more information.

11.Steps to log in will be as follows.

Goto PUPPET_HOME/puppet-modules/vagrant (command line window)

Type the following commands.

vagrant ssh km.dev.wso2.org

sudo su

cd /mnt/ IP of the km.dev.wso2.org /wso2am-2.0.0/

To view the logs : "cat repository/logs/wso2carbon.log" or "tail -f repository/logs/wso2carbon.log"

Do the same for Store, Publisher, KM and TM (in separate command line windows)

vagrant ssh tm.dev.wso2.org, vagrant ssh store.dev.wso2.org, vagrant ssh publisher.dev.wso2.org and vagrant ssh gw.dev.wso2.org

Publisher URL : https://publisher.dev.wso2.org:9443/publisher
Store URL : https://store.dev.wso2.org:9443/store

12.To cleanup all, type "vagrant destroy -f"


More information :
[1] https://docs.wso2.com/display/CLUSTER44x/Clustering+API+Manager+2.0.0#ClusteringAPIManager2.0.0-ConfiguringtheTrafficManager
[2] https://docs.wso2.com/display/PM200/Setting+up+the+Development+Environment

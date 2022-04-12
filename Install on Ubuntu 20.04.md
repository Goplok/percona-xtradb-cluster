# Percona XtraDB Cluster
## _Install Percoba XtraDB Cluster on Ubuntu 20.04_
----------------
## Prerequisites
- Three Ubuntu server using OS version 20.04
- Internet connection
- Allow port 3306, 4444, 4567, 4568 on each server

## Scenario

| Server Name  | IP Server |
| ------------- | ------------- |
| DB01  | 172.16.80.35  |
| DB02  | 172.16.80.36  |
| DB03  | 172.16.80.37  |

> Note: Server and IP adapted to your environment

## Installation

Run this command on all server DB01, DB02, DB03 to install package from repository

```sh
sudo apt update
sudo apt install -y wget gnupg2 lsb-release curl
wget https://repo.percona.com/apt/percona-release_latest.generic_all.deb
sudo dpkg -i percona-release_latest.generic_all.deb
sudo apt update
sudo percona-release setup pxc80
sudo apt install -y percona-xtradb-cluster
```
_During the installation, you are requested to provide a password for the 'root' user on the database node._

Run this command on all server DB01, DB02, DB03 to stop mysql service
```sh
sudo service mysql stop
```
Edit _/etc/mysql/mysql.conf.d/mysqld.cnf_ configuration on server DB01 as a node 1 under [mysqld] section.

```sh
wsrep_provider=/usr/lib/galera4/libgalera_smm.so
wsrep_cluster_name=pxc-cluster
wsrep_cluster_address=gcomm://172.16.80.35,172.16.80.36,172.16.80.37
wsrep_node_name=pxc1
wsrep_node_address=172.16.80.35
pxc_strict_mode=ENFORCING
pxc-encrypt-cluster-traffic=OFF
```
>Note: The last line pxc-encrypt-cluster-traffic=OFF means we would like to disable encryption for the cluster traffic

Edit _/etc/mysql/mysql.conf.d/mysqld.cnf_ configuration on server DB02 as a node 2 under [mysqld] section.

```sh
wsrep_provider=/usr/lib/galera4/libgalera_smm.so
wsrep_cluster_name=pxc-cluster
wsrep_cluster_address=gcomm://172.16.80.35,172.16.80.36,172.16.80.37
wsrep_node_name=pxc2
wsrep_node_address=172.16.80.36
pxc_strict_mode=ENFORCING
pxc-encrypt-cluster-traffic=OFF
```
>Note: The last line pxc-encrypt-cluster-traffic=OFF means we would like to disable encryption for the cluster traffic

Edit _/etc/mysql/mysql.conf.d/mysqld.cnf_ configuration on server DB03 as a node 3 under [mysqld] section.

```sh
wsrep_provider=/usr/lib/galera4/libgalera_smm.so
wsrep_cluster_name=pxc-cluster
wsrep_cluster_address=gcomm://172.16.80.35,172.16.80.36,172.16.80.37
wsrep_node_name=pxc3
wsrep_node_address=172.16.80.36
pxc_strict_mode=ENFORCING
pxc-encrypt-cluster-traffic=OFF
```
>Note: The last line pxc-encrypt-cluster-traffic=OFF means we would like to disable encryption for the cluster traffic

Run this command on server DB01 as a node 1 to initialize the cluster by bootstrapping the first node

```sh
sudo systemctl start mysql@bootstrap.service
```
Login to your mysql on server DB01 using this command

```sh
mysql -u root -p
show status like 'wsrep%';
```
>Note: Make sure the output like picture below.

Run this command on server DB02 as a node 2 to join the cluster.

```sh
sudo systemctl start mysql
```
>Note: Make sure the output like picture below.

Run this command on server DB03 as a node 3 to join the cluster.

```sh
sudo systemctl start mysql
```
>Note: Make sure the output like picture below.



## References

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>

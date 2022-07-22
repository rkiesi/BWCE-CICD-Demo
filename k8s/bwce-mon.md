# How to build and deploy BWCE-Mon

Docs on [TIBCO BusinessWorks™ Container Edition 2.7.2](https://docs.tibco.com/products/tibco-businessworks-container-edition-2-7-2) has a documentation PDF [Application Monitoring and Troubleshooting](https://docs.tibco.com/pub/bwce/2.7.2/doc/pdf/TIB_bwce_2.7.2_application_monitoring_troubleshooting.pdf). For Kubernetes we need to follow the chapter *Monitoring an Application on Kubernetes*.

1. Get bwce_mon-<version>.zip file from [e-Delivery](http://edelivery.tibco.com).
2. Extract the bwce_mon-<version>.zip file.
3. Build the BWCE-Mon image:  `docker build -t k3d-myregistry.localhost:12345/bwce-mon:2.7.2 .`
4. Push image to local K8s registry: `docker push k3d-myregistry.localhost:12345/bwce-mon:2.7.2`

Now we need a database, either MySQL, Postgres, MS SQL Server or Oracle DB.

## Monitoring Database

For the demo a MySQL server installed on the same VM (EC2) that is hosting Rancher/K3s is used.
The database is hosted externally from the K3s cluster and must be created upfront!

For configuration connect to the local mysql server as root and configure it for bwcemon:

```
$ mysql -u root -p

mysql> CREATE USER 'bwcemon'@'%' IDENTIFIED WITH mysql_native_password BY 'passw0rd';

mysql> CREATE DATABASE IF NOT EXISTS bwcemon CHARACTER SET utf-8mb4;

mysql> GRANT ALL PRIVILEGES ON bwcemon.* TO 'bwcemon'@'%' WITH GRANT OPTION;

# Check!
# - reason: node.js mqsldb module is not yet capable of using the newer MySQL authentication method!
# - listed plugin must be mysql_native_password!

mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| user             | authentication_string                                                  | plugin                | host      |
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| bwcemon          | *74B1C21ACE0C2D6B0678A5E503D2A60E8F9651A3                              | mysql_native_password | %         |
```


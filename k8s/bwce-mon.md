# How to build and deploy BWCE-Mon

Docs on [TIBCO BusinessWorks™ Container Edition 2.7.2](https://docs.tibco.com/products/tibco-businessworks-container-edition-2-7-2) has a documentation PDF [Application Monitoring and Troubleshooting](https://docs.tibco.com/pub/bwce/2.7.2/doc/pdf/TIB_bwce_2.7.2_application_monitoring_troubleshooting.pdf). For Kubernetes we need to follow the chapter *Monitoring an Application on Kubernetes*.

1. Get bwce_mon-<version>.zip file from [e-Delivery](http://edelivery.tibco.com).
2. Extract the bwce_mon-<version>.zip file.
3. Build the BWCE-Mon image:  `docker build -t k3d-myregistry.localhost:12345/bwce-mon:2.7.2 .`
4. Push image to local K8s registry: `docker push k3d-myregistry.localhost:12345/bwce-mon:2.7.2`

Now we need a database, either MySQL, Postgres, MS SQL Server or Oracle DB.




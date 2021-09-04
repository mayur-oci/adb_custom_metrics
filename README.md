

# Configuring Custom Metrics for Oracle Autonomous Database, by leveraging Oracle Cloud Monitoring Service
## Introduction
[Oracle Autonomous Database](https://www.oracle.com/autonomous-database/)(ADB) is revolutionizing how data is managed with the introduction of the world’s first "self-driving" database. ADB is powering critical business applications of enterprises, all over the world, as their primary data source. 

ADB provides many important database related [service metrics](https://docs.oracle.com/en-us/iaas/Content/Database/References/databasemetrics_topic-Overview_of_the_Database_Service_Autonomous_Database_Metrics.htm) out of the box, thanks to its deep integration with OCI Monitoring Service. 
That being said, many our innovative customers wish to take their Observability journey a step further:
These **customers want to collect, publish and analyse their own metrics, related to the application data stored in the ADB**. In Oracle Monitoring Service terminology we call these [*custom metrics*](https://docs.oracle.com/en-us/iaas/Content/Monitoring/Tasks/publishingcustommetrics.htm). These are the metrics which your applications can collect and post it to Oracle Monitoring Service, with simple REST API or OCI SDK. 

In this tutorial, I will showcase how easily we can publish custom metrics from your ADB service, with just a few lines of PL/SQL script and few clicks on OCI Console! We, at Oracle Cloud believe in meeting customers where they are, in their cloud journey!

We will use ecommerce shopping order data as an example to see how we can compute, collect metrics on the this data. And finally we will post these custom metrics Oracle Cloud Monitoring Service. 

Custom metrics metrics are first class citizens of Oracle Cloud Monitoring Service, on par with native metrics. You can analyse them with the same powerfull *Metrics Query Language* and setup Alarms on them to notify you whenever any event of interest or trouble happen.

## Prerequisites 
### Infrastructure
 1. Access to Oracle cloud free tier or paid account.
 2. You can use any type of Oracle Autonomous Database Instance i.e.; shared or dedicated. For the tutorial though, Oracle Autonomous Transaction Processing(ATP) instance with just 1 OCPU and 1 TB of storage, is sufficient. 
 
 ### Software Tools
 3. Basic PL/SQL familiarity.
 4. OCI Console familiarity.
 5. If you are going to use SQL Developer or Other desktop clients, then you also need to know about [how to connect to ADB using Wallet](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/cswgs/autonomous-connect-sql-developer.html#GUID-14217939-3E8F-4782-BFF2-021199A908FD).  To make things easier, In the tutorial we are going to use, [SQL Developer Web](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/sql-developer-web.html#GUID-C32A78E5-4C5F-476F-86AB-AEEEA9CF2704), available right from OCI Console page for ATP. 
 6. ADMIN user access to your ADB/ATP instance, incase you want to run the scripts of the tutorial, in new different DB schema other than the ADMIN. If you already have dedicated seperate schema, this requirenment can be skipped.
 7. Basic familiarity with Oracle Cloud Concepts like [Monitoring Service](https://docs.oracle.com/en-us/iaas/Content/Monitoring/Concepts/monitoringoverview.htm), [Dynamic Groups and Resouce Principals](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/overview.htm). 
 
 ## Solution at a glance:
![enter image description here](https://github.com/mayur-oci/adb_custom_metrics/blob/main/images/image.png?raw=true)
 As shown above we will have simple PL/SQL script deployed in our ADB instance,  which is scheduled run periodically computes, collects & posts the custom metrics Oracle Monitoring Service. 
 
## Steps at high level
 1. Create Dynamic Group named ***adb_dg*** for your ADB instance(or instances) and authorize the same dynamic group, to post metrics to *Oracle Cloud Monitoring Service* with policy named ***adb_dg_policy***.
 2. Create new DB user/schema named ***ecommerce_user*** in your ADB for this tutorial. Please note, this step optional since you can use any of your existing user/schema for this tutorial. 
 3. Create example data table ***SHOPPING_ORDER*** to showcase computation of metrics on a database tables. You can create this table in newly created schema in step 2 or in already existing DB schema of your choice. 
 4. Run dummy PL/SQL scripts with necessery stored procedures to populate data in ***SHOPPING_ORDER*** table. 
 5. Schedule and run PL/SQL scripts with necessery stored procedures and functions to compute, collect and post metrics to compute, collect & post the custom metrics *Oracle Monitoring Service*. 

 ## Detailed Steps:

 


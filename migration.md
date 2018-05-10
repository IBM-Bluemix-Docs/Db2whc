---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-08"

---

<!-- Attribute definitions --> 
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Migration to IBM Cloud
{: #migration}

You can load data from a data file in a delimited format (CSV or TXT) located on a local network or in an object store (Amazon S3 or IBM Cloud Object Storage) to {{site.data.keyword.dashdblong}}. You can even migrate your data from an on-premises system.
{: shortdesc}

## Load data from object store
{: #cos}

To load data from Amazon S3, select one of the following methods:
  * {{site.data.keyword.dashdbshort_notm}} web console. **Load > Amazon S3**. 
  * External Tables directly. The following is an example SQL statement:

    ```
    INSERT INTO <table-name> SELECT * FROM EXTERNAL '<mys3file.txt>' USING
      (CCSID 1208 s3('s3.amazonaws.com', 
      '<S3-access-key-ID>',
      '<S3-secret-access-key>', 
      '<my_bucket>'
         )
      )      
    ```

To load data from IBM Cloud Object Storage by using External Tables directly, the following is an example SQL statement:

```
INSERT INTO <table-name> SELECT * FROM EXTERNAL '<mys3file.txt>' USING
  (CCSID 1208 s3('s3-api.us-geo.objectstorage.softlayer.net', 
  '<S3-access-key-ID>',
  '<S3-secret-access-key>', 
  '<my_bucket>'
     )
  )      
```

**Note:** For IBM Cloud Object Storage, to create HMAC credentials when creating new service credentials, specify {"HMAC:true"} in the *Add Inline Configuration Parameters* field.

## Migrate data from on-premises system
{: #onprem}

To migrate your data from an on-premises system, choose one of the following methods depending on the size of your data set:
* Less than 25 TB of data: [IBM Lift CLI](#lift)
* 25 TB of data and greater: [IBM Cloud Mass Data Migration](#mdms)

### Lift CLI
{: #lift}

The Lift CLI is an application that you can use without charge to migrate your data to the {{site.data.keyword.Bluemix_notm}} from the various data sources listed in Table 1. 

| Target database on IBM Cloud | Data source |
|------------------------------|-------------|
| IBM Db2 Warehouse on Cloud   | IBM Db2 |
|                              | IBM Db2 Warehouse |
|                              | IBM Integrated Analytics System |
|                              | IBM PureData System for Analytics |
|                              | Oracle Database |
|                              | Microsoft SQL Server |
|                              | CSV file format |
{: caption="Table 1. Migration data sources" caption-side="top"}

To download and install Lift CLI, see: [Download Lift CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lift.ng.bluemix.net/#download){:new_window}.

For step-by-step instructions about migrating your data to the {{site.data.keyword.Bluemix_notm}} by using Lift CLI, see: [Migrate data to IBM Db2 Warehouse on Cloud ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lift.ng.bluemix.net/#docs){:new_window}.

### IBM Cloud Mass Data Migration
{: #mdms}

A fast, simple, secure way to physically transfer terabytes to petabytes of data to the {{site.data.keyword.Bluemix_notm}}. Mass Data Migration is a mobile storage device with 120 TB of usable storage capacity to accelerate moving data to the {{site.data.keyword.Bluemix_notm}}. Overcome common transfer challenges like high costs, long transfer times, and security concerns – all in a single service.

![View of the Mass Data Migration device](images/mdms.svg)

For more information about the Mass Data Migration device, see: [Getting started with IBM Cloud Mass Data Migration](/docs/infrastructure/mass-data-migration/index.html#getting-started-with-ibm-cloud-mass-data-migration){:new_window}.


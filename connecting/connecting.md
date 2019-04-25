---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-01"

keywords:

subcollection: Db2whc

---

<!-- Attribute definitions --> 
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}

# Connecting overview
{: #connect_ov}

You can connect command-line interfaces, IBM® or third-party applications and tools, or apps that you create to your {{site.data.keyword.dashdbshort_notm}} database. 
{: shortdesc}

## Prerequisites
{: #prereqs}

Before attempting to connect to your {{site.data.keyword.dashdbshort_notm}} database, verify that you have the necessary prerequisites. 

- Collect database details and credentials

   To connect to your database, you need database details (such as the host name), as well as credentials (such as a user ID and password.) You can collect this connection information from the {{site.data.keyword.dashdbshort_notm}} web console.

- Verify that a supported driver is installed

   - If your application or tool already contains the Db2 v11.1 IBM Data Server Driver Package, then your application or tool is able to connect to your {{site.data.keyword.dashdbshort_notm}} database using that driver.
   - Otherwise, install the Db2 driver package, which you can download from the {{site.data.keyword.dashdbshort_notm}} web console.

- Configure your environment

  - Add entries to the driver configuration file, `db2dsdriver.cfg`, for your database.
  - Secure Sockets Layer (SSL)

    You can choose to connect with or without SSL. Connection details, such as which port to use and the connection string, depend on whether or not you use SSL connections.

    To use SSL connections, you need a CA certificate:
    - If you use the latest {{site.data.keyword.dashdbshort_notm}} driver package, the certificate file is bundled with the package and will be used for connections.
    - If you use the IBM Data Server Driver Package, you can download the SSL certificate from the {{site.data.keyword.dashdbshort_notm}} web console.

- Confirm ports are available

   If your network is behind a firewall, confirm that communications are permitted on port number `50000` for standard protocols or port number `50001` for SSL connections.

<!-- Before you can connect to your {{site.data.keyword.dashdbshort_notm}} database, verify that you completed downloading and installing the necessary components on the prerequisites checklist: 

- [Prerequisites checklist](prereqs.html) -->

### Collecting connection information
{: #collect_info}

- [Database details and connection credentials](/docs/services/Db2whc/connecting?topic=Db2whc-db_details_cxn_creds#db_details_cxn_creds)

### Downloading and installing driver package
{: #dl_install}

- [Download driver package](/docs/services/Db2whc/connecting?topic=Db2whc-dr_pkg#dr_pkg)
- [Installing on Linux or PowerLinux](/docs/services/Db2whc/connecting?topic=Db2whc-install_dr_pkg_linux#install_dr_pkg_linux)
- [Installing on Mac OS X](/docs/services/Db2whc/connecting?topic=Db2whc-install_dr_pkg_mac#install_dr_pkg_mac)
- [Installing on Windows](/docs/services/Db2whc/connecting?topic=Db2whc-install_dr_pkg_windows#install_dr_pkg_windows)

### Configuring your environment
{: #cfg_env}

- [Configuring your environment](/docs/services/Db2whc/connecting?topic=Db2whc-cfg_loc_env#cfg_loc_env)
- [Secure Sockets Layer (SSL) support](/docs/services/Db2whc/connecting?topic=Db2whc-ssl_support#ssl_support)

## Connectivity options
{: #connect_opts}

{{site.data.keyword.dashdbshort_notm}} offers multiple secure connectivity options depending on your application connection requirements.  
{: shortdesc}

See [Connectivity options](/docs/services/Db2whc/connecting?topic=Db2whc-connect_options#connect_options).

## Connecting programmatically
{: #conx_prgrm}

You can use common programming languages to create applications that connect to a {{site.data.keyword.dashdbshort_notm}} database.
{: shortdesc}

- [JDBC](/docs/services/Db2whc/connecting?topic=Db2whc-con_prog_jdbc#con_prog_jdbc)
- [Microsoft Windows ODBC or CLI](/docs/services/Db2whc/connecting?topic=Db2whc-con_prog_odbc_cli#con_prog_odbc_cli)
- [.NET](/docs/services/Db2whc/connecting?topic=Db2whc-con_prog_net#con_prog_net)
- [ODBC Data Source Administrator](/docs/services/Db2whc/connecting?topic=Db2whc-con_prog_odbc_dsa#con_prog_odbc_dsa)
- [PHP](/docs/services/Db2whc/connecting?topic=Db2whc-con_prog_php#con_prog_php)
- [REST API](/docs/services/Db2whc/connecting?topic=Db2whc-con_rest_api#con_rest_api)
<!-- - [C++]() -->
<!-- - [Java]() -->
<!-- - [Node.js]() -->
<!-- - [Perl]() -->
<!-- - [Python]() -->

## Integrating apps and tools
{: #conx_apps_tools}

You can also connect external applications and tools to {{site.data.keyword.dashdbshort_notm}} and use them to further manage or analyze your data. For example:

### Data integration
{: #di}

- Connect your {{site.data.keyword.Bluemix_short}} applications that need an analytics database.
- [DataStage](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#datastage)
- [Informatica](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#informatica)
- [Lift ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://www.lift-cli.cloud.ibm.com/#docs){:new_window}
- [InfoSphere Data Replication](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#idr)
- [Segment ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://segment.com/docs/destinations/db2/){:new_window}
- [Data Studio](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#data_studio)
- [Data Server Manager](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#dsm)
- [CLPPLUS](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#clpplus)
- [Aginity Workbench to migrate Netezza® data models and data to {{site.data.keyword.dashdbshort_notm}}](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#aginity_wb)
- [InfoSphere Data Architect to design and deploy your database schema](/docs/services/Db2whc/connecting?topic=Db2whc-data_int#ida)

### Data visualization/BI
{: #dvis_bi}

- [Cognos Analytics to run Business Intelligence reports against your data](/docs/services/Db2whc/connecting?topic=Db2whc-data_vis_bi#cognos)
- [Looker ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.looker.com/setup-and-management/connecting-to-db){:new_window}
- [Tableau](/docs/services/Db2whc/connecting?topic=Db2whc-data_vis_bi#tableau)
- [Microsoft Excel](/docs/services/Db2whc/connecting?topic=Db2whc-data_vis_bi#excel)
- [Esri ArcGIS for Desktop to perform geospatial analytics and map publishing with your data](/docs/services/Db2whc/connecting?topic=Db2whc-data_vis_bi#esri_arcgis)

### Data science
{: #dsci}

- [Watson Studio (formerly IBM Data Science Experience)](/docs/services/Db2whc/connecting?topic=Db2whc-ds#watson_studio)
- [SPSS Statistics](/docs/services/Db2whc/connecting?topic=Db2whc-ds#spss_stats)
- [SAS](/docs/services/Db2whc/connecting?topic=Db2whc-ds#sas)
- [Local R development environment](/docs/services/Db2whc/connecting?topic=Db2whc-ds#r_dev_env)

## Connecting to another Db2 database
{: #fed}

Db2 data virtualization (also known as federation) is supported by {{site.data.keyword.dashdbshort_notm}}. Data virtualization gives you single-query access to all of your data that is on multiple distributed databases anywhere in your organization. You can access data that is on any of your Db2 or Informix data sources, both in the cloud and on premises. 

This function is supported on all versions of {{site.data.keyword.dashdbshort_notm}}, except for the Entry plan. However, you can use the Entry plan as a target from which you can pull data.

- [Data virtualization (federation)](/docs/services/Db2whc?topic=Db2whc-data_virt_fed#data_virt_fed)



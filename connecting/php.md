---

copyright:
  years: 2014, 2019
lastupdated: "2018-09-25"

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

# Connecting programmatically with PHP
{: #con_prog_php}

Define a connection between a PHP application and a {{site.data.keyword.dashdbshort_notm}} database.
{: shortdesc}

## Prerequisites

Before attempting to connect to your {{site.data.keyword.dashdbshort_notm}} database, verify that you have the necessary [prerequisites](/docs/services/Db2whc/connecting/connecting.html#prereqs).

<!-- Before you can connect to your database, you must perform the following steps:

- [Verify prerequisites](prereqs.html), including installing driver packages, configuring your local environment, and downloading SSL certificates (if needed)
- Collect [connection information](credentials.html), including database details such as host name and port numbers, and connection credentials such as user ID and password -->

## Procedure

### Scenario 1: Connecting from outside {{site.data.keyword.Bluemix_notm}}:
        
1. Download the [Db2 driver package](/docs/services/Db2whc/connecting/driver_pkg.html) from the web console, and then install the driver package on the machine where your PHP application will run.
                
2. Use the [`odbc_connect` function ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://php.net/manual/en/function.odbc-connect.php){:new_window} to connect to the BLUDB database.
    
   PHP code example:

   ```
   <?php
   $database = "BLUDB";        # Get these database details from
   $hostname = "<Host-name>";  # the web console
   $user     = "<User-ID >";   #
   $password = "<Password>";   #
   $port     = 50000;          #
   $ssl_port = 50001;          #
   # Build the connection string
   #
   $driver  = "DRIVER={IBM DB2 ODBC DRIVER};";
   $dsn     = "DATABASE=$database; " .
              "HOSTNAME=$hostname;" .
              "PORT=$port; " .
              "PROTOCOL=TCPIP; " .
              "UID=$user;" .
              "PWD=$password;";
   $ssl_dsn = "DATABASE=$database; " .
              "HOSTNAME=$hostname;" .
              "PORT=$ssl_port; " .
              "PROTOCOL=TCPIP; " .
              "UID=$user;" .
              "PWD=$password;" .
              "SECURITY=SSL;";
   $conn_string = $driver . $dsn;     # Non-SSL
   $conn_string = $driver . $ssl_dsn; # SSL
   # Connect
   #
   $conn = odbc_connect( $conn_string, "", "" );
   if( $conn )
   {
       echo "Connection succeeded.";
       # Disconnect
       #
       odbc_close( $conn );
   }
   else
   {
       echo "Connection failed.";
   }
   ?>
   ```

   Saving this PHP code example to a script file called `C:\sample.php` and then running the script from a command line produces the following output:

   ```
   C:\> php –f sample.php

   Connection succeeded.
   ```

### Scenario 2: Connecting from a PHP web app in {{site.data.keyword.Bluemix_notm}}

1. From the {{site.data.keyword.Bluemix_notm}} catalog, create a new PHP App.
        
2. From the "Getting Started" section of the new PHP App in your {{site.data.keyword.Bluemix_notm}} dashboard, download the App starter code to your local work directory.
        
3. In your {{site.data.keyword.Bluemix_notm}} dashboard, create a new connection from your Db2 service to your new PHP App. (Creating this Connection in {{site.data.keyword.Bluemix_notm}} makes the `VCAP_SERVICES` environment variable available to your PHP App. The `VCAP_SERVICES` environment variable contains database details for your Db2 service. Using `VCAP_SERVICES` is more convenient than hard-coding the database details in your PHP App.)
        
4. In your local working directory, update the `index.php` file to connect to the BLUDB database by using the [`db2_connect` function ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://php.net/manual/en/function.db2-connect.php){:new_window}.
        
   Example:

   ```
   <!DOCTYPE html>
   <html>
   <head>
       <title>PHP Starter Application</title>
       <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
       <link rel="stylesheet" href="style.css" />
   </head>
   <body>
   <?php
   if( getenv( "VCAP_SERVICES" ) )
   {
       # Get database details from the VCAP_SERVICES environment variable
       #
       # *This can only work if you have used the Bluemix dashboard to 
       # create a connection from your dashDB service to your PHP App.
       #
       $details  = json_decode( getenv( "VCAP_SERVICES" ), true );
       $dsn      = $details [ "dashDB" ][0][ "credentials" ][ "dsn" ];
       $ssl_dsn  = $details [ "dashDB" ][0][ "credentials" ][ "ssldsn" ];
       # Build the connection string
       #
       $driver = "DRIVER={IBM DB2 ODBC DRIVER};";
       $conn_string = $driver . $dsn;     # Non-SSL
       $conn_string = $driver . $ssl_dsn; # SSL
       $conn = db2_connect( $conn_string, "", "" );
       if( $conn )
       {
           echo "<p>Connection succeeded.</p>";
           db2_close( $conn );
       }
       else
       {
           echo "<p>Connection failed.</p>";
       }
   }
   else
   {
       echo "<p>No credentials.</p>";
   }
   ?>
   </body>
   </html>
   ```

5. From your local working directory, push the updates to {{site.data.keyword.Bluemix_notm}} by following the instructions in the "Getting Started" section of the PHP App in your {{site.data.keyword.Bluemix_notm}} dashboard. Then restart the App in {{site.data.keyword.Bluemix_notm}} and view the App in a browser.



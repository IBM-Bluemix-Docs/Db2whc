---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-15"

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

# Identity and access management (IAM) on {{site.data.keyword.Bluemix_notm}}
{: #iam}

Identity and access management (IAM) enables you to securely authenticate users for platform services and control access to resources consistently across the {{site.data.keyword.Bluemix_notm}} platform. For example, with only a single login to {{site.data.keyword.Bluemix_notm}} with your IBMid, you have access to any of your service consoles and their applications without having to log in to each of them separately.
{: shortdesc}

## Is IAM enabled on your instance?
{: #enabled}

Over a period of time, the {{site.data.keyword.dashdblong}} managed database instances on {{site.data.keyword.Bluemix_notm}} will be enabled to use IAM for access control. To check that IAM is enabled on your instance, run the following query:

`SELECT CASE WHEN VALUE = 'IBMIAMauth' THEN 1 ELSE 0 END AS IAM_ENABLED FROM SYSIBMADM.DBMCFG WHERE NAME = 'srvcon_gssplugin_list'`

If the returned value of **IAM_ENABLED** is 1, then IAM is enabled on your instance.

## Features of {{site.data.keyword.Bluemix_notm}} IAM
{: #features}

The following IAM features are implemented for the {{site.data.keyword.dashdbshort_notm}} managed service with two types of supported identities:

**IBMid**

Users with an IBMid must be added to each database service instance by the database administrator through the console or REST API before these users can connect to the particular database service instance. Just like for a non-IBMid user, a user ID for the database service instance must be entered at the same time that the IBMid user is added. This user ID needs to be unique within the database service instance. This user ID is also the authorization (AUTH) ID within the database. The database administrator can grant and revoke permissions based on the AUTH ID.

**Service IDs**

A service ID identifies a service or application similar to how a user ID identifies a user. The service IDs are IDs that can be used by applications to authenticate with an {{site.data.keyword.Bluemix_notm}} service. A service ID represents a separate entity from the owning IBMid. Therefore, different authorities and permissions can be granted specific to the service ID within the database. Service IDs do not have passwords. An API key must be created for each service ID for the service ID to connect to the database service instance. For more information about service IDs, see: [Introducing {{site.data.keyword.Bluemix_notm}} IAM Service IDs and API Keys ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## Client connections and user logins
{: #connect}

**Prerequisite**: Db2 Client V11.1 FP3 and later.

The following methods can be used for IAM authentication:

**Access token**

An access token can be obtained from the IAM service directly by the application through the REST API by using an API key. For more information, see: [Getting an {{site.data.keyword.Bluemix_notm}} IAM token by using an API key ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/iam/apikey_iamtoken.html#iamtoken_from_apikey){:new_window}. The access token has a default validity period of 60 minutes before it expires. If the token has expired, the Db2 server won't allow the connection to be established. The token isn’t checked for expiry after the connection is established. Just as it was prior to IAM integration, the connection will stay connected until the application disconnects or the connection is terminated due to other reasons.

```
curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=<apikey>" \
  "https://iam.bluemix.net/identity/token"
```

<!-- `curl -k -X POST \ --header "Content-Type:application/x-www-form-urlencoded" \ --header "Accept: application/json" \ --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \ --data-urlencode "apikey=<apikey>" \ "https://iam.bluemix.net/identity/token"` -->

An access token identifies an IBMid user or a service ID to the database. The database server verifies the validity of the access token before accepting it. This is the best method if the application needs to connect to more than one database service instance or other {{site.data.keyword.Bluemix_notm}} service instances because it minimizes the communication to the IAM service to validate the access token.

**API key**

Multiple API keys can be created for each IBMid user or service ID. Each API key is typically created for a single application. It allows the application to connect to the database service instance as long as the owning IBMid or service ID is added as a user to the same database service instance. The API key has the same authorities and permissions within the database as the owning IBMid or service ID. If an application should no longer be allowed to connect to the database, the corresponding API key can be removed. This method of authentication requires less changes in the application than using an access token as it requires no direct interaction with the IAM service. For more information about creating and managing API keys, see: [Managing user API keys ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/iam/userid_keys.html#userapikey){:new_window}.

**IBMid/password**

The IBMid/password can be used to log in to the console and can also be used within the application in the same ways that a user ID/password is allowed. The exception occurs when the IBMid is federated because a redirection to your organization’s login page cannot be done. However, the recommended method for an application to establish a connection to the database service instance is to use an access token.

## Supported interfaces
{: #sup-if}

The following database client interfaces are supported:

* [ODBC](#odbc-clpplus)
* [CLP](#odbc-clpplus)
* [CLPPLUS](#odbc-clpplus)
* [JDBC](#jdbc)

### ODBC, CLP, and CLPPLUS
{: #odbc-clpplus}

For an ODBC application or a command line client (CLP, CLPPLUS) to connect to a Db2 server using IAM authentication, a data source name (DSN) needs to be configured first in a `db2dsdriver.cfg` configuration file by running the following command:

`db2cli writecfg add -dsn <dsn_alias> -database <database_name> -host <host_name_or_IP_address> -port 50001 -parameter "Authentication=GSSPLUGIN;SecurityTransportMode=SSL"`

The `db2dsdriver.cfg` configuration file is an XML file, typically located in the `sqllib/cfg` directory, that contains a list of DSN aliases and their properties.

The following example of a `db2dsdriver.cfg` configuration file shows the configurations that are used to establish a connection to a database service instance. The configuration file provides the DSN alias, the database name, the host name (or IP address), and the **Authentication** type and **SecurityTransportMode** parameter values:

```
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
<configuration>
        <dsncollection>
        <dsn alias="<data_source_name>" name="bludb" host="<host_name_or_IP_address>" port="50001">
            <parameter name="Authentication" value="GSSPLUGIN"/>
            <parameter name="SecurityTransportMode" value="SSL"/>
        </dsn>
        </dsncollection>
        <databases>
            <database name="bludb" host="<host_name_or_IP_address>" port="50001"/>
        </databases>
</configuration>
```

* The ODBC connection string can contain one of the following:

    **Access token**

    `DSN=<dsn>;ACCESSTOKEN=<access_token>`

    **API key**

    `DSN=<dsn>;APIKEY=<api_key>`

    **IBMid/password**
    
    `DSN=<dsn>;UID=<ibmid>;PWD=<password>`

    For ODBC, the **AUTHENTICATION=GSSPLUGIN** can be specified in either the `db2dsdriver.cfg` configuration file or in the application’s connection string.

* The CLP and CLPPLUS **connect** command can contain one of the following:

    **Access token**

    Connect to the DSN alias (`@<data_source_name>`) and pass the access token by running the following command at the CLP or CLPPLUS command prompt or script:

    `connect @<data_source_name> using(accesstoken <access_token_string>)`

    **API key**

    Connect to the DSN alias (`@<data_source_name>`) with an API key by running the following command at the CLP or CLPPLUS command prompt or script:

    `connect @<data_source_name> using(apikey <api-key-string>)`

    **IBMid/password**

    Connect to the DSN alias (`@<data_source_name>`) with an IBMid/password by running the following command at the CLP or CLPPLUS command prompt or script:

    `connect <IBMid>/<password>@<data_source_name>`

    For more details about connecting to DSN aliases with CLPPLUS, see: [DSN aliases in CLPPlus ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.swg.im.dbclient.clpplus.doc/doc/c0057148.html){:new_window}.

### JDBC
{: #jdbc}

Type 4 JDBC Driver is supported for IAM authentication.

The following examples show connection snippets for the three methods:

**Access token**

```
DB2SimpleDataSource dataSource;

dataSource.setDriverType( 4 );
dataSource.setDatabaseName( "BLUDB" );
dataSource.setServerName( "<host_name_or_IP_address>" );
dataSource.setPortNumber( 50001 );
dataSource.setSecurityMechanism( com.ibm.db2.jcc.DB2BaseDataSource.PLUGIN_SECURITY );
dataSource.setPluginName( "IBMIAMauth" );
dataSource.setAccessToken( "<access_token>" );
Connection conn = dataSource.getConnection( );
```

**API key**

```
DB2SimpleDataSource dataSource;

dataSource.setDriverType( 4 );
dataSource.setDatabaseName( "BLUDB" );
dataSource.setServerName( "<host_name_or_IP_address>" );
dataSource.setPortNumber( 50001 );
dataSource.setSecurityMechanism( com.ibm.db2.jcc.DB2BaseDataSource.PLUGIN_SECURITY );
dataSource.setPluginName( "IBMIAMauth" );
dataSource.setApiKey( "<api_key>" );
Connection conn = dataSource.getConnection( );
```

**IBMid/password**

```
DB2SimpleDataSource dataSource;

dataSource.setDriverType( 4 );
dataSource.setDatabaseName( "BLUDB" );
dataSource.setServerName( "<host_name_or_IP_address>" );
dataSource.setPortNumber( 50001 );
dataSource.setSecurityMechanism( com.ibm.db2.jcc.DB2BaseDataSource.PLUGIN_SECURITY );
dataSource.setPluginName( "IBMIAMauth" );
Connection conn = dataSource.getConnection( "<user_ID>", "<password>" );
```

## Console user experience
{: #console-ux}

The service console login page has the option to log in with your IBMid and password. After the **Sign In via IBMid** button is clicked, the user is directed to the IAM login page, on which the password is entered. When authentication is completed, the user is redirected back to the console. Before such login can be successful, the IBMid user must be added to each database service instance by the database administrator through the console or REST API. Just like for a non-IBMid user, a user ID for the database service instance must be entered at the same time that the IBMid user is added. The user ID needs to be unique within the database service instance. This user ID is also the authorization (AUTH) ID within the database.

## REST API experience
{: #api}

The {{site.data.keyword.dashdbshort_notm}} REST API was enhanced to also accept an IAM access token for the functions that previously accepted a database service-generated access token.

* To add a new IBMid user, run the following example API call:

  `curl --tlsv1.2 "https://<IPaddress>/dbapi/v3/users" -H "Authorization: Bearer <access_token>" -H "accept: application/json" -H "Content-Type: application/json" -d "{"id":"<userid>","ibmid":"<userid>@<email_address_domain>","role":"bluadmin","locked":"no","iam":true}"`

  The `<userid>` value for `"id"` and `"ibmid"` do not have to be the same. The two different IDs are not linked together in any way.
  {: note}

* To migrate an existing non-IBMid database user (for example, `abcuser`) and make them an IBMid user, first delete the non-IBMid user ID by running the following example API call:

  `curl --tlsv1.2 -X DELETE "https://<IPaddress>/dbapi/v3/users/abcuser" -H "Authorization: Bearer <access_token>" -H "accept: application/json" -H "Content-Type: application/json"`

  Next, re-add the user with an IBMid that is the same as the previous user ID (`abcuser`) by running the following example API call:

  `curl --tlsv1.2 "https://<IPaddress>/dbapi/v3/users" -H "Authorization: Bearer <access_token>" -H "accept: application/json" -H "Content-Type: application/json" -d "{"id":"abcuser","ibmid":"abcuser@<email_address_domain>","role":"bluadmin","locked":"no","iam":true}"`

  Because the user ID (`abcuser`) remains the same for the new IBMid for that user, the granted database permissions for the user remain unchanged. If a list of existing non-IBMid database users needs to be migrated to make them IBMid users, you are required to run the previous pair of API calls for each user.

* To add many new IBMid users at one time, create a batch file that lists the following example API calls, one for each user:

  ```
  curl --tlsv1.2 "https://<IPaddress>/dbapi/v3/users" -H "Authorization: Bearer <access_token>" -H "accept: application/json" -H "Content-Type: application/json" -d "{"id":"<userid1>","ibmid":"<userid1>@<email_address_domain>","role":"bluadmin","locked":"no","iam":true}"
  curl --tlsv1.2 "https://<IPaddress>/dbapi/v3/users" -H "Authorization: Bearer <access_token>" -H "accept: application/json" -H "Content-Type: application/json" -d "{"id":"<userid2>","ibmid":"<userid2>@<email_address_domain>","role":"bluadmin","locked":"no","iam":true}"
  .
  .
  .
  ```

For more details about your service's API, see: [{{site.data.keyword.dashdbshort_notm}} REST API ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ibm.biz/db2whc_api){:new_window}.

## IBMid federation
{: #fed}

To use your own identity provider such as LDAP, you must first federate your LDAP server with IBMid. For instructions about federating your LDAP server with IBMid, see: [IBMid Enterprise Federation Adoption Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://ibm.ent.box.com/notes/78040808400?s=nhuzrhlsn0ly338zddomx329tlpmfghc){:new_window}. After IBMid federation is completed and the allowed users are added to the database service instance by the database administrator, these users can log in to the console with their company user ID and password. Alternatively, these users can use an access token or API key that represents their user ID to connect to the database service instance through one of the supported database client interfaces.

## Restrictions
{: #restrictions}

The following are restrictions with regard to IAM authentication:

* IAM authentication for a Db2 client that is connecting to a Db2 server is only supported over an SSL connection.
* IBMid federation is supported to allow custom user management portal or server to be used for authentication. Such support does not include any group federation.
* IAM authentication for database federation is not supported.
* Running IDA and user-defined extension (UDX) commands through CLPPlus are not supported.
* Type 2 JDBC Driver is not supported.





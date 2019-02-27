---

copyright:
  years: 2014, 2019
lastupdated: "2018-07-18"

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

# 資料虛擬化（聯合）
{: #data_virt_fed}

{{site.data.keyword.dashdbshort_notm}} 支援 Db2 資料虛擬化（也稱為聯合）。資料虛擬化讓您能對位於組織各地多個分散式庫料庫上的所有資料，進行單一查詢的存取。您可以存取位於任何 Db2 或 Informix 資料來源的資料，不論是在雲端或內部部署皆可。
{: shortdesc}

這項功能在所有版本的 {{site.data.keyword.dashdbshort_notm}} 上皆受支援，但入門方案例外。不過，您可以使用入門方案作為從中取回資料的目標。

## 使用案例
{: #use_cases}

### 合併資料來源

藉由聯合位於雲端及組織任意處內部部署的資料來源，您的虛擬化資料會彷彿從單一來源擷取。資料虛擬化免除了繁重且成本高昂的資料移轉處理程序，讓您能有效率且有成本效益地分析所有資料。

<!-- A company may have started their operations with an on-premises Db2 server. As cloud technology becomes more widespread and companies start to operate on cloud in a cost-effective fashion, there will be continued Cloud growth. However, the organization’s data on both sources remain as a critical component to their decision-making processes. By way of example, a client operating in retail industry needs to be able to access all data, say customer information, to run further analysis on their customers’ consumption behaviors. They need to be able to identify customers, match their records on cloud with already existing ones from an on-premises database and compose them as if the data is being retrieved from a single source. Federation capability here prevents the burdensome data migration process and allows the user to access the data without moving the data.

located in the cloud and on-premises -->

### 連接至 Db2 on Cloud

Db2 系列產品的使用者，可以聯合來自 {{site.data.keyword.Db2_on_Cloud_short}} 和 {{site.data.keyword.dashdbshort_notm}} 資料庫的資料。從共用的資料存取介面，您可以輕鬆新增、查詢和分析資料，而不需複雜的 ETL 處理程序，也不需要任何額外的程式碼。

<!-- Db2 family users would now be able to federate data between Db2 on Cloud and Db2 Warehouse on Cloud. By being provided a common interface for accessing the data, a user can now easily add or query data from or to the Warehouse without complex ETL processes or any additional code. -->

<!-- ### Sharded data across multiple servers

At times, you might choose to partition (shard) your data. With federation capabilities, sharded data can be queried with a unified interface. Federation gives you the ability to better balance your workloads, scale specific parts of an app, and create microservices that work together. -->

<!-- At times, users may choose to partition (shard). With federation capabilities, data can be queried with a unified interface and this lets the user better balance the workload, scale specific parts of an app or create microservices that work together. -->

### 在固定限制之後再提高資料庫容量

聯合讓您能藉由與雲端資料庫聯合，而提高內部部署資料庫的容量。在此情況下，如果您的內部部署資料庫用盡儲存空間，資料虛擬化是很棒的選項。使用聯合來提高資料庫的容量，適用於新的開發，因為開發人員不需要變更已經在正式作業中的資料庫。您也可以在兩個 {{site.data.keyword.dashdbshort_notm}} 資料庫之間聯合，將資料庫容量提高到超過「彈性」方案的現行限制。

<!-- By using federation, users can increase capacity of an on premises database by federating to or from the cloud. This is a great option if your on premises database is running out of storage. Increased capacity will also be useful for new development as our users no longer need to change a database in production. You can also use this feature to federate between two Db2 on Cloud databases to increase the capacity beyond the current limits of the Flex plan. -->

## 開始使用
{: #getting_started}

下列步驟是您如何聯合不同資料來源，使得資料彷彿從單一來源擷取的範例。下列範例說明兩個 {{site.data.keyword.dashdbshort_notm}} 資料庫的聯合：

### 在 Db2 Warehouse on Cloud 目標機器上

主機名稱：targetdotcom

1. 在綱目 `admin2` 中建立表格 `testdata`。

2. 從 {{site.data.keyword.dashdbshort_notm}} 主控台，以使用者 `admin2` 的身份、密碼 `YYYY`，在 `testdata` 表格載入資料。

### 在用來作為聯合來源的 Db2 Warehouse on Cloud 機器上

從 {{site.data.keyword.dashdbshort_notm}} 主控台：

<!-- 1. Catalog the target machine:<br/>
   `db2 catalog tcpip node <node_name> remote <host_name> server 50000`<br/>

   For example:<br/>
   `db2 catalog tcpip node fedS remote targetdotcom server 50000`

2. Catalog the database on fedS:<br/>
   `db2 catalog db bludb as <db_name> at node <node_name>`

   For example:<br/>
   `db2 catalog db bludb as srcdb at node fedS`

3. Connect to the database on fedS:<br/>
   `db2 connect to <catalog_db_name> user <admin_user> using '<admin_password>'`

   For example:<br/>
   `db2 connect to srcdb user 'admin1' with password 'XXXX'`

4. Create a wrapper on fedS:<br/>
   `db2 "create wrapper drda"` -->

1. 建立伺服器以與目標機器通訊：<br/>
   `create server <server_name> type dashdb version 11 wrapper drda authorization "<admin_user_on_target>" password "<admin_password_on_target>" options (host '<target_host_name>', port '50000', dbname 'bludb')`

   例如：<br/>
   `create server db2server type dashdb version 11 wrapper drda authorization "admin2" password "YYYY" options (host 'targetdotcom', port '50000', dbname 'bludb')`

2. 建立 admin2 的使用者對映：<br/>
   `create user mapping for <admin_user> server db2server options (remote_authid '<admin_user_on_target>', remote_password '<admin_password_on_target>')`

   例如：<br/>
   `create user mapping for admin1 server db2server options (remote_authid 'admin2', remote_password 'YYYY')`

3. 建立資料庫的暱稱：<br/>
   `create nickname <nickname> for <server_name>.<schema_name>.<table_name>`

   例如：<br/>
   `create nickname ntest1 for db2server.admin2.testdata`

4. 測試您可以從目標伺服器取回資料：<br/>
   `select * from <nickname>`

   例如：<br/>
   `select * from ntest1`

## 其他資訊

如需資料虛擬化（聯合）的相關資訊，請參閱 [Federation ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SS6NHC/com.ibm.swg.im.dashdb.doc/fcontainer.html){:new_window}。

如需聯合所支援的資料來源相關資訊，請參閱 [Federation Supported Data Sources ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/docview.wss?uid=swg27050561){:new_window}。

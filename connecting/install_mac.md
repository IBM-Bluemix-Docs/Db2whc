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

# Installing the driver package on Mac OS X

You can install the {{site.data.keyword.dashdbshort_notm}} driver package on Mac OS X by using the `installDSDriver.sh` script. 
{: shortdesc}

## Prerequisites

Before attempting to connect to your {{site.data.keyword.dashdbshort_notm}} database, verify that you have the necessary [prerequisites](connecting.html#prereqs).

<!-- Download the Db2 driver package for your operating system from the web console and install it. -->

## Procedure

- **For a new installation**

  1. Mount the disk image by double-clicking the `macos_dsdriver.dmg` file.
   
     A new **Finder** window opens with the contents of the disk image.

     If the **Finder** window does not open, double-click the `macos_dsdriver` icon on your desktop.
  2. In the **Finder** window, double-click the `installDSDriver.sh` file.

     The driver package is installed in the default location: `/Applications/dsdriver`.

- **For updates to your existing driver package installation**

  1. Back up current configuration files:

     a. Go the `Applications/dsdriver/cfg` folder.

     b. Copy the following files to a different folder: 
    
        `db2cli.ini`

        `db2dsdriver.cfg`
  2. Remove the currently installed driver package by right-clicking the `dsdriver` folder and selecting **Move to Trash**.
  3. Install the new driver package as described in the preceding **For a new installation** section:
     
     a. Mount the disk image by double-clicking the `macos_dsdriver.dmg` file.
     b. In the **Finder** window, double-click the `installDSDriver.sh` file.
  4. Restore the configuration files:

     Copy the `db2cli.ini` and `db2dsdriver.cfg` files that you saved from Step 1 to the `/Applications/dsdriver/cfg` folder.

## What's next?

To be able to connect your local applications or client tools to your {{site.data.keyword.dashdbshort_notm}} database, [configure your local environment](driver_pkg_cfg.html).

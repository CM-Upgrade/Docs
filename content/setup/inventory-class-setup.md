---
title: "Prepare Your Environment"
date: 2026-04-30
draft: false
weight: 1
description: "Prepare your Configuration Manager environment for UpgradeMate by extending the hardware inventory schema with the CM_UpgradeMate class — a required, one-time step per site."
---

Before you install UpgradeMate, prepare your Configuration Manager environment so it can collect the data UpgradeMate relies on. You will extend the hardware inventory schema with the **CM_UpgradeMate** class — a **required** step for both UpgradeMate's installation and its ongoing operation.

<div class="brand-callout">
  <span class="brand-callout-icon">💡</span>
  <div class="brand-callout-text">Run this once per Configuration Manager site. You do not need to repeat it for additional UpgradeMate installations on the same site.</div>
</div>

The process has two parts: extending the client hardware inventory schema, and importing the class into Client Settings so the data becomes available for reporting and queries.

> **Note:** The procedure uses Microsoft's standard mechanism for extending hardware inventory. For background, see [Extend hardware inventory in Configuration Manager](https://learn.microsoft.com/en-us/intune/configmgr/core/clients/manage/inventory/extend-hardware-inventory) on Microsoft Learn.

### Video Tutorial

{{< youtube id="abEOLPQqpvo" >}}

---

### Requirements

* The Configuration Manager console installed on the machine where you perform these steps.
* Write access to the SCCM site server's installation directory (specifically the `inboxes\clifiles.src\hinv` folder).
* Membership in a role that allows modification of **Default Client Settings** — typically the **Full Administrator** role.
* The UpgradeMate MOF files (`Configuration.mof` and `SMS_DEF.mof`) — by default located at `C:\Program Files\UpgradeMate\MOF\`.

---

### Step-by-Step Guide

#### Extend Hardware Inventory

This part adds the `CM_UpgradeMate` class definition to your site's `configuration.mof` file so that clients begin reporting UpgradeMate data.

1. Locate the source `Configuration.mof` file provided by UpgradeMate. By default it is found in `C:\Program Files\UpgradeMate\MOF\`.

   ![UpgradeMate MOF directory](/images/inventory-class-setup/01-mof-source-directory.png)

2. Open this `Configuration.mof` file with a text editor like Notepad and copy its entire content to your clipboard.

   ![Contents of UpgradeMate's Configuration.mof file](/images/inventory-class-setup/02-mof-content.png)

3. To find the destination `configuration.mof` file, you first need to identify your Configuration Manager installation directory. In the console, navigate to **Administration → Site Configuration → Sites**. Select your site, and the **Install Directory** is listed in the details pane.

   ![Finding the ConfigMgr install directory](/images/inventory-class-setup/03-find-install-directory.png)

4. Navigate to the following path within your ConfigMgr installation directory: `[Install Directory]\inboxes\clifiles.src\hinv`. Open the `configuration.mof` file in this location with a text editor.

   ![ConfigMgr's main configuration.mof file before editing](/images/inventory-class-setup/04-configmgr-mof-before.png)

   > **Warning:** Back up your `configuration.mof` file before making any modifications.

5. Scroll to the bottom of the file. Add the content you copied from the UpgradeMate MOF file between the `// Added extensions start` and `// Added extensions end` comments. If these comments do not exist, add the content at the bottom of the file. Save and close the file.

   ![ConfigMgr's main configuration.mof file after adding the extension](/images/inventory-class-setup/05-configmgr-mof-after.png)

#### Import Reporting Class

Now that the hardware inventory schema is extended, this part imports the **CM_UpgradeMate** class into Client Settings so the collected data is visible in the ConfigMgr database for reporting and queries. You will need the `SMS_DEF.mof` file, by default located at `C:\Program Files\UpgradeMate\MOF\SMS_DEF.mof`.

1. In the Configuration Manager console, go to the **Administration** workspace, then navigate to **Site Configuration → Client Settings**. Select **Default Client Settings** in the list and click **Properties** from the ribbon on the Home tab.

   ![Client Settings](/images/inventory-class-setup/06-client-settings.png)

2. In the **Default Settings** dialog, choose **Hardware Inventory** from the left-hand menu. Then, in the **Device Settings** list, click the **Set Classes...** button.

   ![Hardware Inventory – Set Classes](/images/inventory-class-setup/07-hardware-inventory-set-classes.png)

3. The **Hardware Inventory Classes** dialog opens. Click the **Import...** button to add a new class.

   ![Hardware Inventory Classes – Import](/images/inventory-class-setup/08-hardware-inventory-classes-import.png)

4. In the file selection dialog, browse to and select the `SMS_DEF.mof` file provided by UpgradeMate, then click **Open**.

   ![Import MOF file](/images/inventory-class-setup/09-import-mof-file.png)

5. On the **Import Summary** screen, you will see the **CM_UpgradeMate** class that will be added. Review the items, then click **Import**.

   ![Import summary](/images/inventory-class-setup/10-import-summary.png)

6. Once the import completes, you are returned to the **Hardware Inventory Classes** dialog. Verify that the newly added **CM_UpgradeMate (CM_UpgradeMate)** class is present and checked (enabled). Click **OK** to save the changes.

   ![Verify the imported class](/images/inventory-class-setup/11-verify-imported-class.png)

7. Finally, click **OK** in the main **Default Settings** window to finalize the process and apply the new settings to your clients.

Configuration Manager will now begin collecting UpgradeMate data from clients, making it available for reporting.

---

### Related Links
* [Initial Configuration]({{< ref "initial-configuration.md" >}})
* [Power BI Dashboard Setup]({{< ref "powerbi-dashboard-setup.md" >}})

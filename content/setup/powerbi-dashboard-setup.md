---
title: "Power BI Dashboard Setup"
date: 2026-04-30
draft: false
weight: 3
description: "How to import the UpgradeMate Power BI template, publish it to your Power BI Report Server, and configure a scheduled refresh for live reporting."
---

This guide walks you through importing the **UpgradeMate Power BI template**, publishing it to your Power BI Report Server, and configuring a scheduled data refresh. Following these steps enables dynamic, up-to-date reporting on your device upgrade readiness.

### Video Tutorial

{{< youtube id="_2uBTZ8lWDg" >}}

---

### Requirements

* **Power BI Desktop (Report Server optimized)** installed on the machine where you perform the import.
* The UpgradeMate Power BI template file `TemplateUpgradeMate.pbit` — by default located at `C:\Program Files\UpgradeMate\PowerBI\TemplateUpgradeMate.pbit`.
* Access to your **Power BI Report Server**.
* Credentials with read access to the Configuration Manager database.

---

### Step-by-Step Guide

#### Import the Template in Power BI Desktop

1. In Power BI Desktop, navigate to **File → Import** and select **Power BI template**.

   ![Import Power BI template](/images/powerbi-dashboard-setup/01-import-template.png)

2. Browse to the location of the UpgradeMate template file, select `TemplateUpgradeMate.pbit`, and click **Open**.

   ![Select template file](/images/powerbi-dashboard-setup/02-select-template-file.png)

3. Enter the required parameters for your environment, including the **CMDBServer**, **CMDBName**, and other relevant details. Click **Load** to continue.

   ![Enter template parameters](/images/powerbi-dashboard-setup/03-template-parameters.png)

4. When prompted for database credentials, select your preferred method (e.g. **Use my current credentials**) and click **Connect** to load data from the SQL Server database.

   ![SQL Server database credentials](/images/powerbi-dashboard-setup/04-sql-credentials.png)

#### Publish to Power BI Report Server

1. Once the report has loaded, save it to your report server. Go to **File → Save as** and choose **Power BI Report Server**.

   ![Save to Power BI Report Server](/images/powerbi-dashboard-setup/05-save-to-report-server.png)

2. Select or enter the address for your Power BI Report Server (e.g. `http://CMDB01/Reports`) and click **OK**.

   ![Select report server address](/images/powerbi-dashboard-setup/06-report-server-address.png)

3. Provide a name for your new report, such as `UpgradeMate Dashboard`, and click **OK** to publish it.

   ![Name the report](/images/powerbi-dashboard-setup/07-name-report.png)

4. A success message confirms that the report has been saved. Click **Take me there** to open the report server web portal.

   ![Save successful](/images/powerbi-dashboard-setup/08-save-successful.png)

#### Configure Scheduled Refresh

1. In the Power BI Report Server web portal, find your newly published report, click the ellipsis (**...**) for more options, and select **Manage**.

   ![Manage report](/images/powerbi-dashboard-setup/09-manage-report.png)

2. Navigate to the **Data sources** tab. To allow the server to refresh the data automatically, you must store credentials. Enter the username and password for an account with access to the database and click **Test connection**. If successful, click **Save**.

   ![Configure data sources](/images/powerbi-dashboard-setup/10-configure-data-sources.png)

3. Select the **Scheduled refresh** tab from the left menu and then click **+ New scheduled refresh plan**.

   ![Scheduled refresh](/images/powerbi-dashboard-setup/11-scheduled-refresh.png)

4. Choose **Report-specific schedule** and click the **Edit schedule** button to define the refresh frequency.

   ![Edit schedule](/images/powerbi-dashboard-setup/12-edit-schedule.png)

5. Configure the schedule details. For example, you can set the report to refresh every **15 minutes**. Set the start date and time, then click **Apply**.

   ![Configure schedule details](/images/powerbi-dashboard-setup/13-configure-schedule-details.png)

6. Review the configured schedule and click **Create scheduled refresh plan** to finalize the setup.

   ![Create scheduled refresh plan](/images/powerbi-dashboard-setup/14-create-refresh-plan.png)

7. The scheduled refresh plan is now active. The report data will automatically update based on the schedule you defined, ensuring your dashboard always displays the latest information.

   ![Active refresh plan](/images/powerbi-dashboard-setup/15-active-refresh-plan.png)

Setup complete. Your UpgradeMate Power BI report is now live and will automatically stay up-to-date.

---

### Related Links
* [Prepare Your Environment]({{< ref "inventory-class-setup.md" >}})
* [Initial Configuration]({{< ref "initial-configuration.md" >}})

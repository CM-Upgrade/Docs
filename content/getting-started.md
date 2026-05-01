---
title: "Getting Started"
date: 2026-05-01
draft: false
weight: 1
description: "Receive your UpgradeMate license, install the product on your SCCM Primary Site Server, and launch the configuration wizard."
next: /setup/
prev: /
---

Once your purchase or demo request is processed, UpgradeMate emails you the installer and your license file. This page walks you through installing UpgradeMate and launching the Configuration wizard.

---

### Requirements

* The computer where you run the UpgradeMate setup must be able to reach the **SMS Provider** and the **source share directory**.
* The user account running the setup must be a member of the **Administrator** role on the SCCM site.
* The setup must be launched with **Run as Administrator**.

---

### Step-by-Step Guide

#### Receive Your License Email

The email contains your license details and direct download links for both the setup file and the license file.

1. Review **License ID**, **Plan**, **Company**, **SCCM Site Code**, **Device Count**, **Target Windows** version, and **Expires** date in the email to confirm everything matches your purchase.

2. Click **Download UpgradeMate Setup** to download `UpgradeMateSetup.zip`, then click **Download License File (data.bin)** to download your license.

   > **Note:** The download links remain valid for **30 days**. Your license is valid for **one year** from activation for purchased plans, or **10 days** for demo licenses. Keep this email — you will need it again if you re-deploy or renew.

#### Run the Installer

1. Extract `UpgradeMateSetup.zip` to a local folder. Inside you will find `UpgradeMateSetup-vXXXX.exe`.

2. Run `UpgradeMateSetup-vXXXX.exe` as **Administrator**.

3. Complete the installation by clicking **Next** with the default settings on every screen.

4. On the final screen, the **Configure UpgradeMate** checkbox is selected by default. Click **Finish** with it checked to launch the [Initial Configuration]({{< ref "setup/initial-configuration.md" >}}) wizard right away. If you clear the checkbox, you can start it later by running the **UpgradeMate** shortcut from the Start menu.

---

### Need help?

Reply to your activation email or reach the UpgradeMate team at `info@upgrademate.io`.

---

### Related Links
* [Initial Configuration]({{< ref "setup/initial-configuration.md" >}})
* [Inventory Class Setup]({{< ref "setup/inventory-class-setup.md" >}})
* [Power BI Dashboard Setup]({{< ref "setup/powerbi-dashboard-setup.md" >}})

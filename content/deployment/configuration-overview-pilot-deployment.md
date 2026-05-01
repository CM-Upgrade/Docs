---
title: "Configuration Overview & Pilot Deployment"
date: 2026-04-30
draft: false
weight: 1
description: "Tour of the objects UpgradeMate creates in Configuration Manager, the deployment workflow your devices follow, and best practices for a successful pilot deployment."
---

After you complete the UpgradeMate setup wizard, several objects are created in your Configuration Manager environment. This guide walks you through the items that were created, explains the deployment workflow your devices follow, and highlights the steps you should take for a successful pilot deployment.

### Video Tutorial

{{< youtube id="kBKFlBOw57c" >}}

---

### Requirements

* The UpgradeMate [Initial Configuration]({{< ref "initial-configuration.md" >}}) wizard must have been completed successfully.

---

### Step-by-Step Guide

#### Objects Created in Configuration Manager

The wizard provisions five categories of objects across the **Software Library**, **Assets and Compliance**, and **Administration** workspaces. Reviewing them before adding any devices helps you understand what each part does.

1. **Collections** — UpgradeMate creates a folder of **12 device collections** under **Assets and Compliance → Device Collections**. Every step of the upgrade workflow is driven by membership in these collections.

   ![The 12 UpgradeMate collections under one folder in Configuration Manager](/images/configuration-overview-pilot-deployment/01-collections.jpg)

2. **Task Sequences** — **Two task sequences** are created under **Software Library → Operating Systems → Task Sequences**: one for the scan stage and one for executing the upgrade. Each task sequence already has its own deployment configured to the appropriate collection.

   ![The two UpgradeMate task sequences in Software Library](/images/configuration-overview-pilot-deployment/02-task-sequences.jpg)

3. **OS Upgrade Package(s)** — For every Windows ISO you imported during setup, an **OS Upgrade Package** is created under **Software Library → Operating Systems → Operating System Upgrade Packages**.

   ![UpgradeMate OS Upgrade Package created from an imported ISO](/images/configuration-overview-pilot-deployment/03-os-upgrade-package.jpg)

4. **Form Package** — The `UpgradeMateForm` package is created under **Software Library → Application Management → Packages**. The form has its own deployment that controls when it is shown to end-users.

   ![UpgradeMateForm package and its deployment in Application Management](/images/configuration-overview-pilot-deployment/04-form-package.jpg)

5. **Client Settings** — A custom client setting (`CCM Cache Setting for (_UpgradeMate - Windows 11 XXHX)`) is created under **Administration → Client Settings**. It increases the Configuration Manager client cache so the upgrade content can be downloaded without filling the default cache.

   ![UpgradeMate CCM cache client setting in Administration](/images/configuration-overview-pilot-deployment/05-client-settings.jpg)

#### Deployment Workflow & Schedules

Devices move through the UpgradeMate collections in a defined order. Two scheduled deployments — the scan and the form — drive the workflow forward.

1. Eligible devices are listed automatically in **01.Candidates** with version control. If there are devices in this list that you do *not* want to upgrade, add them to the **Exclude from Upgrade** collection so they are excluded from both deployment and reporting.

2. To start the upgrade workflow for a device, add it to **02.Scan PreCache Content**.

   > **Key point:** From this point on UpgradeMate handles the rest of the flow automatically. The only collection you ever need to populate to trigger an upgrade is `02.Scan PreCache Content`.

3. The scan stage runs first. A deployment of the **Scan** task sequence is targeted at **02.Scan PreCache Content** on a daily schedule (default: **10:00 AM**). If a scan succeeds, it is not retried. Leave the **Run behavior** setting at its default.

   ![Scan deployment schedule set to daily at 10:00 AM](/images/configuration-overview-pilot-deployment/06-scan-schedule.jpg)

   > **Tip:** You can change the time, add additional schedules, or both. Open the deployment and edit the schedule to fit when your devices are typically online and reachable.

4. Devices that pass the scan move to **04.Execute Upgrade: Available**. The **UpgradeMateForm** deployment targets this collection and controls how often the form is presented to end-users. By default the form is offered every day at **12:00 PM** (noon). Leave the **Run behavior** setting at its default.

   ![Form deployment schedule set to daily at 12:00 PM](/images/configuration-overview-pilot-deployment/07-form-deployment-schedule.jpg)

   > **Tip:** You can change this time or add additional schedules. Open the deployment and edit the schedule to match when you want the upgrade form to appear to your users.

5. Devices that fail the scan move to a separate collection. After **10 failed attempts** the device stops retrying.

6. Devices whose scan succeeds are added automatically to the **04.Execute Upgrade: Available - Form** collection, and the upgrade form starts appearing to the end-user.

7. After the upgrade runs, devices land in **05.Execute Upgrade: Success** or **05.Execute Upgrade: Failed**.

   > **Note:** Reporting is not done from these collections. Use the [UpgradeMate Power BI dashboard]({{< ref "powerbi-dashboard-setup.md" >}}) for outcomes — it shows the failure reason, the failed step, and hardware/software details for each device.

> **Optional — Expired path:** If a device is not upgraded within the *Days to Force Upgrade* period set in the wizard, it moves from **04.Execute Upgrade: Available** to **04.Execute Upgrade: Expired**. From this point the user only sees the **Install Now** option in the form — the Schedule and Postpone options are hidden.

> **Optional — Forced install:** The **04.Execute Upgrade: Required** collection lets you force the upgrade without user interaction. Create a new **Required** deployment of the **Execute Upgrade** task sequence and target it at this collection. The deployment then runs on the schedule you define.

#### Pilot Deployment Best Practices

Always run a pilot before letting UpgradeMate upgrade your full candidate list. The pilot lets you validate scan and install behaviour against the variety of devices in your environment.

1. For the pilot, add devices that represent different **models**, **operating system versions**, and **installed software** to **02.Scan PreCache Content**. This pre-caches the upgrade content and triggers the scan workflow on a representative sample.

2. Once the pilot meets your target success rate, add the remaining candidates to **02.Scan PreCache Content** in bulk. The deployment continues automatically based on the existing schedules.

3. Remember: the only collection you need to populate to start and trigger an upgrade is **02.Scan PreCache Content**. Everything else happens through the existing collection memberships and deployments.

4. Distribute the UpgradeMate content to the right Distribution Points. If you imported a new ISO during setup, the new **OS Upgrade Package(s)** and the **UpgradeMateForm** package must be distributed to every DP that your candidate devices will reach for content download.

   > **Reminder:** Devices cannot scan or upgrade until the required content is available on a DP they can reach. Verify the content state on each DP before adding devices to `02.Scan PreCache Content`.

You are ready to run the pilot. Add a small set of representative devices to `02.Scan PreCache Content` and follow the workflow on the [Power BI dashboard]({{< ref "powerbi-dashboard-setup.md" >}}).

---

### Related Links
* [Initial Configuration]({{< ref "initial-configuration.md" >}})
* [Inventory Class Setup]({{< ref "inventory-class-setup.md" >}})
* [Power BI Dashboard Setup]({{< ref "powerbi-dashboard-setup.md" >}})

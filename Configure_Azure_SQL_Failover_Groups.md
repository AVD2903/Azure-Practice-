# Configure Azure SQL with Failover Groups

## Overview
Azure SQL includes several features that help protect against outages. Auto-failover groups provide this protection with built-in management capabilities to simplify failover.

In this hands-on lab, we'll use the Azure portal to configure an auto-failover group for an existing Azure SQL server.

---

## Scenario
You are a cloud administrator tasked with improving business continuity for Azure SQL services after a recent audit. The goals are:
- Configure replication to improve high availability.
- Ensure prompt recovery in the event of a region-wide disaster.
- Automate failover and include DNS management.

---

## Objectives
- Create a new Azure SQL logical server in a different region from the existing server.
- Configure an Azure SQL auto-failover group between the two servers.
- Include the pre-built database `testDatabase1` in the failover group.

---

## Steps

### 1. Log in to Azure Portal
Use the credentials provided on the lab page.

### 2. Create an Azure SQL Logical Server
- Copy the name of the existing SQL server.
- Click **Create** and search for **Azure SQL**.
- Select **Azure SQL** > **Create**.
- In the **SQL databases** card, choose **Database server**.
- Click **Create**.
- Fill in Project details:
  - **Server name**: Modify the existing server name to make it unique.
  - **Location**: Select a different region (e.g., East US).
  - **Authentication Method**: Use SQL authentication.
  - **Server admin login**: e.g., `sql-adm`.
  - **Password**: Choose a strong password.
- Under **Additional Settings**, set **Enable Azure Defender for SQL** to **Not now**.
- Click **Review + create** > **Create**.

### 3. Configure Auto-Failover Groups
- Navigate back to **Home** > **All resources**.
- Select the original SQL server.
- From the left menu, under **Data management**, select **Failover groups**.
- Click **+ Add group**.
- Set **Failover group name** (e.g., `<original-server>-fog`).
- Select the newly created server in a different region as the failover server.
- Under **Database within the group**, click **Configure database** and select `testDatabase1`.
- Click **Select** > **Create**.
- Review the failover group details.

---

## Outcome
You have successfully configured an Azure SQL auto-failover group to ensure high availability and disaster recovery.

---

## Lab Credentials
- **URL**: https://portal.azure.com/#@realhandsonlabs.com/resource/subscriptions/80ea84e8-afce-4851-928a-9e2219724c69/resourceGroups/487-1f29d478-configure-azure-sql-with-failover-gro/overview
- **Username**: cloud_user_p_07e0f132@realhandsonlabs.com
- **Password**: B*K^jLvB

---

## Notes
- Open the lab in an incognito window for best results.

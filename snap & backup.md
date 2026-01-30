
# Azure Lab Runbook – VM Creation → Snapshot → Enhanced Backup → Decommissioning

This repository documents the end‑to‑end workflow performed in an Azure lab environment.

## Overview of Tasks
1. Create a Linux Ubuntu VM
2. Take a full disk snapshot
3. Start Enhanced Backup
4. Stop Enhanced Backup
5. Decommission the VM and remove associated resources

## 1. Create an Ubuntu Linux VM
1. Log in to the Azure Portal.
2. Navigate to Virtual Machines.
3. Create a new VM using Ubuntu Server.
4. Choose VM size, authentication method, and networking options.
5. Click Review + Create → Create.

## 2. Take a Full Disk Snapshot
1. Go to the VM → Disks.
2. Select the OS Disk → Create Snapshot.
3. Choose Full snapshot type and create it.

## 3. Start Enhanced Backup
1. Open Recovery Services Vault.
2. Select Backup → Azure Virtual Machine.
3. Choose the VM and apply an Enhanced Backup policy.
4. Enable Backup.

## 4. Stop Enhanced Backup
1. Open Recovery Services Vault.
2. Navigate to Backup Items → Azure VM.
3. Select your VM → Stop Backup.
4. Choose whether to retain backup data.

## 5. Decommission the VM and Delete All Resources
1. Go to Virtual Machines.
2. Select your VM → Delete.
3. Select deletion of OS disk, NIC, public IP, and other resources.
4. Confirm deletion.

## Summary
- Ubuntu VM created
- Full snapshot taken
- Enhanced Backup started and stopped
- VM and resources decommissioned

# Linux Storage Upgrade & MySQL Log Optimization Guide

This guide details the steps to expand storage on an AWS EC2 Ubuntu instance and resolve disk space issues caused by accumulating MySQL binary logs (`binlog`).

---

## Part 1: Upgrading Disk Space (AWS EC2 & Ubuntu)

### Step 1: Modify Volume in AWS Console
1. Log into the **AWS Management Console**.
2. Navigate to **EC2 Dashboard** > **Instances** and select your instance.
3. Click the **Storage** tab at the bottom, and click on your **Volume ID** (e.g., `vol-xxxxxx`).
4. Select the volume, click **Actions** > **Modify Volume**.
5. Increase the size (e.g., change from `28 GiB` to `100 GiB`) and click **Modify**.

### Step 2: Extend the Partition in Linux
Once AWS updates the volume configuration in the background, access your terminal and run the following commands:

1. **Verify the new raw disk size is visible:**
   ```bash
   sudo lsblk
   ```
   *You should see the main disk device (e.g., `nvme0n1`) reflects the new size (e.g., `100G`), but the root partition (`nvme0n1p1`) is still small.*

2. **Expand the partition:**
   Grow partition `1` on device `/dev/nvme0n1` *(note the space between the disk path and partition number)*:
   ```bash
   sudo growpart /dev/nvme0n1 1
   ```

### Step 3: Resize the Filesystem
Tell the Linux filesystem to stretch out and occupy the newly expanded partition space.

* **For EXT4 filesystems (Standard Ubuntu default):**
  ```bash
  sudo resize2fs /dev/root
  ```
* **For XFS filesystems (Alternative):**
  ```bash
  sudo xfs_growfs /
  ```

### Step 4: Verify the New Storage Layout
```bash
df -h /
```
The total available storage capacity will now match your newly upgraded limit.

---

## Part 2: Fixing Bloated MySQL Binary Logs (`binlog`)

MySQL records every data modification transaction into binary log files (`binlog.00000X`). On high-traffic systems, these files can rapidly stack up and consume gigabytes of storage space.

### Step 1: Instantly Purge Old Logs (Safe Method)
*Do not delete these files manually using standard Linux `rm` commands, as it will corrupt the MySQL tracking index.*

1. Log into your database server engine via the command line interface:
   ```bash
   sudo mysql -u root -p
   ```
2. Execute the purge query to clear files up to your latest active log file (e.g., `binlog.000063`):
   ```sql
   PURGE BINARY LOGS TO 'binlog.000063';
   ```
3. Exit the database client prompt:
   ```sql
   exit;
   ```

### Step 2: Configure Automatic Expiration Rules
To prevent logs from accumulating indefinitely again, establish an automated data-retention threshold window.

1. Open your native server engine configuration profile:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
2. Scroll to the bottom of the file under the active `[mysqld]` context group block, and append the following property allocation rule to keep logs for exactly **3 days** (259,200 seconds):
   ```ini
   binlog_expire_logs_seconds = 259200
   ```
   *(Note: For older legacy database versions, use `expire_logs_days = 3` instead).*
3. Save changes and close the editor (`Ctrl + O`, `Enter`, then `Ctrl + X`).

### Step 3: Refresh System Configurations and Apply
Reload the system daemon management configs to authorize file modifications, then securely restart the database backend process:

```bash
sudo systemctl daemon-reload
sudo systemctl restart mysql
```

### Step 4: Confirm Service Health
Verify that the service configuration started up properly with no syntax errors:
```bash
sudo systemctl status mysql
```
*Look for the green `Active: active (running)` diagnostic message block.*

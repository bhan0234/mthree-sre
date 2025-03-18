
# Jenkins Backup and Restore Guide


## 1. Introduction
Having reliable backups of your Jenkins controller is crucial for:
- Disaster recovery
- Restoring older configurations
- Recovering lost or corrupted files


## 2. Creating a Backup
### Filesystem Snapshots
Filesystem snapshots provide high consistency and are faster than live backups. Supported by:
- Linux LVM
- Linux btrfs
- Solaris ZFS
- FreeBSD ZFS
- OpenZFS on Linux
- Cloud providers and storage devices

### Plugins for Backup
Jenkins offers backup plugins:
- Navigate to **Manage Jenkins** > **Plugins** > **Available**
- Search for "backup"
- Use "thinBackup Plugin" (actively maintained)

### Writing a Shell Script for Backups
A shell script can automate backups:
1. Create a backup directory (e.g., `/mnt/backup`)
2. Use cron to schedule periodic backups
3. Store backups on a separate filesystem or remote storage
4. Include timestamps to prevent overwriting

## 3. Backing Up the Controller Key Separately
- **DO NOT** include the controller key in backups.
- It is located in `$JENKINS_HOME/secrets/hudson.util.Secret`.
- It encrypts sensitive data; store it securely and separately.
- The `master.key` file should be backed up separately for full restoration.

## 4. Files to Back Up
### $JENKINS_HOME
Backing up the full `$JENKINS_HOME` ensures complete recovery.

### Configuration Files
- Stored in `$JENKINS_HOME/*.xml`
- Main file: `config.xml`
- Can be stored in an SCM repository

### ./jobs Subdirectory
- Stores job-related data
- **./builds/** - Contains build records
- **./builds/archive/** - Stores archived artifacts (can be large)
- **./workspace/** - Contains checked-out files (can be excluded)
- **./plugins/*.hpi** and **./plugins/*.jpi** - Plugin packages

## 5. Files That May Not Need Backup
Some files can be re-downloaded, reducing backup size:
- **./war** - Download the latest Jenkins WAR file.
- **./cache** - Contains downloaded tools.
- **./tools** - Can be re-extracted.
- **./plugins/xxx** - Auto-populated on restart.

## 6. Validating a Backup
Ensure the backup is valid before relying on it:
1. Restore the backup to a test location (`/mnt/backup-test`).
2. Set Jenkins home: `export JENKINS_HOME=/mnt/backup-test`.
3. Start Jenkins: `java -jar jenkins.war --httpPort=9999`.

## 7. Summary
- Use filesystem snapshots or shell scripts for backups.
- Keep the controller key separate for security.
- Prioritize configuration files, jobs, and plugins in backups.
- Validate backups regularly to ensure recovery success.

## 8. Configuring Jenkins Home
To configure the Jenkins home directory:
- Default: `/var/lib/jenkins`
- Change it by modifying the environment variable:
  ```bash
  export JENKINS_HOME=/new/path/to/jenkins_home
  ```
- Ensure permissions are correctly set:
  ```bash
  chown -R jenkins:jenkins /new/path/to/jenkins_home
  ```
- Restart Jenkins after modification:
  ```bash
  systemctl restart jenkins
  ```

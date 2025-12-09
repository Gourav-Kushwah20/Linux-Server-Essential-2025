# 🔄 Rsync Commands

`rsync` is a fast, reliable tool for copying and synchronizing files and directories between local and remote systems.
It uses SSH for secure transfers and supports options for incremental syncing, compression, exclusion, bandwidth limits, and more.

---

## 📦 Install rsync

```bash
apt install rsync
```

```bash
yum install rsync
```

---

## 📥 Sync Remote Directory to Local

Copy files or directories **from a remote server to your local machine**:

```bash
rsync -av root@192.168.1.33:/var/log /tmp/
```

```bash
rsync -az root@192.168.1.33:/var/log /tmp/
```

---

## 📤 Sync Local Directory to Remote

Upload local data **to a remote machine**:

```bash
rsync -av /tmp/log root@192.168.1.33:/var/
```

```bash
rsync -az /tmp/log root@192.168.1.33:/var/
```

### Options:

* **-a** : Archive mode (preserves permissions, timestamps, etc.)
* **-v** : Verbose (shows detailed progress)
* **-z** : Compress data during transfer (faster over slow connections)

---

## 🔐 Use Custom SSH Key

Specify a private key to connect securely:

```bash
rsync -av -e 'ssh -i ~/.ssh/id_rsa' root@192.168.1.33:/var/log /tmp/log
```

---
## ⚙️ Specify SSH Port

Connect through a non-default SSH port:

```bash
rsync -av -e 'ssh -p 2200' /backup root@192.168.1.100:/mnt/backup
```

---

## 🧪 Perform a Dry Run

Preview changes **without making them**:

```bash
rsync -av --dry-run root@192.168.1.33:/var/log /tmp/
```

### ℹ️ `--dry-run`

Simulates the transfer, allowing you to preview what will happen **without applying any changes**.

---

## 🗑️ Delete Extra Files on Remote

Mirror the local directory exactly:

```bash
rsync -av --delete root@192.168.1.33:/var/log /tmp/
```

### Explanation

* **`--delete`**: Removes files in the destination directory (`/tmp/`) that **do not exist** in the source directory (`root@192.168.1.33:/var/log`).
* **`root@192.168.1.33:/var/log`**: Source directory on remote machine.
* **`/tmp/`**: Destination directory on local machine where files will be synchronized.

---

## 🔍 Show Transfer Progress

```bash
rsync -av --progress largefile.iso root@192.168.1.33:/iso/
```

### Explanation

* **`--progress`**: Shows transfer progress, including speed, % completion, and estimated time remaining.
* **`largefile.iso`**: File being transferred from your local machine.
* **`root@192.168.1.33:/iso/`**: Destination directory on the remote system.

---

## 🎯 Sync Only Specific File Types

```bash
rsync -av --include='*.jpg' --exclude='*' /photos/ root@192.168.1.33:/backup/images/
```

### Explanation

* **`--include='*.jpg'`** → Only include files ending in `.jpg`.
* **`--exclude='*'`** → Exclude everything except what matches the include rule.
* **`/photos/`** → Local source directory.
* **`root@192.168.1.33:/backup/images/`** → Remote destination directory.

---

## 🚫 Exclude Files or Directories

```bash
rsync -av --exclude 'node_modules' ~/project root@192.168.1.33:/deploy/
```

```bash
rsync -av --exclude '*.log' --exclude 'tmp/' /var/www/ root@192.168.1.33:/backup/
```

### Common Exclusions

* `node_modules` (large dependency directories)
* `*.log` (log files)
* `tmp/` (temporary files)

---

## 🔗 Preserve Symlinks

```bash
rsync -av -l /var/www/ root@192.168.1.33:/www/
```

* **🔧 -l :** This option preserves symbolic links (i.e., copies the symlink itself rather than the file it points to). It ensures that symlinks are treated as symlinks during the transfer, rather than being followed or dereferenced.
* **📁 /var/www/ :** This is the source directory on your local machine.
* **📤 root@192.168.1.33:/www/ :** This is the destination directory on the remote system at **192.168.1.33**. The **root** user is used to perform the transfer.

---

## 🔁 Enable Partial Transfers

Resume interrupted transfers without restarting.

```bash
rsync -av --partial --progress bigfile.iso root@192.168.1.33:/iso/
```

* **🧩 --partial :** This option ensures that partially transferred files are kept if the transfer is interrupted. This way, if the transfer stops for any reason, you can resume from where it left off rather than starting over.
* **📊 --progress :** This option shows the progress of the transfer for large files, including the percentage completed, transfer speed, and estimated time remaining.
* **📦 bigfile.iso :** This is the file you're transferring from your local machine.
* **📥 root@192.168.1.33:/iso/ :** This is the destination directory on the remote system (**192.168.1.33**), where the file will be copied to.

---

## 📝 Create Verbose Log File

Log file transfers for auditing.

```bash
rsync -av --log-file=/var/log/rsync-backup.log /data/ root@192.168.1.33:/backup/data/
```

* **📄 --log-file=/var/log/rsync-backup.log :** This option writes the details of the file transfer to the specified log file (**/var/log/rsync-backup.log**). The log will include information like which files were transferred, skipped, or failed. This is useful for tracking the progress and results of the transfer.
* **📁 /data/ :** This is the source directory on your local machine that you are syncing from.
* **📂 root@192.168.1.33:/backup/data/ :** This is the destination directory on the remote machine (**192.168.1.33**). The files will be copied to **/backup/data/**.

---

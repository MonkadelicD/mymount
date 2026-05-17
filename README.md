***

# Dynamic Multi-Share Mount Script (Portable Version)

## 1. Overview

This script allows mounting and unmounting any number of NFS shares defined dynamically through environment variables. It enables fast switching of mount targets for home labs, workstation fleets, or shared NAS environments—without any code changes.  
This is a new version of a script I've used on my personal workstations for years. This version was made portable by utilizing environment variables instead of hard-coded values.

- Supports unlimited, user-defined share names.
- Mount or unmount multiple shares in one command.
- Fully controlled by environment, portable between systems.
- Easy integration into automation or user workflows.

***

## 2. Environment Configuration

Before running the script, set the required environment variables. You have two options:

### 2a. Config File (recommended)

The script automatically loads environment from `$XDG_CONFIG_HOME/mymount/env` (defaults to `~/.config/mymount/env`).

Create the file:

```bash
mkdir -p ~/.config/mymount
cp /path/to/mymount/env.example ~/.config/mymount/env
# edit ~/.config/mymount/env with your NAS details
```

Then run without manually sourcing anything:

```bash
sudo mymount --all
```

(The script automatically falls back to the original user's config when run under `sudo`.)

### 2b. Manual / Inline

Set variables directly or source a file of your choosing:

```bash
export MYMOUNT_NAS_ROOT="nas.example.com:/mnt/nas"

# Define each NAS share you want available
export MYMOUNT_NAS_backups="${MYMOUNT_NAS_ROOT}/backups"
export MYMOUNT_NAS_isos="${MYMOUNT_NAS_ROOT}/Storage/ISO_Store"
export MYMOUNT_NAS_shares="${MYMOUNT_NAS_ROOT}/Storage/ShareData"
# Add more as needed
# export MYMOUNT_NAS_media="${MYMOUNT_NAS_ROOT}/Media"

# (Optional) Local mount root, defaults to "$HOME/lanshares"
export MYMOUNT_LOCAL_ROOT="$HOME/lanshares"

# (Optional) Override individual local mount points
export MYMOUNT_LOCAL_backups="$MYMOUNT_LOCAL_ROOT/backups"
export MYMOUNT_LOCAL_isos="$MYMOUNT_LOCAL_ROOT/iso_store"
export MYMOUNT_LOCAL_shares="$MYMOUNT_LOCAL_ROOT/sharedata"
# export MYMOUNT_LOCAL_media="/mnt/media"

# (Optional) NFS mount options
export MYMOUNT_OPTS="tcp,vers=4,hard"
```

Place these in any file and source it:

```bash
source ~/.mymount.env
```

When using manual/inline env vars, pass them through sudo with `-E`:

```bash
sudo -E mymount --all
```

***

## 3. Installation

Copy the script to a directory in your PATH (e.g. `~/bin`) and make it executable:

```bash
sudo cp mymount /usr/local/bin/
chmod +x /usr/local/bin/mymount
```

Create a hard link for unmounting:

```bash
ln /usr/local/bin/mymount /usr/local/bin/myunmount
```

***

## 4. Usage

Mount shares:

```bash
sudo mymount -b --isos        # Mount backups and isos
sudo mymount --all            # Mount all configured shares
```

Unmount shares:

```bash
sudo myunmount -b             # Unmount backups
sudo myunmount -a             # Unmount all shares
```

> **Note:** If you're using the config file at `~/.config/mymount/env`, plain `sudo` works. If you're setting env vars manually or sourcing a custom file, use `sudo -E` instead.

All mount/unmount options are auto-generated based on your environment settings.

***

## 5. Adding/Removing Shares

To change available shares, simply edit your environment config:

- Add new shares:
  ```bash
  export MYMOUNT_NAS_project="${MYMOUNT_NAS_ROOT}/ProjectFiles"
  ```
- Remove a share by un-exporting or deleting its definition from your env/config file.

No code changes required.

***

## 6. Troubleshooting

- **Missing shares?**  
  Check your environment variable names. Each share must be set as `MYMOUNT_NAS_<share>`.

- **Mounting errors?**  
  Ensure you run as root (`sudo`) and that `mount.nfs` is installed.

- **Local mount points?**  
  If not overridden, they default to `$MYMOUNT_LOCAL_ROOT/<share>`. Override with `MYMOUNT_LOCAL_<share>` if needed.

- **Additional NFS options?**  
  Set `MYMOUNT_OPTS` to tweak mount behavior.

***

## 7. Example .env File

A working template is provided at `env.example` in the script directory. Copy to the XDG config path and edit:

```bash
cp env.example ~/.config/mymount/env
# edit ~/.config/mymount/env
```

***

## 8. License

MIT License. Use and modify at your own risk.

***

## 9. Contributions

PRs and suggestions welcome! Fork and tailor to suit your local or organizational needs.

***

## 10. Support

For questions, open an issue or reach out via your preferred channel.

---

***

# Dynamic Multi-Share Mount Script (Portable Version)

## Overview

This script allows mounting and unmounting any number of NFS shares defined dynamically through environment variables. It enables fast switching of mount targets for home labs, workstation fleets, or shared NAS environments—without any code changes.  
This is a new version of a script I've used on my personal workstations for years. This version was made portable by utilizing environment variables instead of hard-coded values.

- Supports unlimited, user-defined share names.
- Mount or unmount multiple shares in one command.
- Fully controlled by environment, portable between systems.
- Easy integration into automation or user workflows.

***

## Environment Configuration

Before running the script, set the required environment variables:

```bash
export MYMOUNT_NASSERVER="nas.example.com"
export MYMOUNT_NASBASE="/mnt/nas"

# Define each NAS share you want available
export MYMOUNT_NAS_backups="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/backups"
export MYMOUNT_NAS_isos="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/Storage/ISO_Store"
export MYMOUNT_NAS_shares="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/Storage/ShareData"
# Add more as needed
# export MYMOUNT_NAS_media="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/Media"

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

You can place these in a file (e.g. `.mymount.env`) and source them:

```bash
source ~/.mymount.env
```

***

## Installation

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

## Usage

Mount shares (using '-E' to include local environment variables):

```bash
sudo -E mymount -b --isos        # Mount backups and isos
sudo -E mymount --all            # Mount all configured shares
```

Unmount shares:

```bash
sudo -E myunmount -b             # Unmount backups
sudo -E myunmount -a             # Unmount all shares
```

All mount/unmount options are auto-generated based on your environment settings.

***

## Adding/Removing Shares

To change available shares, simply edit your environment config:

- Add new shares:
  ```bash
  export MYMOUNT_NAS_project="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/ProjectFiles"
  ```
- Remove a share by un-exporting or deleting its definition from your env/config file.

No code changes required.

***

## Troubleshooting

- **Missing shares?**  
  Check your environment variable names. Each share must be set as `MYMOUNT_NAS_<share>`.

- **Mounting errors?**  
  Ensure you run as root (`sudo`) and that `mount.nfs` is installed.

- **Local mount points?**  
  If not overridden, they default to `$MYMOUNT_LOCAL_ROOT/<share>`. Override with `MYMOUNT_LOCAL_<share>` if needed.

- **Additional NFS options?**  
  Set `MYMOUNT_OPTS` to tweak mount behavior.

***

## Example .env File

```bash
export MYMOUNT_NASSERVER="nas.internal"
export MYMOUNT_NASBASE="/storage"
export MYMOUNT_NAS_data="${MYMOUNT_NASSERVER}:${MYMOUNT_NASBASE}/data"
export MYMOUNT_LOCAL_ROOT="/mnt/shares"
```

***

## License

MIT License. Use and modify at your own risk.

***

## Contributions

PRs and suggestions welcome! Fork and tailor to suit your local or organizational needs.

***

## Support

For questions, open an issue or reach out via your preferred channel.

---

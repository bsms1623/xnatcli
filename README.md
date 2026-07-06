# xnatcli

A lightweight Bash command-line client for listing and downloading datasets from an XNAT archive.

`xnatcli` uses the XNAT REST API via `curl` and requires only standard Unix tools (`bash`, `awk`, `curl`, and `column`).

## Features

- List all sessions within an XNAT project
- Filter sessions by subject ID
- Display:
  - Scan date
  - Subject ID
  - Experiment ID
  - Project name
- Download a complete imaging session as a ZIP archive

## Requirements (available as standard on most Linux installs)

- Bash
- curl
- awk
- column (typically provided by `util-linux`)
- Access to an XNAT instance

## Installation

Clone the repository:

```bash
git clone https://github.com/bsms1623/xnatcli.git
cd xnatcli
```

Make the script executable:

```bash
chmod +x xnatcli
```

Optionally place it somewhere on your PATH:

```bash
cp xnatcli ~/bin/
```

## Configuration

Create a credentials file at:

```text
~/.xnatcli
```

The file should contain:

```bash
SERVER=https://your-xnat-server.ac.uk
USER=myusername
PASSWORD=mysecretpassword
```

Restrict the permissions on the file:

```bash
chmod 600 ~/.xnatcli
```

## Usage

### List all sessions in a project

```bash
xnatcli -p MYPROJECT -l
```

Example output:

```text
Scan Date   Subject    Experiment   Project
---------   -------    ----------   -------
2026-07-01  SUBJ39080  XNAT_E19054  MYPROJECT
2026-06-17  SUBJ32264  XNAT_E19062  MYPROJECT
```

### List sessions for a specific subject

```bash
xnatcli -p MYPROJECT -l -s SUBJ39080
```

Example output:

```text
Scan Date   Subject    Experiment   Project
---------   -------    ----------   -------
2026-07-01  SUBJ39080  XNAT_E19054  MYPROJECT
2026-10-14  SUBJ39080  XNAT_E19311  MYPROJECT
```

### Download a session

```bash
xnatcli -g XNAT_E19054 -o /data/downloads
```

This will create:

```text
/data/downloads/XNAT_E19054.zip
```

## Command Reference

| Option | Description |
|----------|-------------|
| `-p PROJECT` | XNAT project name |
| `-l` | List sessions |
| `-s SUBJECT` | Filter session list by subject |
| `-g EXPERIMENT_ID` | Download a specific experiment/session |
| `-o OUTPUT_DIR` | Output directory for downloaded ZIP file |

## Typical Workflow

Find sessions within a project:

```bash
xnatcli -p MYPROJECT -l
```

Locate the desired experiment:

```text
Scan Date   Subject    Experiment
---------   -------    ----------
2026-07-01  SUBJ39080  XNAT_E19054
```

Download the session:

```bash
xnatcli -g XNAT_E19054 -o ~/Downloads
```

## Notes

- The "Experiment" column corresponds to the XNAT experiment/session ID.
- Experiment IDs are globally unique within an XNAT instance and can be used directly for downloading sessions.
- Downloaded files are retrieved as ZIP archives containing the DICOM data associated with the selected session.
- The script currently downloads all scans associated with an experiment.

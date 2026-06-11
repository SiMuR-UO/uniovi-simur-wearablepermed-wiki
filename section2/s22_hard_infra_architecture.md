---
layout: default
title: "Hard Infrastructure"
nav_order: 2
parent: "System Architecture"
---

# Description
This diagram represent the hardware infrastructure that SIMUR group offer to its projects

- **Server Node**: this node represent the servers where we train our models. 

- **NAS Nodes**: This node represent the servers where the input and output files are synchronized and mirror all the time to be preserved.

If you need access to some of these repositories contact with [Admin Support](mailto:amlopez@uniovi.es)

<object
    data="{{ site.baseurl }}/section2/assets/images/hard_infra_architecture.svg"
    type="image/svg+xml"
    style="max-width: 800px; width: 100%;">
</object>

# Administration and Usage Protocol: WPM Dataset (v2.1)

**System:** Synology DS225+ | **Servers:** Ameno / Aburrido | **Date:** May 2026

---

## 1. System Access (NAS)

| Field | Value |
|---|---|
| Web access (preferred) | `quickconnect.to/simur-wearablepermed` |
| Mobile app / QuickConnect ID | `simur-wearablepermed` |
| Admin username | `******` |
| Password | `******` |

> **Note for Linux users:** Use the web interface or the desktop client with selective sync. The "On-demand sync" feature is not available on Linux.

---

## 2. Directory Structure on the NAS

Access is centralised through the `wpm-dataset-group` user group. All members can see every experiment subfolder.

| Folder | Contents | Permissions |
|---|---|---|
| `/wpm-dataset` | Raw dataset collected at CLM | Read-only (immutable) |
| `/wpm-curated-dataset` | Dataset curated by SiMuR | Read-only (immutable) |
| `/wpm-experiments` | Data for each experiment | Read: all group members |
| `/wpm-experiments/exp_name/` | Experiment-specific folder | Write: responsible researcher + assigned members |

---

## 3. User Accounts on Servers (Ameno / Aburrido)

**One user account per experiment** is created for training tasks. If more than one researcher is involved, they must share the account or delegate access to one of them.

> **Important:** Access to experiments other than your own must be done via **NFS or SMB**. Do not sync additional folders to the server to avoid overloading local storage.

---

## 4. Procedure for a New Experiment (administrator)

### 4.1 Workspace on the NAS

1. Create a user account for the researcher in DSM (Control Panel → User & Group).
2. Add the user to the `wpm-dataset-group` group.
3. Create the subfolder `/wpm-experiments/exp_name/` in File Station.
4. Assign **write** permission to the responsible researcher and any assigned members via the "Permissions" tab in File Station.

### 4.2 Account on Ameno / Aburrido _(if training is required)_

1. Create the user account on the appropriate server (Ameno or Aburrido).
2. Sync **only** the experiment folder (`/wpm-experiments/exp_name/`) to the user's local directory. Do not sync any other folders.
3. To access other experiments, configure NFS or SMB access to the NAS.

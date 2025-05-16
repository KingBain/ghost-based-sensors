# Generated Report Overview

This document describes the structure and contents of the Ghost-Based Sensors (GBS) **analyze** report that you will see in your CI/CD job summary and as uploaded artifacts. The report surfaces four primary sections:

---

## 1. Report Details: Processes

**Table: List of all processes executed during the pipeline run**

| Column          | Description                                     |
| --------------- | ----------------------------------------------- |
| `proc.exepath`  | Full filesystem path to the binary invoked      |
| `proc.name`     | The process name (binary filename)              |
| `proc.pexepath` | Parent process executable path (who spawned it) |
| `proc.pname`    | Parent process name                             |
| `user.name`     | Username under which the process ran            |

*Example*:

| proc.exepath                        | proc.name | proc.pexepath                            | proc.pname      | user.name |
| ----------------------------------- | --------- | ---------------------------------------- | --------------- | --------- |
| `/home/runner/runners/.../bin/node` | `node`    | `/home/runner/runners/.../Runner.Worker` | `Runner.Worker` | `runner`  |
| `/usr/bin/bash`                     | `bash`    | `/home/runner/runners/.../Runner.Worker` | `Runner.Worker` | `runner`  |
| `/usr/bin/npm`                      | `npm`     | `/usr/bin/bash`                          | `bash`          | `runner`  |

---

## 2. Report Details: Contacted IPs

**Table: Outbound network connections established by processes**

| Column         | Description                                       |
| -------------- | ------------------------------------------------- |
| `fd.sip`       | Remote IP address contacted                       |
| `fd.sport`     | Remote port number                                |
| `proc.exepath` | Process executable path initiating the connection |
| `proc.name`    | Process name                                      |
| `user.name`    | Username under which the process ran              |

*Example*:

| fd.sip          | fd.sport | proc.exepath                        | proc.name         | user.name |
| --------------- | -------- | ----------------------------------- | ----------------- | --------- |
| `140.82.112.21` | `443`    | `/home/runner/.../bin/node`         | `node`            | `runner`  |
| `168.63.129.16` | `53`     | `/usr/lib/systemd/systemd-resolved` | `systemd-resolve` | `root`    |

---

## 3. Report Details: Contacted DNS Domains

**List: DNS names resolved by the runner during the job**

| Column   | Description                         |
| -------- | ----------------------------------- |
| `domain` | Fully qualified domain name queried |

*Example*:

* `github.com`
* `blob.core.windows.net`
* `system.aster.amazon`

---

## 4. Artifacts

**Uploaded Artifacts**: At the end of the `analyze` job, the following artifacts are produced for offline review:

| Name                | Description                                        |
| ------------------- | -------------------------------------------------- |
| `capture`           | Full Falco Sysdig capture (`.scap` or `.tar.gz`)   |
| `hashes`            | JSON list of SHA256 hashes for spawned executables |
| `package-manifests` | JSON snapshot of Node/Python packages installed    |

These artifacts can be downloaded from the Actions UI for deep forensic analysis or ingestion into your SIEM.

---

*End of report description.*

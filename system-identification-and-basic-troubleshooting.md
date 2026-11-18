# Linux System Identification and Basic Troubleshooting

## Objective

Before making changes on a Linux server, it is important to identify:

- Which user is logged in
- Which server you are connected to
- Which Linux distribution is running

This helps prevent accidental changes on the wrong machine or with the wrong account.

---

## Lab Environment

| Item | Value |
|--------|--------|
| Operating System | Ubuntu 22.04.5 LTS |
| Hostname | ubuntu-jammy |
| User | vagrant |
| Environment | VirtualBox + Vagrant |

---

## Step 1: Identify Current User

### Command

```bash {preview="false"}
whoami
```

### Output

```text
vagrant
```

### Explanation

The `whoami` command displays the currently logged-in user account.

---

## Step 2: Identify Hostname

### Command

```bash {preview="false"}
hostname
```

### Output

```text
ubuntu-jammy
```

### Explanation

The hostname identifies the system currently being managed.

---

## Step 3: Identify Linux Distribution

### Command

```bash {preview="false"}
cat /etc/os-release | head -3
```

### Output

```text
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
```

### Explanation

The `/etc/os-release` file contains Linux distribution information.

---

# Troubleshooting Scenario 1

## Problem Statement

A developer reports:

> "I cannot access the project-data directory. I receive a Permission Denied error."

---

## Investigation Approach

Instead of immediately changing permissions, first investigate:

- Directory permissions
- Directory ownership
- User group membership

### Check Directory Permissions

```bash {preview="false"}
ls -ld project-data
```

Example Output:

```text
drwx------ 2 alice developers 4096 Nov 17 10:00 project-data
```

### Check User Group Membership

```bash {preview="false"}
id bob
```

Example Output:

```text
uid=1002(bob) gid=1002(bob) groups=1002(bob)
```

---

## Key Learning

Do not immediately use:

```bash {preview="false"}
chmod 777 project-data
```

Investigate ownership, permissions and group membership first. Apply the principle of least privilege.

---

# Troubleshooting Scenario 2

## Problem Statement

A developer reports:

> "The website was working this morning but is now down."

---

## Investigation Process

### Verify Service Status

```bash {preview="false"}
systemctl status application.service
```

### Check Service Logs

```bash {preview="false"}
journalctl -u application.service
```

Example Error:

```text
Failed to start application.service
No space left on device
```

---

## Check Disk Utilization

```bash {preview="false"}
df -h
```

Example Output

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G   20G     0 100% /
```

---

## Investigate Directory Usage

```bash {preview="false"}
du -sh /*
```

### Purpose

Identify which directories are consuming disk space before deleting files.

---

## Lessons Learned

1. Investigate before fixing.
2. Verify the correct user and server before making changes.
3. Check logs before restarting services.
4. Find root causes rather than treating symptoms.
5. Use least-privilege access whenever possible.

---

## Verification

Successful verification included:

- Identifying logged-in user
- Identifying hostname
- Identifying operating system
- Investigating permissions issues
- Reviewing service logs
- Diagnosing disk space problems

---

## Skills Practiced

- Linux system identification
- Permissions troubleshooting
- User and group administration
- Service troubleshooting
- Log analysis
- Disk space investigation
- Root cause analysis

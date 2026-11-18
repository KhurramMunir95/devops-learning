# Day 3 – Linux Logs

## 1. What are Logs?
Logs are records of system activities, errors, and events.

They help us:
- Find errors.
- Troubleshoot problems.
- Monitor system activities.
- Understand service failures.

## 2. Log Directory

Main log directory:
/var/log/

## 3. Important Log Files

| File | Purpose |
|---|---|
| syslog | General system logs (Ubuntu) |
| auth.log | Login and authentication logs |
| kern.log | Kernel-related logs |

## 4. Useful Commands

List log files:
ls /var/log

View logs:
cat /var/log/syslog

View last 10 lines:
tail /var/log/syslog

Monitor live logs:
tail -f /var/log/syslog

Search for errors:
grep "error" /var/log/syslog

## 5. journalctl

Used to view systemd journal logs.

View all logs:
journalctl

View Docker service logs:
journalctl -u docker

Monitor live logs:
journalctl -f

## Real-World Example

If Docker fails to start:

journalctl -u docker

This helps identify why the service failed.

## Remember
Logs = System history + Errors + Troubleshooting.
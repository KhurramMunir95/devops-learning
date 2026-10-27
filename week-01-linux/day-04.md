# Day 04: Linux Users & Groups

## 🎯 Learning Objectives

By the end of this session, I should be able to:

- Identify the current Linux user.
- Understand UID and GID.
- Check which groups a user belongs to.
- Understand the purpose of `sudo`.
- Inspect Linux groups.

## 1. `whoami` — Current User

Displays the username of the current user.

```bash
whoami
```

Example output:

```text
khurram
```

**Purpose:** Helps identify which user is currently running commands.

Check the effective user when using `sudo`:

```bash
sudo whoami
```

Expected output:

```text
root
```

`sudo` runs a command with elevated privileges. `root` is the Linux superuser.

## 2. `id` — User and Group Information

Displays the user's UID, primary GID, and group memberships.

```bash
id
```

Example output:

```text
uid=1000(khurram) gid=1000(khurram) groups=1000(khurram),27(sudo)
```

| Term | Meaning |
|---|---|
| UID | User ID |
| GID | Group ID |
| Primary group | Main group associated with the user |
| Supplementary groups | Additional groups the user belongs to |

**Note:** Numeric IDs can differ between Linux systems.

## 3. `groups` — Group Membership

Shows the groups associated with the current user.

```bash
groups
```

Example output:

```text
khurram adm cdrom sudo dip plugdev users
```

Groups help manage permissions for multiple users.

For example, users in the same project group may be allowed to access shared project files.

## 4. `sudo` — Run Commands with Elevated Privileges

`sudo` allows an authorized user to run a command with elevated privileges.

Example:

```bash
sudo apt update
```

This runs `apt update` with the required administrative permissions.

**Best practice:** Use `sudo` only when necessary. Avoid running every command as root.

## 5. `getent group` — Inspect Groups

Display information about a specific group:

```bash
getent group sudo
```

Example output:

```text
sudo:x:27:khurram
```

The fields represent:

- `sudo`: group name
- `x`: password placeholder
- `27`: group ID (GID)
- `khurram`: listed group member

To check whether a group exists:

```bash
getent group developers
```

If the group exists, its information is displayed. If nothing appears, it may not exist or may not be available through the system's configured account sources.

## 6. Important Concepts

- **User:** An account that can run commands and own files.
- **Group:** A collection of users used to manage access.
- **UID:** Numeric identifier for a user.
- **GID:** Numeric identifier for a group.
- **Root:** The superuser with extensive system privileges.
- **sudo:** Runs a command with elevated privileges when authorized.

## 🧪 Hands-On Practice

Run these commands one by one:

```bash
whoami
id
groups
sudo whoami
getent group sudo
getent group users
```

Observe the output and answer:

1. What is your current username?
2. What is your UID?
3. What is your primary GID?
4. Which groups do you belong to?
5. What is the difference between `whoami` and `id`?
6. Why does `sudo whoami` usually display `root`?

## ✅ Completion Checklist

- [ ] Identified my current username.
- [ ] Understood UID and GID.
- [ ] Checked my group memberships.
- [ ] Understood the purpose of `sudo`.
- [ ] Practised inspecting groups.
- [ ] Recorded my observations.
- [ ] Committed and pushed the notes to GitHub.

## 📝 Key Takeaway

Linux uses users and groups to manage identities and access. `whoami`, `id`, and `groups` help inspect identity and memberships, while `sudo` allows authorized users to execute commands with elevated privileges.

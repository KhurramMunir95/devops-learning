# Day 05: Linux File Permissions & Ownership

## 🎯 Learning Objectives

By the end of this session, I should be able to:

- Read file permissions using `ls -l`.
- Understand read, write, and execute permissions.
- Change permissions using `chmod`.
- Understand numeric and symbolic permission modes.
- Change file ownership using `chown`.
- Verify permission and ownership changes.

## 1. `ls -l` — View Permissions

Create a practice directory and file:

```bash
mkdir -p ~/devops-lab/day5
cd ~/devops-lab/day5
touch app.log
```

Display file details:

```bash
ls -l app.log
```

Example output:

```text
-rw-r--r-- 1 khurram khurram 0 Oct 27 10:00 app.log
```

The first ten characters represent the file type and permissions:

```text
-rw-r--r--
```

- First character: file type (`-` means regular file, `d` means directory).
- Next three characters: owner permissions.
- Next three characters: group permissions.
- Last three characters: permissions for others.

## 2. Understand `r`, `w`, and `x`

| Permission | Meaning for a file |
|---|---|
| `r` | Read the file's contents |
| `w` | Modify the file's contents |
| `x` | Execute the file as a program or script |

Permissions apply to three categories:

- **Owner (`u`):** The file's owner.
- **Group (`g`):** Users in the file's assigned group.
- **Others (`o`):** Everyone who is neither the owner nor in the assigned group.

For directories, `r`, `w`, and `x` have different practical meanings: listing names, creating/removing entries, and traversing the directory.

## 3. `chmod` — Change Permissions

`chmod` changes file or directory permissions.

### Numeric method

The permission values are:

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| None (`---`) | 0 |

Add the values together for each permission category.

Examples:

| Command | Owner | Group | Others |
|---|---|---|---|
| `chmod 600 app.log` | Read, write | None | None |
| `chmod 644 app.log` | Read, write | Read | Read |
| `chmod 755 script.sh` | Read, write, execute | Read, execute | Read, execute |
| `chmod 000 app.log` | None | None | None |

The three digits always represent **Owner → Group → Others**.

Practise:

```bash
chmod 600 app.log
ls -l app.log
```

Then try:

```bash
chmod 644 app.log
ls -l app.log
```

### Symbolic method

Symbolic permissions use:

- `u` = owner
- `g` = group
- `o` = others
- `a` = all categories
- `+` = add permission
- `-` = remove permission
- `=` = set permissions exactly

Examples:

```bash
chmod u+x script.sh
chmod g-w app.log
chmod o+r app.log
chmod u=rw,g=r,o= app.log
```

The last command sets owner to read/write, group to read-only, and others to no permissions.

### Useful options

```bash
chmod -R 755 directory
chmod -v 600 app.log
chmod -c 644 app.log
```

- `-R`: Apply recursively to a directory and its contents.
- `-v`: Display a message for each processed file.
- `-c`: Display a message only when a change is made.

**Warning:** Be careful with recursive permission changes. Applying `755` to every file and directory can make files executable unnecessarily.

## 4. `chown` — Change Ownership

`chown` changes the owner and optionally the group of a file or directory.

Check the current ownership:

```bash
ls -l app.log
```

Change the owner:

```bash
sudo chown anotheruser app.log
```

Change both owner and group:

```bash
sudo chown anotheruser:developers app.log
```

Change only the group:

```bash
sudo chown :developers app.log
```

These examples require the relevant user or group to exist and sufficient privileges.

Check whether a group exists:

```bash
getent group developers
```

To change only the group, you can also use:

```bash
sudo chgrp developers app.log
```

**Important:** `chown` changes ownership, while `chmod` changes permissions. They solve different problems.

## 5. Practical Exercise

Use a test file, not an important system file.

```bash
cd ~/devops-lab/day5
echo "DevOps permission practice" > app.log
ls -l app.log
```

### Task A: Set permissions to `600`

```bash
chmod 600 app.log
ls -l app.log
```

Expected permission string:

```text
-rw-------
```

The owner can read and write the file. Group members and others have no file permissions.

### Task B: Set permissions to `644`

```bash
chmod 644 app.log
ls -l app.log
```

Expected permission string:

```text
-rw-r--r--
```

The owner can read and write; group members and others can read.

### Task C: Remove all ordinary permissions

```bash
chmod 000 app.log
ls -l app.log
```

Expected permission string:

```text
----------
```

Try reading the file as your normal user:

```bash
cat app.log
```

It should normally fail with a permission error when you do not have another applicable access path or elevated privileges.

Restore permissions after testing:

```bash
chmod 644 app.log
```

### Task D: Understand ownership

Check the current owner and group:

```bash
ls -l app.log
```

If you have access to another existing user or group, you can test a `chown` command on this practice file. Do not change ownership of system files.

## 6. Troubleshooting: Why Can I Still Read a `chmod 000` File?

If you can still read a file after running `chmod 000 app.log`, check the following:

1. **Are you root?**

   ```bash
   whoami
   id
   ```

   Root can bypass many ordinary file permission checks.

2. **Are you testing inside the Linux filesystem?**

   ```bash
   cd ~/devops-lab/day5
   pwd
   ```

   In WSL, test under your Linux home directory (`~`) rather than a Windows-mounted path such as `/mnt/c/...`. Windows-mounted filesystems may handle permissions differently depending on mount settings.

3. **Did the permissions actually change?**

   ```bash
   ls -l app.log
   ```

4. **Are you accessing the same file you changed?**

   ```bash
   readlink -f app.log
   ```

Do not assume every permission issue is caused by WSL. First verify the user, path, and actual permissions.

## 🧠 Quick Knowledge Test

1. What does `chmod 600 app.log` mean?
2. What is the difference between `chmod` and `chown`?
3. What does the `x` permission mean for a regular file?
4. In `chmod 754 file`, which digit represents the group?
5. What is the purpose of `ls -l`?
6. Why should you be careful with `chmod -R`?
7. Why might `chmod 000` behave differently on a Windows-mounted WSL path?

## ✅ Completion Checklist

- [ ] Created a test file in `~/devops-lab/day5`.
- [ ] Inspected permissions with `ls -l`.
- [ ] Tested numeric permission modes.
- [ ] Tested symbolic permissions.
- [ ] Understood owner, group, and others.
- [ ] Understood the difference between `chmod` and `chown`.
- [ ] Investigated permission behavior in WSL if relevant.
- [ ] Recorded results and errors.
- [ ] Committed and pushed the notes to GitHub.

## 📝 Key Takeaway

`chmod` controls what users can do with files and directories. `chown` changes who owns them. Always verify changes with `ls -l` and practise on test files before modifying important data.
# Day 02 — Linux `cp` Command

## 1. What is `cp`?

The `cp` command is used to copy files and directories from one location to another.

**Syntax:**

```bash
cp source destination
```

- `source`: File or directory you want to copy.
- `destination`: Location where you want to copy it.

**Example:**

```bash
cp file.txt backup.txt
```

Creates a copy of `file.txt` named `backup.txt` in the current directory.

## 2. Copy a File to Another Directory

```bash
cp errors.txt backup/
```

Copies `errors.txt` into the `backup` directory with the same filename.

## 3. Copy a File with a New Name

```bash
cp errors.txt backup/errors-backup.txt
```

Copies `errors.txt` into the `backup` directory and renames the copy to `errors-backup.txt`.

## 4. Copy a Directory

Use `-r` to copy a directory and its contents recursively.

```bash
cp -r project backup/
```

Copies the `project` directory, including its files and subdirectories, into `backup/`.

## 5. Useful `cp` Options

### `cp -r` — Recursive Copy

Copies a directory and its contents.

```bash
cp -r project backup/
```

### `cp -i` — Interactive Mode

Asks for confirmation before overwriting an existing destination file.

```bash
cp -i file.txt backup/file.txt
```

Type `y` to confirm or `n` to cancel when prompted.

### `cp -v` — Verbose Mode

Displays the source and destination of the copy operation.

```bash
cp -v file.txt backup/
```

Example output:

```text
'file.txt' -> 'backup/file.txt'
```

### `cp -n` — No Clobber

Prevents overwriting an existing destination file.

```bash
cp -n file.txt backup/file.txt
```

If the destination file already exists, its content remains unchanged.

## 6. Copy Files from Different Locations

You do not need to be inside the source or destination directory. You can use **absolute paths** to specify both locations.

**Example:**

```bash
cp /mnt/e/devops-learning/README.md /mnt/e/folder/copy-README.md
```

- Source: `/mnt/e/devops-learning/README.md`
- Destination: `/mnt/e/folder/copy-README.md`

This copies `README.md` into `/mnt/e/folder/` and names the copy `copy-README.md`.

**Important:** The destination directory must already exist. If it does not, create it first:

```bash
mkdir -p /mnt/e/folder
```

## 7. Copy a File or Directory While You Are in Another Location

Suppose your current directory is:

```bash
/home/khurram/devops-learning/week-01-linux
```

But you want to copy a file from `/mnt/e/devops-learning/` to `/mnt/e/backup/`.

You can run:

```bash
cp /mnt/e/devops-learning/README.md /mnt/e/backup/
```

You can copy a directory in the same way:

```bash
cp -r /mnt/e/devops-learning/project /mnt/e/backup/
```

**Key concept:** Your current working directory does not matter when you provide absolute paths.

## 8. Copy Multiple Files

```bash
cp errors.txt access.log backup/
```

Copies both files into the `backup/` directory.

## 9. Quick Reference

| Command | Purpose |
|---|---|
| `cp file.txt backup/` | Copy a file into a directory |
| `cp file.txt backup/new.txt` | Copy a file with a new name |
| `cp -r project backup/` | Copy a directory and its contents |
| `cp -i file.txt backup/` | Ask before overwriting |
| `cp -v file.txt backup/` | Show copy details |
| `cp -n file.txt backup/` | Do not overwrite an existing file |
| `cp source destination` | Copy using relative or absolute paths |

## 10. Practice Tasks

1. Create a directory named `backup`.
2. Copy `README.md` into it.
3. Copy the same file with the name `README-copy.md`.
4. Copy a directory using `cp -r`.
5. Try `cp -v` and observe the output.
6. Try `cp -i` when the destination file already exists.
7. From another working directory, copy a file using its absolute source and destination paths.

## Key Takeaway

The `cp` command copies files and directories. Use `-r` for directories, `-i` for overwrite confirmation, `-v` to see copy details, and `-n` to avoid overwriting existing files.

Using absolute paths lets you copy files between locations without changing your current directory.

# Linux `mv` Command

## 1. Purpose

The `mv` (move) command is used to:

- Move files and directories from one location to another.
- Rename files and directories.
- Move multiple files into a destination directory.

**Syntax:**

```bash
mv [OPTIONS] SOURCE DESTINATION
```

- `SOURCE`: File or directory to move or rename.
- `DESTINATION`: Target location or new name.
- `OPTIONS`: Flags that modify the command's behavior.

## 2. Common Flags

| Flag | Purpose |
|---|---|
| `-i` | Ask for confirmation before overwriting. |
| `-f` | Force the operation without prompting for overwrite confirmation. |
| `-n` | Do not overwrite an existing destination file. |
| `-v` | Display details of the operation. |
| `-u` | Move or overwrite only when the source is newer or the destination is missing. |
| `-t` | Specify the target directory before the source files. |
| `-T` | Treat the destination as a normal target, not as a directory. |

**Note:** Avoid `-f` when you are unsure about overwriting important files.

## 3. Practical Examples

### A. Rename a file

```bash
mv old.txt new.txt
```

Renames `old.txt` to `new.txt` in the same directory.

### B. Move a file into a directory

```bash
mv errors.txt backup/
```

Moves `errors.txt` into the `backup` directory.

### C. Ask before overwriting

```bash
mv -i file.txt backup/
```

If a file with the same name exists in the destination, `mv` asks for confirmation before overwriting it.

### D. Display operation details

```bash
mv -v errors.txt backup/
```

Displays information about the move operation.

### E. Move multiple files

```bash
mv file1.txt file2.txt file3.txt backup/
```

Moves all three files into the `backup` directory.

### F. Prevent overwriting

```bash
mv -n file.txt backup/
```

Does not overwrite an existing file with the same name in `backup/`.

### G. Move only if the source is newer

```bash
mv -u file.txt backup/
```

Moves the file if the destination is missing or the source is newer, subject to the system's file-comparison behavior.

When you run mv -u budget.xlsx Destination/, the computer acts like a smart assistant and looks at the "Last Modified" date and time (timestamp) of both files before doing anything:
• Scenario A (Source is newer): Your source file was saved at 5:00 PM, but the one in the destination folder was saved at 2:00 PM. The assistant sees your source file is newer, so it moves it and overwrites the old one.
• Scenario B (Destination is newer): Your source file was saved at 2:00 PM, but the one in the destination folder was saved at 5:00 PM. The assistant realizes you would lose your latest updates if it moved the old file over it. So, it does nothing and safely skips it.
• Scenario C (File doesn't exist yet): There is no budget.xlsx in the destination folder at all. The assistant just moves it normally.

### H. Specify the target directory first

```bash
mv -t backup/ file1.txt file2.txt
```

Moves both files into `backup/`.

### I. Treat the destination as a file target

```bash
mv -T old.txt new.txt
```

Treats `new.txt` as the exact destination path rather than interpreting it as a directory.

## 4. Move a Directory

```bash
mv project/ backup/
```

Moves the `project` directory into `backup/`.

## 5. Practice Exercise

Create a practice directory and perform the following tasks:

```bash
mkdir -p ~/mv-practice/backup
cd ~/mv-practice

touch test.txt old.txt
mv -v test.txt backup/
mv old.txt new.txt

ls -l
ls -l backup/
```

**Expected results:**

- `test.txt` is inside `backup/`.
- `old.txt` is renamed to `new.txt`.
- `new.txt` remains in the current directory.

## 6. Important Notes

- `mv` moves or renames; it does not normally create an additional copy.
- Use `pwd` to check your current directory.
- Use `ls` to verify file locations.
- Use `mv -i` when you want protection against accidental overwrites.
- A move across different filesystems may require copying and deleting internally.

## 7. Quick Revision

| Task | Command |
|---|---|
| Rename a file | `mv old.txt new.txt` |
| Move a file | `mv file.txt backup/` |
| Show operation details | `mv -v file.txt backup/` |
| Confirm before overwrite | `mv -i file.txt backup/` |
| Prevent overwrite | `mv -n file.txt backup/` |
| Move multiple files | `mv a.txt b.txt backup/` |

**Key takeaway:** Use `mv` to change a file's location or name. Choose flags such as `-i`, `-n`, and `-v` according to the behavior you need.


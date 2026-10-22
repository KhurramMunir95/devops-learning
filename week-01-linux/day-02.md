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

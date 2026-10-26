# Day 03 — Linux Package Management (APT & Nano)

## 1. What is APT?

APT (Advanced Package Tool) Ubuntu/Linux mein software packages ko search, install aur upgrade karne ke liye use hota hai.

Examples: `nginx`, `git`, `nano`

---

## 2. `sudo apt update`

```bash
sudo apt update
```

**Purpose:** Online repositories se latest package information download karke locally update karta hai.

### How it works
1. Online software repositories se connect karta hai.
2. Available packages aur versions ki information download karta hai.
3. Information local system par save karta hai.

**Location:** `/var/lib/apt/lists/`

**Important:** Ye installed software ko upgrade nahi karta aur na hi naya software install karta hai.

---

## 3. `sudo apt upgrade`

```bash
sudo apt upgrade
```

**Purpose:** Installed packages ko available newer versions par upgrade karta hai, agar applicable conditions allow karti hon.

Pehle `sudo apt update` chalana recommended hai.

**Example:**
- Installed Git version: `2.43`
- Available Git version: `2.45`
- `sudo apt upgrade` Git ko newer version par upgrade kar sakta hai.

---

## 4. `apt install`

### Install software

```bash
sudo apt install nginx
```

- Nginx installed nahi hai → install karega.
- Nginx pehle se installed hai → available newer version ho to upgrade kar sakta hai.

### Sirf installed package upgrade karna

```bash
sudo apt install --only-upgrade nginx
```

Ye Nginx ko tabhi upgrade karega jab woh pehle se installed ho.

---

## 5. Package versions check karna

### Installed program ka version

```bash
git --version
```

Git ka currently installed program version show karta hai.

### Installed aur available versions

```bash
apt policy git
```

Example:

```text
Installed: 2.43.0
Candidate: 2.45.0
```

- **Installed:** System mein currently installed version.
- **Candidate:** APT ke mutabiq install ya upgrade ke liye selected version.

*Versions sirf examples hain.*

### Installed packages ki list

```bash
apt list --installed
```

System mein installed packages show karta hai.

### Available upgrades ki list

```bash
apt list --upgradable
```

Un packages ko show karta hai jin ke liye upgrade available hai, based on the current package information.

---

## 6. `apt search`

```bash
apt search nginx
```

**Purpose:** Available package information mein matching software packages search karta hai.

Example:

```text
nginx - web/proxy server
nginx-common - common files for nginx
```

Search karne se software install nahi hota.

Install karne ke liye:

```bash
sudo apt install nginx
```

---

## 7. `nano` — Text Editor

```bash
nano notes.txt
```

Terminal mein text file create ya edit karne ke liye use hota hai.

Agar file exist nahi karti, to usay save karne par create kiya ja sakta hai.

### Useful shortcuts

| Shortcut | Purpose |
|---|---|
| `Ctrl + O` | Save file |
| `Enter` | Filename confirm |
| `Ctrl + X` | Exit Nano |

Agar Nano installed nahi hai:

```bash
sudo apt install nano
```

---

## 8. `sudo` aur password

```bash
sudo apt update
```

`sudo` command ko elevated permissions ke saath chalata hai, agar current user authorized ho.

Ubuntu password maang sakta hai.

**Important:** Password type karte waqt terminal mein characters ya `*` nazar nahi aate. Password type karke `Enter` press karo.

WSL mein aam tor par Ubuntu user ka password use hota hai, Windows password zaroori nahi.

---

## 9. APT output samajhna

Example:

```text
Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 132
```

- `Upgrading: 0` — Koi package upgrade nahi hua.
- `Installing: 0` — Koi naya package install nahi hua.
- `Removing: 0` — Koi package remove nahi hua.
- `Not Upgrading: 132` — 132 packages is operation mein upgrade nahi hue.

Exact reason samajhne ke liye command aur complete output check karna zaroori hai. Kuch packages hold back ho sakte hain ya dependency changes ki zarurat ho sakti hai.

---

## 10. Quick Command Reference

| Command | Purpose |
|---|---|
| `sudo apt update` | Package information refresh |
| `sudo apt upgrade` | Installed packages upgrade |
| `sudo apt install nginx` | Nginx install ya potentially upgrade |
| `sudo apt install --only-upgrade nginx` | Sirf pehle se installed Nginx upgrade |
| `apt search nginx` | Package search |
| `apt list --installed` | Installed packages ki list |
| `apt list --upgradable` | Available upgrades ki list |
| `apt policy nginx` | Installed aur candidate versions |
| `git --version` | Git program version |
| `nano notes.txt` | Text file create/edit |

---

## 11. Hands-on Practice

Commands ko ek ek karke run karo:

```bash
sudo apt update
apt policy nginx
apt search nginx
apt list --upgradable
nano linux-notes.txt
```

### Remember

- `update` = package information refresh
- `upgrade` = installed software upgrade
- `install` = software install
- `search` = package search
- `policy` = package versions check
- `nano` = text files edit
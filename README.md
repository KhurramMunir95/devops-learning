# DevOps Learning Journey

A hands-on DevOps learning journey focused on practical skills, daily progress, troubleshooting, and documenting everything on GitHub.

## Learning Schedule

- **Daily practice:** 30 minutes
- **Learning method:** 5 minutes concepts, 20 minutes hands-on practice, 5 minutes testing and review
- **Documentation:** Maintain daily Markdown notes with commands, examples, outputs, and troubleshooting
- **Version control:** Commit and push daily progress to GitHub
- **Weekly review:** Review completed tasks, challenges, and learning outcomes
- **Learning approach:** Complete one guided session at a time and revisit previous topics to reinforce retention

## Learning Roadmap

- **Week 01:** Linux fundamentals and networking
- **Week 02:** Docker and Nginx
- **Week 03:** CI/CD with GitHub Actions
- **Week 04:** Cloud fundamentals and monitoring

## Progress Tracker

### Week 01 — Linux Fundamentals

#### Day 1: Linux Filesystem & Navigation

**Commands:** `pwd`, `ls`, `cd`, `mkdir`, `touch`

**Practice:**
- Create folders and files inside `~/devops-lab/day1/`.
- Navigate between different directories.
- Understand absolute and relative paths.
- Explore the home directory (`~`) and filesystem root (`/`).

**Outcome:** Create and navigate a basic Linux directory structure.

**Notes:** `week-01-linux/day-01.md`

- [ ] Verify the practical exercise and documentation.

#### Day 2: File Operations & Reading

**Commands:** `cp`, `mv`, `rm`, `cat`, `less`, `head`, `tail`

**Practice:**
- Copy, move, rename, and remove test files.
- Read file contents using `cat` and `less`.
- Display the first and last lines of a file.
- Practise `tail` options, line numbering, and handling blank lines.

**Outcome:** Manage files and inspect their contents from the terminal.

**Notes:** `week-01-linux/day-02.md`

- [ ] Verify the practical exercise and documentation.

#### Day 3: Package Manager & Terminal Editor

**Commands:** `sudo apt update`, `apt search`, `apt install`, `nano`

**Practice:**
- Refresh the local package index using `sudo apt update`.
- Search for available packages using `apt search`.
- Install a package using `sudo apt install`.
- Create and edit a Markdown notes file using Nano.
- Understand the difference between updating package information and upgrading installed packages.

**Outcome:** Find package information, install software, and edit text files in the terminal.

**Notes:** `week-01-linux/day-03.md`

- [ ] Verify the practical exercise and documentation.

#### Day 4: Linux Users & Groups

**Commands:** `whoami`, `id`, `groups`, `sudo`

**Practice:**
- Identify the current username using `whoami`.
- Inspect UID, GID, and group memberships using `id`.
- List group memberships using `groups`.
- Understand common groups such as `sudo`, `adm`, and `users`.
- Learn how `sudo` executes commands with elevated privileges.
- Explore group information using `getent group`.

**Outcome:** Understand Linux user identity, numeric IDs, group membership, and basic privilege escalation.

**Notes:** `week-01-linux/day-04.md`

**Progress:** Commands studied and practised; notes prepared. Verify the final exercise and GitHub commit/push before marking the day complete.

- [ ] Verify the practical exercise and documentation.
- [ ] Commit and push Day 4 notes to GitHub.

#### Day 5: File Permissions & Ownership

**Commands:** `ls -l`, `chmod`, `chown`

**Practice:**
- Understand read (`r`), write (`w`), and execute (`x`) permissions.
- Learn the Owner → Group → Others permission order.
- Change permissions using numeric modes such as `600`, `644`, and `755`.
- Practise symbolic permissions such as `u+x`, `g-w`, and `o+r`.
- Change file ownership and groups using `chown`.
- Verify file permissions and ownership using `ls -l`.
- Investigate permission behaviour in Ubuntu on WSL.

**Outcome:** Understand basic Linux permissions and ownership, verify access settings, and troubleshoot common permission errors.

**Notes:** `week-01-linux/day-05.md`

- [ ] Complete the hands-on permission and ownership exercise.
- [ ] Verify the results and document any issues.
- [ ] Commit and push Day 5 notes to GitHub.

#### Day 6: Searching & Text Processing

**Commands:** `find`, `grep`, `sort`, `uniq`, `wc`

**Practice:**
- Find files by name.
- Search file contents for specific text.
- Sort text and identify duplicate lines.
- Count lines, words, and characters.
- Combine commands using pipes (`|`).

**Outcome:** Find files, filter text, and perform basic log analysis.

**Notes:** `week-01-linux/day-06.md`

- [ ] Complete the practical exercise and document the results.

#### Day 7: Processes, Networking & Weekly Review

**Commands:** `ps`, `top`, `free`, `df`, `du`, `ip`, `ping`, `curl`, `ss`

**Practice:**
- Inspect running processes and system resource usage.
- Check disk space and directory sizes.
- Inspect network interfaces and test connectivity.
- Understand IP addresses, ports, DNS, and HTTP.
- Review the week's commands through a small practical lab.

**Outcome:** Perform basic system inspection, understand networking fundamentals, and demonstrate the week's Linux skills.

**Notes:** `week-01-linux/day-07.md`

- [ ] Complete the weekly practical lab.
- [ ] Review and revise earlier notes.
- [ ] Document outstanding questions and mistakes.
- [ ] Commit and push the weekly review.

## Repository Structure

```text
devops-learning/
├── README.md
└── week-01-linux/
    ├── day-01.md
    ├── day-02.md
    ├── day-03.md
    ├── day-04.md
    ├── day-05.md
    ├── day-06.md
    └── day-07.md
```

## Daily Completion Checklist

For every learning session:

- [ ] Understand the concept.
- [ ] Execute the commands in Ubuntu/WSL.
- [ ] Complete the hands-on exercise.
- [ ] Verify the expected output.
- [ ] Document commands, examples, and troubleshooting.
- [ ] Review the key concepts.
- [ ] Commit and push the notes to GitHub.

Mark a day complete only after its practical exercise and documentation have been verified. Track the GitHub push separately if it is still pending.

## Learning Principles

1. Learn the concept before relying on tools.
2. Practise commands in a real Linux environment.
3. Understand what commands do instead of memorising them.
4. Use test files when experimenting with potentially destructive commands.
5. Document errors, troubleshooting steps, and lessons learned.
6. Prioritise practical exercises over certification alone.
7. Review earlier topics regularly to improve retention.
8. Track progress honestly and commit verified work to GitHub.

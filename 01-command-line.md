# 🐧 Linux Command Line — Beginner Story Notes (Single Markdown Block)

Welcome! Imagine the Linux filesystem as a city and you as an explorer. The **terminal** is your compass and map: you type commands, and Linux guides you around. In this first walk-through, we’ll learn how to find where you are, look around, move, create and remove things, read files, and get help — all gently, step by step.

---

## Opening the Terminal
Open the **Terminal** app (search for “Terminal”). The prompt (often ending with `$`) means Linux is ready for your command.

---

## Where am I? — `pwd`
Use `pwd` (Print Working Directory) to learn your exact location in the city.

- Example output: `/home/sanjeev`
- How to read:
  - `/` → **root** (the top-most place; nothing above it, like `C:\` in Windows)
  - `home` → a district under root
  - `sanjeev` → your personal area inside `home`

---

## What’s here? — `ls`
Use `ls` to list what’s in your current place (files and folders).
- Typical output looks like: `Desktop  Documents  Downloads  Music`

Tip: Use **Tab** for autocompletion. Start typing a name, press Tab, and the terminal finishes it if unique.

---

## Move around — `cd`
Change directory with `cd`.

- Go into a folder: `cd Downloads`
- Go up one level (to the parent): `cd ..`
- Go to root: `cd /`

⚠️ **Case-sensitive**: `Downloads` ≠ `downloads`. If the case doesn’t match, you’ll get “No such file or directory”.

---

## Paths: Absolute vs Relative
Think of **absolute paths** as full postal addresses and **relative paths** as directions from where you currently stand.

- **Absolute path**: always starts with `/` and works from anywhere.
  - Example: `cd /home/sanjeev/Downloads`
- **Relative path**: starts from your current location.
  - If you’re already in `/home/sanjeev`, then `cd Downloads` works.
  - From `/var`, to reach Downloads relatively: `cd ../home/sanjeev/Downloads` (first go up to `/`, then into `home`, then `sanjeev`, then `Downloads`).

Rule of thumb: if you’re unsure where you are or need a guaranteed jump, use an **absolute** path.

---

## Make things — `touch` and `mkdir`
Create an **empty file**:
- `touch file1`

Create a **folder**:
- `mkdir recently_downloaded`

Create things **elsewhere** by giving a path:
- From *anywhere*, create a file in Downloads: `touch /home/sanjeev/Downloads/note.txt`
- From *anywhere*, create a folder in Downloads: `mkdir /home/sanjeev/Downloads/my_new_folder`

Creating directly in **root** (`/`) usually requires admin rights:
- Prefix with `sudo` when needed (you’ll be asked for your password):
  - `sudo touch /another_file_at_root`

---

## Remove things — `rm` (files) and `rm -r` (folders)
Delete a **file**:
- `rm file1`
- Or with a path: `rm /home/sanjeev/Downloads/file1`

Delete a **folder** and all its contents:
- `rm -r my_new_folder`

`-r` means “recursive” — remove the directory and everything inside. Be careful: removals are immediate.

---

## Extra power with options — `ls -l`, `ls -a`, `ls -lh`
Commands can accept options (flags) to change their behavior:

- `ls -l` → long list (type, permissions, owner, size, modified time)
- `ls -a` → include hidden files (names starting with `.`)
- Combine flags: `ls -la` or `ls -l -a`
- Human-readable sizes: `ls -lh` (e.g., `1.9G` instead of `2040109465`)

---

## Get help — `--help` and `man`
Two built-in help systems:

- Quick help: `ls --help` (works for many commands; shows flags and usage)
- Manual pages: `man ls` (full documentation; press **Q** to quit)

---

## Move and rename — `mv`
`mv` does both **moving** and **renaming**.

- Move a file into another folder (relative paths here):
  - From `/home/sanjeev`: `mv newfile Music`
- Move using absolute paths:
  - `mv /home/sanjeev/image1 /var` (may require `sudo` if `/var`)
- **Rename** by “moving” to the same folder with a new name:
  - `mv image1 image30`
- Move **and** rename in one go:
  - `mv image30 /home/sanjeev/Documents/document10`

Remember: the first argument is the **source** (what you’re moving), the second is the **destination** (where and optionally under what new name).

---

## Read files — `cat`, `more`, `less`
When you want to **see** what’s inside a file:

- `cat file` → dumps the whole file (great for small files).
- `more file` → shows a screenful; press **Enter** for one line at a time or **Space** for the next page; press **Q** to quit.
- `less file` → like `more`, but faster and more flexible (use Enter/Space to navigate; **Q** to quit). Prefer `less` for large logs.

Example (system logs):
- `cd /var/log`
- `less syslog` (or `more syslog` / `cat syslog`)

---

## Recap — your mini toolkit
- Location: `pwd`
- Look around: `ls`, `ls -la`, `ls -lh`
- Move: `cd`, `cd ..`, `cd /`
- Create: `touch file`, `mkdir folder`
- Remove: `rm file`, `rm -r folder`
- Move/Rename: `mv source dest`
- Read files: `cat`, `more`, `less`
- Help: `command --help`, `man command` (quit with **Q**)
- Admin actions: prefix with `sudo` when writing to protected places like `/` or `/var`

With a bit of practice, these will feel natural. Wander, peek, tidy up, and read — that’s how you learn the city. Next steps could include permissions, users/groups, and `sudo` in detail, but for now, you have the essentials to explore confidently. Happy terminaling!
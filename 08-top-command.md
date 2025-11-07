# Understanding the `top` Command — Teacher-Style Notes

Welcome back everyone! Today, let’s explore one of the most powerful and most‑used Linux commands — **`top`**.  
If you’ve ever wondered what’s really happening inside your machine — which process is eating your CPU, how much memory is left, or why your fan suddenly sounds like a jet — `top` is your friend. Let’s learn it step by step, like we’re in class together.

---

## 1️⃣ What the `top` command does
When you type:
```bash
top
```
you open a real‑time, constantly updating dashboard of your system’s activity. Think of it as **Task Manager for Linux**, but more powerful and keyboard‑friendly.

At the very top of the screen, you’ll see system summary information — time, uptime, number of users, and the system **load average**.

---

## 2️⃣ The top header — what those first lines mean

### 🕒 Time & Uptime
- Shows the **current time**, how long the machine has been running, and how many users are logged in.

### ⚙️ Load Average
You’ll see something like:
```
load average: 0.33, 0.40, 0.50
```
These three numbers show the **system load** (how busy your CPU is) over **1, 5, and 15 minutes**.  
If you see values higher than your CPU core count, your system is struggling to keep up.

---

## 3️⃣ The Tasks section
This part summarizes all running processes:
```
Tasks: 311 total, 1 running, 243 sleeping, 3 stopped, 0 zombie
```
- **Running** → actively using CPU.  
- **Sleeping** → waiting or idle.  
- **Stopped** → paused.  
- **Zombie** → dead but not cleaned up (leftover processes).

If these sound strange, don’t worry — they’re just states of how the CPU handles work.

---

## 4️⃣ CPU usage breakdown
You’ll see a line like this:
```
%Cpu(s): 3.5 us, 1.2 sy, 0.0 ni, 95.0 id, 0.2 wa, 0.1 hi, 0.1 si, 0.0 st
```
Let’s decode that:

| Code | Meaning | Description |
|------|----------|--------------|
| us | user space | Time spent on user programs |
| sy | system | Time spent on kernel (system) tasks |
| ni | nice | Time on processes with changed priority |
| id | idle | CPU waiting for something to do |
| wa | I/O wait | Waiting for disk or input/output |
| hi | hardware interrupt | CPU dealing with hardware signals |
| si | software interrupt | CPU handling internal signals |
| st | steal time | CPU time taken by hypervisor (only on VMs) |

---

## 5️⃣ Memory section
Shows how much memory and swap is used or free. It’s similar to running:
```bash
free -m
```
but updates live inside `top`.

---

## 6️⃣ Process list — the big table below
Here’s where most of your focus will be. Let’s break the columns:

| Column | Meaning |
|--------|----------|
| PID | Process ID (unique number for each task) |
| USER | Which user owns it |
| PR | Priority |
| NI | Niceness (affects scheduling priority) |
| VIRT | Virtual memory used |
| RES | Physical RAM used |
| SHR | Shared memory |
| S | Process state (R = running, S = sleeping, etc.) |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| TIME+ | Total CPU time used |
| COMMAND | The command that launched it |

---

## 7️⃣ The screen refresh
Notice how `top` updates every few seconds — by default, every **3 seconds**.  
To change that:
- Press `d`
- Enter a new delay (for example, `1` for 1‑second updates)

---

## 8️⃣ Focusing on a single user
By default, `top` shows all users’ processes. To filter:
- Press `u`
- Type the username (e.g., `user1`)
- To go back to everyone, press `u` again and leave it blank.

---

## 9️⃣ Sorting data
By default, processes are sorted by **CPU usage**. But you can sort by other metrics:

| Shortcut | Sorts by |
|-----------|-----------|
| Shift + P | CPU usage (default) |
| Shift + M | Memory usage |
| Shift + N | Process ID |
| Shift + T | Time running |

To **reverse** the order (ascending/descending), press `Shift + R`.

---

## 🔍 Interactive sorting menu
Want to pick columns interactively?
Press `Shift + F`. You’ll see all columns listed.  
- Use the arrow keys to highlight one.  
- Press `S` to sort by it.  
- Press **Space** to toggle visibility of any column.  
- Press `Esc` to go back.

This is super useful if you want to hide some fields or sort by something unusual like “nice value” or “resident memory”.

---

## 10️⃣ Viewing threads instead of processes
By default, `top` shows processes. To see **threads**, press `Shift + H`.  
Press it again to switch back.

---

## 11️⃣ Viewing CPU and memory graphically
We humans love visuals. Try pressing these keys:
- **T** → Cycles CPU stats view (numbers → bar graph → filled bar → off)  
- **M** → Same for memory

Keep pressing them to toggle between styles until you find what feels easiest to read.

---

## 12️⃣ Killing a process safely from `top`
You can end a process directly inside `top` — just like using the `kill` command.

Steps:
1. Find the process’s **PID** (e.g., Firefox 6961).  
2. Press `k`.  
3. Type the PID and press Enter.  
4. It will ask which **signal** to send:  
   - **15 (SIGTERM)** → polite request to stop (default).  
   - **9 (SIGKILL)** → force kill, no cleanup.

It’s best to try **15** first; **9** is like yanking the plug out of the wall.

---

## 13️⃣ Quick summary of keys you should practice

| Key | Action |
|-----|--------|
| `d` | Change refresh delay |
| `u` | Filter by user |
| `Shift + P` | Sort by CPU |
| `Shift + M` | Sort by memory |
| `Shift + N` | Sort by PID |
| `Shift + T` | Sort by runtime |
| `Shift + R` | Reverse sort order |
| `Shift + F` | Open field/sort menu |
| `Shift + H` | Toggle thread view |
| `T` | Change CPU display style |
| `M` | Change memory display style |
| `k` | Kill process |

---

## 🧠 Final teacher’s advice
`top` is like the heartbeat monitor of your Linux box. The more you use it, the more sense those numbers will make.  
Play around — change delays, sort columns, filter users, and try killing test processes you start yourself.

Once you’re comfortable, check out **htop** — it’s a colorful, modern upgrade with easier navigation and mouse support.

That’s it for today’s deep dive — I hope you now feel confident exploring what’s really going on inside your system!

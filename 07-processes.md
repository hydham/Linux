# Managing and Monitoring Processes — Sanjeev’s Style Notes

Hey everyone! Today we’re going to dig into processes in Linux. Let me start by explaining what a process is—this is really important.

A **process** is just a running instance of any program, application, or command on your system. So whenever you run a command or open an app, Linux creates a process for it in memory, and it gets a unique number called a **PID** (process ID). That PID is how Linux keeps track of it.

Now, processes aren’t just floating around randomly. They’re organized in a family tree kind of way. Every process has a **parent**, which is the process that started it, and can have many **child** processes that it created. Parents pass some information to their children when they create them. When a process finishes, it cleans up after itself and tells its parent.

Some processes start right when your computer boots up and just wait quietly in the background for tasks. These are called **daemons** (sometimes people say “demons” but it’s “daemons”).

Processes come in two flavors:
- **Foreground processes** are the ones you start and wait for—they grab your terminal until they finish.
- **Background processes** run behind the scenes and don’t need your input.

### Example: Running a Foreground Process

Let’s say you run this command:

```bash
sleep 300
```

This just tells the system to “do nothing” for 300 seconds. While it’s sleeping, your terminal is locked—you can’t type anything else.

If you want to stop it before the 300 seconds are up, just hit `Ctrl+C`. This sends a "SIGINT" (interrupt) signal, which politely tells the process to stop. We’ll learn about signals later.

### Suspending and Backgrounding a Process

If you press `Ctrl+Z` instead, that suspends the process (sends a SIGSTOP) and frees your terminal. You’ll see a message showing you the job ID.

To see all suspended or background jobs, type:

```bash
jobs
```

You can restart a suspended job in the foreground like this:

```bash
fg %jobID
```

Or in the background:

```bash
bg %jobID
```

You can also start a job directly in the background by adding `&` at the end:

```bash
sleep 300 &
```

This way, your terminal stays free.

---

## Process States

Processes can be in different “states”:

- **running**: Actively doing work or ready to run.
- **waiting**:
  - *Interruptible*: Waiting for an event but can be stopped with a signal.
  - *Uninterruptible*: Waiting on hardware, can’t be stopped.
- **Stopped**: Paused, usually by a signal like SIGSTOP (`Ctrl+Z`).
- **Zombie**: Done running but still taking up space because its parent hasn’t acknowledged its end.

---

## Monitoring Processes

The main command to see what’s running is:

```bash
ps
```

But by default, `ps` only shows your own processes. To see all processes on the system:

```bash
ps -aef
```

There’s a lot of output here, including these important columns:

- **UID**: User who owns the process.
- **PID**: Process ID.
- **PPID**: Parent process ID.
- **CMD**: The command running.

The very first process after boot is `init` (PID 1), it starts everything else.

For real-time process monitoring, use:

```bash
top
```

It refreshes every few seconds and shows you live CPU and RAM usage, which helps if your system slows down.

---

## Sending Signals to Processes

You control processes by sending signals with the `kill` command:

- `SIGINT` (`Ctrl+C`): Nicely ask to stop.
- `SIGKILL` (`kill -9`): Force stops a process immediately.
- `SIGTERM` (default): Gracefully asks to quit.
- `SIGSTOP` (`Ctrl+Z`): Suspends a process.
- `SIGHUP`: Used for daemons, tells them to reload config.

Example: To politely stop (SIGTERM) process 1234:

```bash
kill 1234
```

Force kill (SIGKILL):

```bash
kill -9 1234
```

---

## Quickly Finding Process IDs

To get PIDs easily, use:

```bash
pgrep processname
```

or

```bash
pidof processname
```

And to kill by name:

```bash
pkill processname
```

(Be careful with `pkill` as it kills all matching processes.)

---

## Process Niceness (Priority)

Every process has a niceness value between -20 (highest priority) and +19 (lowest priority). Default is 0.

Lower niceness = higher priority = better CPU time.

To start a process with a new nice value:

```bash
nice -n 10 command
```

To change niceness of a running process:

```bash
renice -n 10 -p PID
```

---

That’s the basics! Practice these commands and managing your Linux processes will feel like second nature. Don’t worry if you get stuck — it’s all part of learning.

---

If you want this as a `.md` file, ready for your GitHub, just let me know!

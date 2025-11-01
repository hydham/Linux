# 🧠 Linux Lecture Notes — `sudo`, Root, `su`, and Who Gets Admin Power (Story Style)

Welcome back! In the last session we briefly *used* `sudo`. Today we’ll understand it properly: who **root** is, why Ubuntu hides root by default, how `sudo` lets you **borrow** root’s power one command at a time, how the **sudo session** timer works (and how to tune it safely), how to **become root temporarily**, how to **enable/disable** direct root login, and how Linux decides **who** gets to use `sudo`. We’ll follow exactly what the lecturer demonstrated.

---

## Root exists, but Ubuntu keeps it closed (on purpose)

Every Linux system has a special account: **root**. Root can read/write/delete any file, install or remove any software, and—if careless—break the system. Some distributions (e.g., CentOS) allow direct logins as root (even over SSH). **Ubuntu** does not: by default, **no root password is set**, so you cannot log in as root or `su` to root.

To switch to another user you would normally use:

    su - <username>

To try switching to **root**, you can omit the username:

    su -

On Ubuntu this asks for the **root password**, which does not exist by default—so it fails. Hence the question: how do we do admin tasks? The answer is **`sudo`**.

---

## `sudo`: borrow root’s power for one command (while staying yourself)

`sudo` means *superuser do*. You remain your user; only the specified command runs with root’s privileges.

Without `sudo`, a privileged action fails:

    touch /newfile
    # -> Permission denied

With `sudo`, you’re prompted for **your** password (not root’s), and the command runs as root:

    sudo touch /newfile
    # enter YOUR user password

That’s the core model the lecturer emphasized: **precise, per-command elevation**.

---

## The sudo session: cached for a short time (default 15 minutes)

After you successfully authenticate the first time, Linux starts a **sudo session**: subsequent `sudo` commands won’t ask again for a while. By default the grace period is **15 minutes**. If you wait beyond that, the next `sudo` will prompt again.

To **end** the session immediately (useful before handing your keyboard to someone else):

    sudo -k

The next `sudo` will ask for a password again.

---

## Inspecting and safely editing sudo behavior (use `visudo`)

The configuration for `sudo` lives in:

    /etc/sudoers

Reading it typically requires elevation:

    sudo cat /etc/sudoers

**Do not** edit `/etc/sudoers` directly with a normal editor; a typo can lock you out of admin access. Use the safe wrapper that runs syntax checks before saving:

    sudo visudo

(Depending on your system, `visudo` may open `nano` by default; the lecturer showed using it and where to make the change.)

To **tune the session timeout**, the lecturer added a Defaults line (near entries like `Defaults    env_reset`):

    Defaults timestamp_timeout=40

Meanings demonstrated in class:
- `timestamp_timeout=40` → cache lasts **40 minutes**.
- `timestamp_timeout=0`  → **always** prompt (no caching).
- `timestamp_timeout=-1` → **never** time out once authenticated.

In `nano`: press Ctrl+X, then Y, then Enter to save. (In the demo the instructor showed the edit but declined saving.)

---

## Temporarily becoming root for a shell (`sudo -i`) — use sparingly

To start a **root shell** (you become root until you exit):

    sudo -i
    whoami
    # root

Exit to return to your normal user:

    exit

Best practice from the lecture: prefer per-command `sudo` so only what needs elevation is elevated; use a root shell briefly and intentionally.

---

## Enabling and disabling direct root login (if you really need it)

By default on Ubuntu, root has **no password** (login disabled). To set one (thus enabling direct root login):

    sudo passwd root
    # set and confirm a new root password

You can then become root with:

    su -
    # enter the root password you just set

To **disable** direct root login again (lock the password):

    sudo passwd -l root

After locking, `su -` to root will fail again (as it did initially).

---

## Who’s allowed to use `sudo`? (the `sudo` group and sudoers rule)

Ubuntu grants `sudo` rights to members of the **sudo** group. In `/etc/sudoers` you’ll find a rule (the lecturer showed this) that effectively says: *members of the `sudo` group can run commands via `sudo`*.

You can see which groups you belong to:

    groups

A new user typically has a primary group with the same name as the user (e.g., `user1`).

---

## Creating a new user and granting `sudo` access

Create a new user (the lecture used `user1`):

    sudo adduser user1
    # set password and optional details; confirm

Switch to that user and test a privileged action:

    su - user1
    sudo touch /user1_file
    # "user1 is not in the sudoers file. This incident will be reported."

So new users **do not** get `sudo` by default. Switch back and add them to the `sudo` group:

    exit
    # lecturer showed help first:
    usermod --help
    # -a = append to supplemental groups
    # -G = specify group list
    sudo usermod -aG sudo user1

Verify:

    su - user1
    groups
    # now includes 'sudo'
    sudo touch /user1_file
    # enter user1's OWN password; succeeds

---

## Optional: allow `sudo` without password (NOPASSWD)

If you want `sudo` to run **without** asking for a password for a specific user, add a line via `visudo`:

    <username> ALL=(ALL) NOPASSWD:ALL

Example used in class:

    sanjeev ALL=(ALL) NOPASSWD:ALL

The lecturer warned this weakens security; use only when you fully understand the risk.

---

## Quick reference (exact commands used in the lecture)

    # switch users
    su - <username>

    # attempt to become root (fails on Ubuntu by default)
    su -

    # run one command as root (asks for YOUR password)
    sudo <command>
    sudo touch /newfile

    # end cached sudo session immediately
    sudo -k

    # open sudoers safely and tune timeout
    sudo visudo
    #   Defaults timestamp_timeout=40
    #   Defaults timestamp_timeout=0
    #   Defaults timestamp_timeout=-1

    # become root for a shell (then exit)
    sudo -i
    whoami
    exit

    # enable/disable direct root login
    sudo passwd root
    su -
    sudo passwd -l root

    # user/groups and granting sudo
    groups
    sudo adduser user1
    usermod --help
    sudo usermod -aG sudo user1
    su - user1
    groups
    sudo touch /user1_file

    # optional NOPASSWD (via visudo)
    #   <username> ALL=(ALL) NOPASSWD:ALL

---

## Big-picture recap (what the lecturer emphasized)

- **Root** can do anything; Ubuntu blocks direct root login by default.
- **`sudo`** elevates **one command** while you remain your user.
- A short **sudo session** caches your auth (default **15 minutes**); end it with `sudo -k`.
- Edit sudo policy safely with **`sudo visudo`**; tune `timestamp_timeout` to your needs.
- Use **`sudo -i`** sparingly; least privilege is safer day-to-day.
- New users don’t get `sudo` automatically—**add them to the `sudo` group**.
- `NOPASSWD` is possible but reduces security; use with caution.

That completes the lecturer’s walkthrough of `sudo`: purpose, workflow, session behavior, safe configuration, root login toggling, and granting admin rights to new users.
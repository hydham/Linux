# Let's Talk About Linux Users, Groups, and Permissions

## So Why Do We Care About Users and Groups?

let’s dive into how Linux thinks about users, groups, and permissions. Imagine Linux as a lively apartment building—lots of people coming and going. Each person (or user) should have their own space, their own keys, and some privacy.  
Linux lets many users share a system at once, but we don’t want anyone peeking into someone else's stuff, right? That's why permissions and groups exist!

A **user** is anyone who can log in.  
A **group** is just a bundle of users.  
Groups help you set permissions easily—so you don’t have to keep track of a hundred people one by one; you can grant access to a group and get on with the day!

Here’s a story for you: Pretend we run a hospital. We’ve got doctors, nurses, and yes, even janitors with accounts on our server. Patient info should be visible to nurses and docs, but not janitors. If we had hundreds of users, setting permissions for each would be a nightmare. Instead, we toss them into groups (doctors, nurses, janitors) and set permissions for those groups. Simple and effective!

## Finding Out Who’s On the System

Alright, let’s see who lives in our Linux "apartment building." If you open the `/etc/passwd` file, you'll see a list of all users:
```bash
sudo cat /etc/passwd
```
Scroll to the bottom for users you've created—those are your real neighbors! Each user gets a special space, called a **home directory** (like `/home/sanjeev`).

Similarly, you can peek at all the groups:
```bash
sudo cat /etc/group
```
Again, skip the system stuff at the top—look for real groups at the bottom.

## What Groups Do I Belong To?

Want to know who's in your "friend circle"? Type:
```bash
groups
```
You'll see your main group and any others you’ve joined. By default, your primary group matches your username, but you can join many more.

To check your primary group, try:
```bash
id -gn
```
Files you make will belong to you and your primary group.

## Adding New Users and Groups

Let's say a new doctor joins. You welcome them with:
```bash
sudo adduser drsmith
```
Set a password and jot down some details. Their home will be `/home/drsmith`.

To create a group for, say, "surgeons":
```bash
sudo groupadd surgeons
```
These help organize our staff!

Whenever you add someone, always double-check with:
```bash
sudo cat /etc/passwd
```
And if you ever feel brave, you can edit the list of users directly—but please, it's risky! If you must, use a safer command:
```bash
sudo vipw
```
This double-checks for errors so you don’t accidentally lock folks out.

Editing groups is similar:
```bash
sudo vigr
```

## Changing Passwords the Easy Way

If someone forgets their password (it happens), you fix it with:
```bash
sudo passwd username
```
If you leave out the username, you’ll change your own password.

## Removing Users & Groups

If someone leaves the hospital, you remove them from the list:
```bash
sudo userdel someone
```
And for groups:
```bash
sudo groupdel groupname
```

Double-check the results with:
```bash
sudo cat /etc/passwd
sudo cat /etc/group
```

## Joining or Leaving Groups

Sometimes you want to add a nurse to the "emergency" group. Use:
```bash
sudo usermod -aG emergency nurse1
```
The `-aG` options mean “append to these groups.” To kick them out (politely):
```bash
sudo deluser nurse1 emergency
```
Remember, `deluser` removes someone from a group; `userdel` removes them entirely.

## What Are File Permissions?

Now for the fun part—permissions!
Every file or folder has three sets of rules:
- Read: Look at stuff.
- Write: Change stuff.
- Execute: Run stuff (like scripts).

Type:
```bash
ls -l
```
You'll see something like: `drwxr-xr-x`. The first letter means if it’s a folder (d) or file (-). Then, nine characters show permissions for user, group, and everyone else.

For example:  
- `rwx` for the owner? They can do anything.
- `r-x` for the group? They can look and run, but not change.
- `r--` for others? Only allowed to look.

## How to Change Permissions (chmod)

We use `chmod` to set permissions. Let me show you two approaches:

### Letters and Symbols

Suppose you want the user to be able to execute a file:
```bash
sudo chmod u+x filename
```
Want to take it away?
```bash
sudo chmod u-x filename
```
If you want to be fancy, you can combine changes:
```bash
sudo chmod u+x,o-r filename
```
User gets execute, others lose read.

Set a group’s permissions exactly:
```bash
sudo chmod g=rw filename
```
Group can now only read and write.

### The Number System

Permissions can also be set like this:
- 4 is read
- 2 is write
- 1 is execute

Add them up for each set:
```bash
chmod 777 filename
```
Everyone gets everything.

Another example:
```bash
chmod 740 filename
```
User: all, group: read, others: nothing.

## Special Note About Directories

Here’s something students always ask: If I change a folder’s permissions, does it change what’s inside? Nope! You need to use recursive mode:
```bash
sudo chmod -R 777 foldername
```
This updates everything inside.

## Changing Who Owns a File

Suppose a file needs to switch owners:
```bash
sudo chown newuser filename
```
Or change its group:
```bash
sudo chgrp newgroup filename
```

Want to update everything inside a folder?  
Just add `-R` for recursion.

---

That's a wrap! Take these basics and try out the commands; you’ll get the hang of it. If anything’s fuzzy, just ask in the next class. Good work, everyone!

# Automate Tasks with Cron Jobs

Hey everyone! Today I’m going to show you how to automate tasks in Linux using something called a cron job. Cron jobs let you run commands automatically at set times, so you don’t have to remember to do them yourself.

## What is a cron job?

Imagine you want to back up some files every Sunday at 2:30 a.m. Now, you certainly don’t want to get out of bed and log in at that exact time. So instead, you tell Linux, “Hey, please run this backup command for me every Sunday morning at 2:30.” That’s a cron job.

All cron jobs for your user are stored in a text file called the **crontab**.

## Viewing existing cron jobs

To see what cron jobs you already have set up, run:

```bash
crontab -l
```

If you see a lot of lines starting with `#`, those are comments. Comments are not active cron jobs. If there are no active jobs, you’ll just see comments or no output.

## Adding or editing cron jobs

To add a new cron job or edit existing ones, run:

```bash
crontab -e
```

The first time you do this, Linux will ask you to pick an editor. You can choose `vi` or `nano`, or whatever you like.

This opens your crontab file where each line represents a cron job.

## Crontab format basics

A cron job line has two parts:

- The **time specification** (when to run)
- The **command** (what to run)

The time part has five columns:

1. Minute (0-59)
2. Hour (0-23)
3. Day of month (1-31)
4. Month (1-12)
5. Day of week (0-7, where 0 or 7 is Sunday)

For example:

```
30 2 * * 0 tar czf /backups/backup.tgz /home/user
```

This means: at minute 30 past hour 2 (2:30 a.m.) every day of the month, every month, but only on Sunday (`0`), run the tar backup command.

## Examples

- **Backup every day at 2:30 a.m.**

```
30 2 * * * tar czf /backups/backup.tgz /home/user
```

Here, `*` means “any value,” so it runs every day regardless of month or day of week.

- **Run at 10 a.m. on the 1st day of every month**

```
0 10 1 * * command
```

- **Run at 2 a.m. on November 3rd**

```
0 2 3 11 *
```

You can also write the month as `NOV` (all caps).

## Special notations

- **Every minute**

```
* * * * *
```

- **Every hour (at zero minute)**

```
0 * * * *
```

- **Every 5 minutes**

```
*/5 * * * *
```

- **Every 3 hours**

```
0 */3 * * *
```

- **Every Saturday and Sunday at midnight**

```
0 0 * * 0,6
```

You can also write days as `SAT,SUN`.

## Multiple values in a column

Use commas to specify multiple values. For example:

- Run at 0, 10, and 20 minutes every hour:

```
0,10,20 * * * *
```

## Permissions and sudo

Each user has their own crontab. So what you add with `crontab -e` is for your user only.

If your scheduled command needs root privileges (for example, installing packages), simply adding `sudo` won’t work because cron won’t ask for your password.

Instead, edit root’s crontab with:

```bash
sudo crontab -e
```

Add your root-required cron jobs here.

---

That’s it for cron jobs! They’re incredibly handy and once you get comfortable with the syntax, they’ll save you tons of time.

Want this in a Markdown file for your GitHub? Just say the word!

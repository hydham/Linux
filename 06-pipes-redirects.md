# Pipes and Redirections

Hey everyone! Today we're going to talk about two super useful Linux features: **pipes** and **redirections**. But before we get into those, let's understand something really important: every command or process in Linux has three data streams.

---

## The Three Data Streams

- **Standard Input (stdin)**: This is the input to a command. For example, when you type `ls /var/log`, `/var/log` is the input.
- **Standard Output (stdout)**: This is the normal output of a command. When you run `ls`, the list of files is the output.
- **Standard Error (stderr)**: This is where error messages go. If you try to read a file that doesn't exist, the error message is sent here.

Each of these streams has a number:
- stdin = 0
- stdout = 1
- stderr = 2

---

## Redirections

### Redirecting Output

Let's say you want to save the output of a command to a file instead of seeing it on the screen. You can do that with redirection.

Example:
```bash
echo "some random text" > random_file.txt
```
This sends the output (stdout) to `random_file.txt`. The `>` symbol means "redirect to."

If you want to be specific, you can use:
```bash
echo "some random text" 1> random_file.txt
```
Here, `1` stands for stdout.

### Redirecting Errors

If a command gives an error, you can redirect that error to a file too.

Example:
```bash
cat nonexistent_file 2> error_file.txt
```
Here, `2` stands for stderr.

### Redirecting Both Output and Error

You can send both output and error to separate files:
```bash
./script.sh 1> output.txt 2> error.txt
```
Or, send both to the same file:
```bash
./script.sh 1> combined.txt 2>&1
```
Here, `2>&1` means "send stderr to wherever stdout is going."

### Appending vs Overwriting

- `>` overwrites the file if it exists.
- `>>` appends to the file if it exists.

Example:
```bash
echo "first line" > file.txt
echo "second line" >> file.txt
```
Now `file.txt` will have both lines.

---

## Pipes

Pipes let you take the output of one command and send it as input to another command.

Example:
```bash
ls -l /bin | more
```
This lists files in `/bin` and sends the output to `more`, so you can scroll through it.

You can chain multiple commands:
```bash
ls -l /bin | grep "vdir" | wc -l
```
This lists files, finds lines with "vdir," and counts how many there are.

---

## Summary

- **stdin (0)**: Input to a command.
- **stdout (1)**: Normal output.
- **stderr (2)**: Error messages.
- Use `>` to redirect output, `2>` for errors, and `>>` to append.
- Use `|` to pipe output from one command to another.

---

That's it for pipes and redirections! Practice these commands and you'll be able to control where your data goes and how it flows between commands.

---



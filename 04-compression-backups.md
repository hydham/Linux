# Compression Tools: Sanjeev's Style Notes

Hey everyone, today I’m going to walk you through some common compression tools you’ll find on any Linux system. Let’s start with the most popular one—gzip.

## gzip Basics

We have this file, say `logfile1`. You might want to check how big it is, but sometimes the number looks weird, right? So use this command:

```bash
ls -lh logfile1
```

The `-h` means human readable. It’ll show you something like 136K instead of a huge number.

To compress it, just run:

```bash
gzip logfile1
```

Boom, now you’ve got `logfile1.gz` and the original file is gone because gzip replaces it. If you check the file size again:

```bash
ls -lh logfile1.gz
```

You’ll see it’s much smaller. This is what compression does—makes files smaller.

If you want to decompress (go back to the original), do this:

```bash
gzip -d logfile1.gz
```

Or, you can use:

```bash
gunzip logfile1.gz
```

Either works. After decompressing, you’ll have your original file back, and the `.gz` file disappears.

## Keeping Original Files

What if you want to keep the original file but also make a compressed copy? Use:

```bash
gzip -c logfile1 > logfile1_compressed.gz
```

The `-c` flag means write compressed output to standard output (which we then redirect to a new file). Original stays safe.

To decompress a copy but keep the compressed version:

```bash
gzip -dc logfile1_compressed.gz > logfile1_copy
```

Same idea.

## Peek Inside Compressed Files

If you try to read a compressed file with `cat`, it’s just garbage. So instead, do:

```bash
gunzip -c logfile1.gz | less
```

or

```bash
zcat logfile1.gz | less
```

This works like `cat` but for compressed files, and lets you scroll through the content easily.

## Compression Level

gzip lets you control compression level from 1 to 9:

- 1 is fastest compression, but bigger file
- 9 is slowest compression, but smallest file

Default is 6, which balances speed and size.

Try:

```bash
gzip -9 logfile1
```

That’ll squeeze it the most, but can take longer.

Or:

```bash
gzip -1 logfile2
```

Compresses quickly but won’t get you the smallest file. For small files like 136K, you won’t notice much difference, but for big files, it matters.

## Compressing Directories

You CAN’T compress a whole directory with just gzip:

```bash
gzip directoryName
```

will fail.

But you can compress all files inside recursively:

```bash
gzip -r directoryName
```

This compresses each file inside but leaves the directory intact.

Later, we’ll see a better way to back up whole directories (hint: tar).

## bzip2

bzip2 is another compression tool working similarly to gzip.

To compress:

```bash
bzip2 logfile1
```

To decompress:

```bash
bunzip2 logfile1.bz2
```

To read compressed files without decompressing first:

```bash
bzcat logfile1.bz2
```

Commands are almost identical to gzip, so learning one helps with the other.

## tar Command for Archiving and Compression

tar stands for tape archive and is great for making backups.

To make an archive (a bundle):

```bash
tar cvf backup.tar directoryName
```

- `c`: create
- `v`: verbose (shows you what’s being added)
- `f`: file name to create (`backup.tar`)

To compress the archive with gzip on the fly:

```bash
tar czvf backup.tar.gz directoryName
```

To look inside an archive without extracting:

```bash
tar tvf backup.tar
```

To extract an archive:

```bash
tar xvf backup.tar
```

To extract compressed archive:

```bash
tar xzvf backup.tar.gz
```

## tar with bzip2

Almost identical; just replace the `z` with `j`:

```bash
tar cjvf backup.tar.bz2 directoryName
```

tar xjvf backup.tar.bz2
```

## Keep Permissions When Backing Up?

By default, tar won’t keep file permissions perfectly.

Use `p` flag to preserve permissions:

```bash
tar czvpf backup_with_perm.tar.gz directoryName
```

Extract with permissions:

```bash
tar xzvpf backup_with_perm.tar.gz
```

---

That’s the basics! Try these commands yourself as you learn. Compression tools are super useful to save storage space and make backups easier. Let me know if you want me to make this into a GitHub-ready .md file!

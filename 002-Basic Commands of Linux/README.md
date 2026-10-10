# 002 — Basic Commands of Linux

This section introduces commonly used Linux terminal commands for navigating directories, managing files, viewing text, and getting help. These notes support my Linux learning and cybersecurity / OSCP preparation.

## Topics

- Terminal basics and command syntax
- Directory navigation with `pwd`, `ls`, and `cd`
- File and directory management with `touch`, `mkdir`, `cp`, and `mv`
- Viewing files with `cat`, `less`, and `head`
- Identifying file types with `file`
- Finding command documentation with `man`
- Basic system information with `whoami` and `date`

## Essential Commands

| Command | Purpose | Example |
|---|---|---|
| `pwd` | Show the current directory | `pwd` |
| `ls -la` | List files, including hidden files | `ls -la` |
| `cd /etc` | Change directory | `cd /etc` |
| `mkdir practice` | Create a directory | `mkdir practice` |
| `touch notes.txt` | Create an empty file | `touch notes.txt` |
| `cp notes.txt backup.txt` | Copy a file | `cp notes.txt backup.txt` |
| `mv old.txt new.txt` | Move or rename a file | `mv old.txt new.txt` |
| `cat notes.txt` | Display file contents | `cat notes.txt` |
| `less notes.txt` | Read a file interactively | `less notes.txt` |
| `head -n 5 notes.txt` | Show the first five lines | `head -n 5 notes.txt` |
| `file notes.txt` | Identify a file type | `file notes.txt` |
| `man ls` | Open the manual for a command | `man ls` |
| `whoami` | Show the current username | `whoami` |
| `date` | Show system date and time | `date` |

## Practice Checklist

- [ ] Check your current directory with `pwd`.
- [ ] List files with `ls -la`.
- [ ] Navigate using absolute and relative paths.
- [ ] Create a practice directory and files.
- [ ] Copy and rename files.
- [ ] Read files with `cat` and `less`.
- [ ] Read a command manual using `man`.
- [ ] Explain the difference between absolute and relative paths.

## Safety Tips

- Check the current directory before making changes.
- Be careful with commands that remove files or directories, especially when using elevated privileges.
- Practice in a dedicated test directory or disposable virtual machine.

## Why This Matters for Cybersecurity

Linux commands are useful for system administration, troubleshooting, log review, file analysis, enumeration, and authorized security testing. Understanding command output is more useful than memorizing commands alone.

## Learning Resources

- [Linux man-pages](https://man7.org/linux/man-pages/)
- [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)

## Lesson Notes

- [001 — Basic Command of the Linux](./001-Basic-Command-of-the-Linux.md)

---

**Author:** [Gourav Kushwah](https://github.com/Gourav-Kushwah20)  
**Repository:** [Linux-Server-Essential-2025](https://github.com/Gourav-Kushwah20/Linux-Server-Essential-2025)

# Filtering Empty and Comment Lines with `grep`

## View and Edit a File

To open and edit a file using `vim`:
```bash
vim os-release
```

## Show Only Empty Lines

To display only the empty lines in a file:
```bash
grep "^$" os-release
```

## Show Only Non-Empty Lines

To exclude all empty lines:
```bash
grep -v "^$" os-release
```

## View the SSH Configuration File

To display the contents of the SSH client configuration file:
```bash
cat /etc/ssh/ssh_config
```

## Remove Comment Lines

To exclude lines that start with `#` (comments):
```bash
grep -v "#" /etc/ssh/ssh_config
```

## Remove Comment and Empty Lines (Two-Step Method)

Use two `grep` commands to remove both comment and empty lines:
```bash
grep -v "#" /etc/ssh/ssh_config | grep -v '^$'
```

## Remove Comment and Empty Lines (Single `grep` with Extended Regex)

A concise way to filter out both comment and empty lines:
```bash
grep -v -E "(^#|^$)" /etc/ssh/ssh_config
```


## Create and View the File
```bash
vim user.txt
```
**Contents:**
```
user
user.
 user1
 user12
 user123
 user1234
 user 1234
 user  1234
user-name
user_name
user name
123
1234
12345
USERNAME
```

To view the file:
```bash
cat user.txt
```

## Basic Pattern Matching
```bash
grep "user" user.txt
```

## Match with Special Characters
```bash
grep "user\." user.txt
```

## Match with Any Two Characters After 'user'
```bash
grep "user.." user.txt
```

## Case-Insensitive Match for 'user.'
```bash
grep -i "user\." user.txt
```

## Match Any of the Characters 1–8
```bash
grep [12345678] user.txt
```

## Match Any Digit
```bash
grep [0-9] user.txt
```

## Match Two Consecutive Digits
```bash
grep [0-9][0-9] user.txt
```

## Match Numbers Starting with 3 Followed by a Digit
```bash
grep 3[0-9] user.txt
```

## Match 'user' Followed by a Digit
```bash
grep user[0-9] user.txt
```

## Match Lowercase Letters
```bash
grep [a-z] user.txt
```

## Match Letters Case-Insensitive
```bash
grep -i [a-z] user.txt
```

## Match Both Uppercase and Lowercase Letters
```bash
grep [A-Z,a-z] user.txt
```
## Match any 4-digit number
```bash
grep [1][0-9][0-9][0-9] passwd
```
Finds any sequence starting with `1` followed by any three digits (i.e., numbers from 1000 to 1999) in the `passwd` file.

## Extract only matched 4-digit numbers
```bash
grep [0-9][0-9][0-9][0-9] passwd
```
Prints only the matched 4-digit sequences, not the full lines.

## Match special characters
```bash
grep [0-9][0-9][0-9][0-9] passwd -o
```

## Match Special Characters
```bash
grep "[@#$%]" /var/log/messages
```
Searches for any of the special characters `@,` `#,` `$,` or `%` in the `/var/log/messages` file.


## Search for "root" in `/etc/*` and suppress errors
```bash
grep root /etc/* 2> /dev/null
```
Searches for "root" in all files in `/etc/,` ignoring permission errors using `2> /dev/null`.
## Case-insensitive search

Case-insensitive search for "root".
```bash
grep -i root /etc/* 2> /dev/null
```
Recursive, case-insensitive search for "root".

```bash
grep -iR root /etc/* 2> /dev/null
```
Recursive, case-insensitive search showing line numbers.
```bash
grep -iRn root /etc/* 2> /dev/null
```
List only filenames containing "root".
```bash
grep -l root /etc/* 2> /dev/null
```
Recursive version of the below.
```bash
grep -rl root /etc/* 2> /dev/null
```
Same as below (option order doesn’t matter).
```bash
grep -Rl root /etc/* 2> /dev/null
```
Case-insensitive list of filenames with "root".
```bash
grep -il root /etc/* 2> /dev/null
```

## Filtering IP Addresses with `ifconfig`

Show IPv4 addresses (ignore IPv6):
```bash
ifconfig | grep "inet "
```

```bash
ifconfig | grep "inet " | grep -v "inet6"
```

## Process Monitoring with `ps`

View all processes:
```bash
ps -aux
```

Filter for processes related to root:
```bash
ps -aux | grep root
```

Match lines starting with 'root':
```bash
ps -aux | grep '^root'
```

Exclude lines starting with 'root':
```bash
ps -aux | grep -v '^root'
```

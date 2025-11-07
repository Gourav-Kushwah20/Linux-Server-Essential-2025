
# 🔁 Handling Duplicates and Live Monitoring in Linux

This guide covers how to handle duplicate lines in a text file and how to monitor system activity in real-time using `watch` and `tail` commands.

---

## 📄 Handling Duplicate Lines

Create a new file:

```bash
vim duplicates.txt
```

### ✍️ Content of `duplicates.txt`

```text
apple
banana
apple
cherry
banana
date
cherry
```

---

### 🚫 Attempt to Remove Duplicates Using `sort -u`

```bash
sort -u duplicates.txt
```

This command **does not work properly if the file is not sorted** beforehand. It removes duplicate lines only if they are adjacent.

---

### 🔍 Using `uniq` Alone Won't Help

```bash
uniq duplicates.txt
```

`uniq` only removes **consecutive** duplicate lines, so the file must be sorted first.

---

### ✅ Correct Approach: Sort Then Use `uniq`

```bash
sort -h duplicates.txt | uniq
```

Sorts the file and then removes all duplicates.

---

### ✅ Count Occurrences of Each Unique Line

```bash
sort -h duplicates.txt | uniq -c
```

Outputs each line along with its number of occurrences.

---

## 👁️ Live System Monitoring with `watch` and `tail`

### 🧑‍💻 Watch Currently Logged In Users

```bash
watch -n 0.5 w
```

Refreshes the list of logged-in users every 0.5 seconds.

---

### 🧠 Watch Running Processes

```bash
watch -n 0.5 ps -aux
```

Displays all processes and refreshes every 0.5 seconds.

---

### 📜 Monitor Log File in Real-Time

```bash
watch -n 0.5 tail /var/log/messages
```

Displays the last few lines of `/var/log/messages` and updates them every 0.5 seconds.

---

### 📢 Follow Log File Output

```bash
tail -f /var/log/messages
```

Outputs new lines from the log file as they are written.

---

## 📝 Notes

- `uniq` requires sorted input for full effectiveness.
- `watch` is useful for observing real-time system status.
- Combine `sort`, `uniq`, and `watch` effectively for diagnostics and scripting.

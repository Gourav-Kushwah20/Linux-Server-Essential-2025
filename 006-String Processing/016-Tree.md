# Linux `tree` Command – Examples with Explanation

The `tree` command displays directories and files in a **tree-like structure**.

---

## 🔹 Basic Usage
```bash
tree
```
➡️ Shows the tree structure of the **current directory**.

---

## 🔹 Help
```bash
tree --help
```
➡️ Displays available options and usage.

---

## 🔹 Display a Directory Structure
```bash
tree /home/
```
Example output:
```
/home/
└── sec-learner
    ├── Desktop
    │   └── d1
    │       └── passwd
    ├── Documents
    ├── Downloads
    ├── Music
    ├── Pictures
    ├── Public
    ├── Templates
    └── Videos
```
➡️ Prints the folder structure of `/home/`.

---

## 🔹 Show Hidden Files
```bash
tree -a
```
➡️ Includes hidden files (starting with `.` like `.bashrc`).

---

## 🔹 Sort Alphabetically
```bash
tree -v -a /home/
```
➡️ Sorts directories/files alphabetically (case-insensitive) and shows hidden files.

---

## 🔹 Reverse Order
```bash
tree -r -a /home/
```
➡️ Lists directories/files in **reverse order**, including hidden ones.

---

## 🔹 Sort by Modification Time
```bash
tree -t -a /home/sec-learner
```
➡️ Sorts by **last modification time**, including hidden files.

---
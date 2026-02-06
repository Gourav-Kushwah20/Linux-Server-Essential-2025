# ✏️ **`nano` Text Editor**

The `nano` command is a **simple, lightweight, and beginner-friendly** text editor for editing files directly in the Linux 🐧 terminal.  
It is ideal for quick edits, configuration changes, and learning command-line editing because all shortcuts are **visible at the bottom** of the editor.

---

## 🧰 **`nano`**

### 🧾 **Syntax**
```bash
nano [OPTIONS] filename
```

* **`filename`** → File to open or create
* If the file does not exist, `nano` will **create it automatically** 🆕

---

## 📌 **Common Usage Examples**

### 📂 Open or create a file

```bash
nano file.txt
```

✔ Opens `file.txt`
✔ Creates the file if it doesn’t exist

---

### 🔢 Show line numbers

```bash
nano -l file.txt
```

Helpful when editing scripts or configuration files 📜

---

### 🔒 Open file in read-only mode (no editing)

```bash
nano -v file.txt
```

Prevents accidental modifications ❌✍️

---

### 🎯 Open file at a specific line and column

```bash
nano +10,5 file.txt
```

➡️ Cursor starts at **line 10, column 5**

---

### 📏 Disable line wrapping

```bash
nano -w file.txt
```

Useful for long commands, logs, or code files 💻

---

## ⌨️ **Basic `nano` Keyboard Shortcuts**

> 💡 `CTRL` is shown as `^` inside nano (for example, `^O` = `CTRL + O`)

### 📄 File Operations

* **Save (Write to file):** `CTRL + O` → Press **Enter** ✅
* **Exit nano:** `CTRL + X` 🚪

---

### ✂️ Editing Text

* **Cut a line:** `CTRL + K`
* **Paste a line:** `CTRL + U`
* **Copy a line:**

  * Cut with `CTRL + K`
  * Paste multiple times with `CTRL + U`

---

### 🔍 Searching & Replacing

* **Find text:** `CTRL + W` → Type text → **Enter**
* **Find & replace:** `CTRL + \`

  * Enter search text
  * Enter replacement text

---

### 📍 Navigation

* **Beginning of file:** `CTRL + Y` ⬆️
* **End of file:** `CTRL + V` ⬇️

---

### ↩️ Undo / Redo

* **Undo last action:** `ALT + U`
* **Redo last undone action:** `ALT + E`

---

## 📖 **Help & Manual Pages**

For full documentation and advanced options 📚:

```bash
nano --help
man nano
```

✨ **Tip:** If you are new to Linux, `nano` is the best editor to start with before moving to advanced editors like `vim` or `emacs`. ✨

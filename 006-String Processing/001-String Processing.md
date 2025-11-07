# 🧵 String Processing in Linux Commands

**String processing** in Linux refers to the manipulation, transformation, and analysis of text (strings) using the command line. This is widely used in shell scripting, automation, and text analysis tasks.

---

## ✅ Common String Processing Tasks

- **Searching**: Find patterns or specific text
- **Replacing**: Substitute strings
- **Extracting**: Pull out specific parts of text
- **Transforming**: Modify strings (e.g., change case)
- **Splitting/Joining**: Break into parts or concatenate

---
# 🔍 String Processing and Finding Files

Linux provides powerful command-line tools for working with text and searching files. Below is a handy cheat sheet of essential string processing commands.

---

## 🧰 Core Commands

|Command | Description |
|---------|-------------|
| 🔝`head` | Output the first N lines of a file |
| 🔚`tail` | Output the last N lines of a file |
| 🔢`wc` | Word count: count lines, words, and characters |
| 📑`sort` | Sort lines alphabetically or numerically |
| ✂️`cut` | Cut out specific fields or columns from each line |
| 🔗`paste` | Merge lines of files horizontally |
| 🔍`grep` | Search for lines matching a pattern or regex |
| 📊`awk` | Pattern scanning and data extraction language |
| 🛠️`sed` | Stream editor for find-and-replace, insert, delete |
| 🛠️`tr` | Translate or delete characters in text |

---

## 📌 Quick Examples

```bash
head -n 10 file.txt            # 🔝 First 10 lines of a file
tail -f log.txt                # 🔚 Follow new lines added to a log
wc -l words.txt                # 🔢 Count lines in a file
sort data.txt                  # 📑 Sort file content
cut -d',' -f2 data.csv         # ✂️ Get the second field from CSV
paste file1.txt file2.txt      # 🔗 Combine lines side by side
grep "error" syslog.log        # 🔍 Find lines containing "error"
awk '{print $1}' names.txt     # 📊 Print the first word of each line
sed 's/foo/bar/g' input.txt    # 🛠️ Replace "foo" with "bar"
tr 'a-z' 'A-Z' < input.txt     # 🛠️ Convert text to uppercase
```
## 📌 Common Examples

### 1. Search for a Word in a File
```bash
grep "error" logfile.txt
```
### 2. Replace Text in a String

echo "hello world" | sed 's/world/Linux/'

### 3. Extract the 2nd Column from a CSV

cut -d',' -f2 file.csv

### 4. Convert Lowercase to Uppercase

echo "linux" | tr 'a-z' 'A-Z'

### 5. Trim Whitespace (Leading & Trailing)

echo "  hello world  " | awk '{$1=$1; print}'

🧠 Tip

You can combine commands with pipes (|) for advanced string manipulation. For more powerful needs, use scripting languages like Bash, Perl, or Python.

### 📚 Related Topics

    Shell scripting

    Regular expressions

    Text processing pipelines


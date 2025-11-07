# 🧮 Sort Commands in Linux

Below is a sample workflow demonstrating how to use the `sort` command with different options on a file named `test1.txt`.

---

## 📄 Step 1: Create the File

Open `test1.txt` with `vim`:

```bash
vim test1.txt
```
## ✍️ Content of test1.txt
```bash
banna
apple
cherry
date
eldeberry
fig
grape
20
5
100
45
2
8
90
banana
apple
cherry
date
eldeberry
fig
grape
John 25
Alice 30
Bob 22
Eve 35
Charlie 28
Diana 30
John,25,Engineer
Alice,30,manager
Bob,22,Intern
Eve,35,Director
Charlie,28,Analyst
Diana,30,Consultant
March
January
Febuary
May
April
June
July
Agust
September
Octomber
Nov
Dec
```
### Save and exit in vim:
```bash
# Press Esc, then type:
:wq!
```
---
## 🔀 Sort Command Usages
#### 1. Default Sort
```bash
sort test1.txt
```
Sorts all lines in **dictionary (ASCII) order**, considering uppercase and lowercase.

---

#### 2. Human-Readable Number Sort
```bash
sort -h test1.txt
```
---
#### 3. Dictionary Order Sort (ignores punctuation)
```bash
sort -d test1.txt
```
Sorts using **dictionary order** (only letters, numbers, and blanks are considered).

---

#### 4. Numerical Sort
```bash
sort -n test1.txt
```
Sorts **only numeric values**, and may leave text entries in unpredictable order.

---
#### 5. Reverse Sort

```bash
sort -r test1.txt
```
Sorts in **reverse lexicographical order**.

---
#### 6. Reverse Human-Readable Number Sort
```bash
sort -hr test1.txt
```
Sorts **human-readable numbers in reverse**.

---
#### 7. Unique Numeric Sort
```bash
sort -u -n test1.txt
```
Sorts **numerically** and removes **duplicate numeric lines**.

---
#### 8. Unique Sort (Alternative Syntax)
```bash
sort --unique -n test1.txt
```
Same as above, using `--unique` instead of `-u`.

---
#### 9. Random Sort (Shuffling)
```bash
sort -R text1.txt
```
#### Shuffles lines randomly. (**Note**: `-R` is not supported in all Unix systems — it's available in GNU `sort`.)
---
## 📝 Notes

- Use `sort --help` to explore more sorting options.
- Combine flags like `-nr` (numeric + reverse) or `-k` (sort by column).


```bash
vim datafile.txt
```
In datafile.txt
```bash
John 25
Alice 30
Bob 22
Eve 35
Charlie 28
Diana 30
```
```bash
sort datafile.txt
```
```bash
sort -h datafile.txt
```
```bash
sort -h -k 1 datafile.txt
```
```bash
sort -h -k 2 datafile.txt
```


## 🔀 Sort by Fields Example

Open another file:
### ✍️ Content of `datafile.txt`

```text
John 25
Alice 30
Bob 22
Eve 35
Charlie 28
Diana 30
```

---

### 10. Default Lexicographical Sort (by full line)

```bash
sort datafile.txt
```

Sorts by the entire line lexicographically.

---

### 11. Human-Readable Sort (entire line)

```bash
sort -h datafile.txt
```

Treats numeric values as human-readable but sorts whole lines.

---

### 12. Sort by First Field (Name)

```bash
sort -h -k 1 datafile.txt
```

Sorts by the **first column (name)** alphabetically.

---

### 13. Sort by Second Field (Age)

```bash
sort -h -k 2 datafile.txt
```

Sorts by the **second column (age)** numerically.

---

## 📅 Sort by Month Names

Create a new file:

```bash
vim month-name.txt
```
### ✍️ Content of month-name.txt
```bash
March
January
Febuary
December
November
April
```
### 14.Sort by Month Names
```bash
sort -M month-name.txt
```
Sorts month names chronologically (January to December), ignoring invalid names.

# AWK Commands

**Search files for Pattern:**  
- AWK scans files line by line, checking if a line matches a specified pattern.

**Performs Operation on Matching Lines:**  
- When a line matches the pattern, AWK executes commands defined in its action block `{ ... }`.

**Performs Operation on Specific Lines:**  
- You can target specific lines using patterns like `NR==5` (fifth line) or `/pattern/` (lines matching the pattern).

**In AWK Body, Bash (Shell) Features Don't Work:**  
- Bash syntax and features are not available inside AWK's action blocks.

**Completely New Language in AWK Body:**  
- AWK uses its own programming language, distinct from shell scripting.

**Completely New Features Can Be Called on Specific Lines (Special Text):**  
- AWK allows powerful features and custom actions to be triggered by matching specific text or patterns in lines.

# 📚 Complete AWK Command Guide with Examples

## Basic Command to Print Entire Line

## 1. Print Entire Line from `/etc/passwd`
```bash
awk '{print $0}' /etc/passwd
```
Explanation:

`$0` represents the entire current line. This command prints every line of the `/etc/passwd` file as-is.

## 2. Print First Field (Username) from passwd
```bash
awk -F : '{print $1}' passwd
```
Explanation:

`-F :` sets the field separator to a colon (`:`), which is the delimiter used in the `passwd` file.

`$1` refers to the first field, which is the username

## 3. Print Third Field (User ID) from passwd
```bash
awk -F : '{print $3}' passwd
```
Explanation:

`$3` refers to the third field in the colon-separated file, which represents the User ID (UID).

## 4. Duplicate of Previous Command (Print UID Again)

```bash
awk -F : '{print $3}' passwd
```
Explanation:

This is the same as the previous command and again prints the User ID (UID) from the `passwd` file.

## 📁 User Info: `/etc/passwd` and `/etc/shadow`

### 5. Extract Specific Fields from Both Files

```bash
awk -F : '{print $1,$4,$7}' /etc/passwd /etc/shadow
```
- `-F `: sets the field separator to :

- `$1`: Username

- `$4`: Group ID (GID) in `/etc/passwd`

- `$7`: Login shell in `/etc/passwd`

- `/etc/shadow `doesn't have the same structure, so output may not be meaningful for it.

### 6.Print Line Containing "root"

```bash
awk -F : '/root/{print $0}' /etc/passwd
```
- Finds and prints full line(s) containing `root`.

### 7. Print Username of "root"
```bash
awk -F : '/root/{print $1}' /etc/passwd
```
- Prints only the username field (root).

### 8.Print Password Field for "root"
```bash
awk -F : '/root/{print $2}' /etc/passwd
```
- Prints password field (`x` usually, indicating shadow file is used).

### 9.Print UID of "root"
```bash
awk -F : '/root/{print $3}' /etc/passwd
```
- Prints user ID of root (typically `0`).

## 🌐 Network Info: ifconfig

### 10.Print All Lines (Unchanged)
```bash
ifconfig | awk '{print $0}'
```
- Prints every line exactly as is.

### 11.Show All inet Addresses (IPv4 + IPv6)
```bash
ifconfig | awk  '/inet/{print $0}'
```
- Prints lines containing inet (matches both IPv4 and IPv6).

### 12.Show Field 1 of IPv4 Lines
```bash
ifconfig | awk  '/inet /{print $1}'
```
- Filters only IPv4 lines (inet with space).

- Prints first word (usually "inet").

### 13. Show IP Address
```bash
ifconfig | awk  '/inet /{print $2}'
```
- Prints the second word of IPv4 address lines (usually the actual IP address).

### 14.Show Interface Name (MTU Line)
```bash
ifconfig | awk  '/mtu/{print $1}'
```
- Filters lines with `mtu` and prints the interface name.

### 15.Show MAC Address Line
```bash
ifconfig | awk  '/ether/{print $0}'
```
- Prints the entire line containing `ether`, which includes the MAC address.


## 🛠️ AWK Program Structure

```bash
awk 'BEGIN {Code_in_Begin} {Code_in_main} END{Code_In_End}' inputfile1 inputfile2
```

```bash
awk 'BEGIN {Begin} {main} END{End}' inputfile1 inputfile2
```
- **BEGIN**: Executes once before processing any input.
- **Main**: Executes for each input line.
- **END**: Executes once after all input is processed.


## 📌 1. BEGIN Section
✅ Executed once before any lines are processed.

📍 Useful for initialization, like printing headers or setting variables.

#### 🔹 Example:
```bash
awk 'BEGIN { print "🔍 Starting to process the file..." }' file.txt
```

## 📌 2. Main Section ({ Action })
✅ Executed for every line in the input.

📍 By default, matches all lines if no pattern is specified.

📍 Fields in each line are accessed with:

- $1, $2, ..., $NF (last field)

#### 🔹 Example:
```bash 
awk '{ print "📄 Line:", $1, $NF }' file.txt
```
- `$1` → first field

- `$NF` → last field

## 📌 3. END Section
✅ Executed once after all lines are processed.

📍 Useful for summarizing or cleanup operations.

🔹 Example: 
```bash
awk 'END { print "✅ Processing Complete" }' file.txt
```
## ✅ Full Example
```bash
awk 'BEGIN {print "Processing files..."}
{print "Line: " $1,$NF}
END {print "Processing Complete"}' file.text
```
This program:

- Prints a message before processing

- Prints the first and last word of each line

- Prints a completion message at the end

## 1. Extract IP addresses from ifconfig
```bash
ifconfig | awk '/inet /{print $2}'
```
- 🔍 Filters lines with inet and prints the 2nd field (IP address).

## 2. Add a header before IP list
```bash
ifconfig | awk 'BEGIN{print "===IP Add==="} /inet /{print $2}'
```
- ✅ Prints a header before showing IP addresses.

## 3. Add header and footer around IP list
```bash
ifconfig | awk 'BEGIN{print "===IP Add==="} /inet /{print $2} END{print "============="}'
```
- ✅ Adds both a header and a footer to the output.

## 4. Simple echo
```bash
echo "one two three four"
```
- ➡️ Just prints the line one two three four.

## 5. AWK BEGIN only
```bash
echo "one two three four" | awk 'BEGIN{print "demo"}
```
- ✅ Prints "demo" (from BEGIN).

- 🛑 Does not print input, because there's no main block.


## 6. AWK END only
```bash
echo "one two three four" | awk 'END{print "demo"}'
```
- ✅ Prints "demo" after reading the input.

- 🛑 Skips printing the input line.

## 7. AWK with all three blocks
```bash
awk 'BEGIN{print "demo"} {print "demo1"} END{print "demo2"}' Desktop/anaconda.log
```
- Prints:

`"demo"` (once, at start)

`"demo1"` for each line

`"demo2" `at the end

## 8. Print full file with header and footer
```bash
awk 'BEGIN{print "======Password File======="} {print $0} END{print "======```
================="}' Desktop/d1/passwd
```
- ✅ Adds a header before printing the full file and a footer after.

## 9. Echo with BEGIN, main, and END
```bash
echo "one two three four" | awk 'BEGIN{print "===START BEGIN==="} {print $0} END{print "====EN```
D PART===="}'
```
- Output:

"===START BEGIN==="

one two three four

"====END PART===="
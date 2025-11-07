# Useful Shell Commands and File Samples

## 🧾 `user.txt` — Character Class Matching Examples

```bash
cat user.txt
```
# Match uppercase letters
```bash
grep "[[:upper:]]" user.txt
```
```bash
grep "[A-Z]" user.txt 
```

# Match lowercase letters
```bash
grep "[[:lower:]]" user.txt
```
```bash
grep "[a-z]" user.txt
```

# Match alphabetic characters (both cases)
```bash
grep "[[:alpha:]]" user.txt
```
```bash
grep "[A_Z,a-z]" user.txt  # Incorrect syntax; use [:alpha:]
```


# Match digits

```bash
grep "[[:digit:]]" user.txt
```
```bash
grep "[0-9]" user.txt
```

# Match alphanumeric characters
```bash
grep "[[:alnum:]]" user.txt
```

```bash
grep "[A-Z,a-z,0-9]" user.txt  # Incorrect syntax; use [:alnum:]
```



# Match whitespace characters (spaces, tabs, etc.)
```bash
grep "[[:space:]]" user.txt
```
## 🌐 `domain-name.txt` (List of Domains)
```bash
vim domain-name.txt
# Content:
example.com
openai.com
github.com
wikipedia.org
nytimes.com
bbc.com
```
## 🔗 url-list.txt (List of URLs)
```bash
vim url-list.txt
# Content:
https://example.com/about
https://example.com/contact
https://example.com/blog
https://openai.com/research
https://openai.com/blog/chatgpt
https://openai.com/api/pricing
https://github.com/features
https://github.com/login
https://github.com/explore
https://wikipedia.org/wiki/Linux
https://wikipedia.org/wiki/Artificial_intelligence
https://wikipedia.org/wiki/OpenAI
https://nytimes.com/section/technology
https://nytimes.com/section/world
https://nytimes.com/interactive/2024/technology/ai-chatbots.html

```
### ✅ Find URLs that match the domain list
```bash
grep -f domain-name.txt url-list.txt
```
## 📄 my-csv.csv — User Data

```bash
vim my-csv.csv
# Content:
id,firstname,lastname,email
1,Alice,Walker,alice.walker@example.com
2,James,Reed,james.reed@example.com
3,Olivia,Hughes,olivia.hughes@example.com
4,Ethan,Grant,ethan.grant@example.com
5,Chloe,Bennett,chloe.bennett@example.com
6,Lucas,Anderson,lucas.anderson@example.com
7,Sophia,Wells,sophia.wells@example.com
8,Noah,Carter,noah.carter@example.com
9,Emily,Cooper,emily.cooper@example.com
10,Liam,Price,liam.price@example.com
11,Grace,Hamilton,grace.hamilton@example.com
12,Henry,Lee,henry.lee@example.com
13,Mia,Brooks,mia.brooks@example.com

```
## ✂️ Extract CSV Fields Using cut
```bash
cut -d , -f1 my-csv.csv        # IDs only
cut -d , -f2 my-csv.csv        # First names only
cut -d , -f2,3 my-csv.csv      # First and last names
cut -d , -f1,4 my-csv.csv > my-csv2.txt  # ID and Email to another file
```
## 🔐 Extract Usernames from /etc/passwd
```bash
cut -d : -f1,6 passwd  # Username and home directory
cut -d : -f1 passwd    # Just the usernames
```
## 🌐 Network Info with ifconfig

```bash
ifconfig | grep 'inet '                      # List inet addresses
ifconfig | grep 'inet ' | cut -d ' ' -f 10   # Extract just the IPs (depends on interface output)
ifconfig | grep 'flags' | cut -d ' ' -f1     # Extract interface flags

```
# AWK Command Examples

## Example 1:  
Print header, full line, and footer.
```bash
echo "one two three four" | awk 'BEGIN{print "====BEGIN PART===="} {print $0} END{print "====END PART===="}'
```
---
## Example 2:  
Print header, first field, and footer.
```bash
echo "one two three four" | awk 'BEGIN{print "====BEGIN PART===="} {print $1} END{print "====END PART===="}'
```
--- 
## Example 3:  
Print header, second field, and footer.
```bash
echo "one two three four" | awk 'BEGIN{print "====BEGIN PART===="} {print $2} END{print "====END PART===="}'
```
---
## Example 4:  
Print entire input line.
```bash
echo "one two three four" | awk '{print $0}'
```
---
## Example 5:  
Print first word of input.
```bash
echo "one two three four" | awk '{print $1}'
```
---
## Example 6:  
Print third word of input.

```bash
echo "one two three four" | awk '{print $3}'
```
--- 

## Example 7:  
Print first and second words.

```bash
echo "one two three four" | awk '{print $1,$2}'
```
---

## Example 8:  
Print first word, a slash, then second word.

```bash
echo "one two three four" | awk '{print $1 " /" $2}'
```
---
## Example 9:  
Print first word, literal `\n`, then second word.

```bash
echo "one two three four" | awk '{print $1 " \n" $2}'
```
---
## Example 10:  
Print full input line.

```bash
echo "Infosec Warrior" | awk '{print $0}'
```
---

## Example 11:  
Replace first word with "INFOSEC" and print first two words.
```bash
echo "Infosec Warrior" | awk '{$1="INFOSEC"; print $1,$2}'
```
---
## Example 12:  
AWK script to print header, first field and sixth field, then footer.

```bash
vim test.txt
BEGIN{

		print "password file"
	}
{

	print $1, "Home at", $6

}
END {

print "END passwd file"
}
```
---
## Example 13:  
Run AWK script with colon as field separator on `/etc/passwd`.

```bash
awk -f test.txt -F : /etc/passwd
```
---

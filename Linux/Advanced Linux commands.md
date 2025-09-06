🐧 AWK Command Cheat Sheet
🔹 What is awk?

awk = A text processing tool in Linux.

Works on patterns and actions:

awk 'pattern { action }' file


$0 = whole line, $1 = 1st column, $2 = 2nd column, etc.

🔹 Basic Usage
1. Print entire file
awk '{print}' file.txt

2. Print the first column
awk '{print $1}' file.txt

3. Print multiple columns (1st & 3rd)
awk '{print $1, $3}' file.txt

4. Print line numbers with content
awk '{print NR, $0}' file.txt


NR → line number

🔹 Filtering
5. Print lines containing a word
awk '/error/ {print}' logfile.log

6. Print lines where column 2 > 50
awk '$2 > 50 {print $0}' data.txt

7. Print lines where column 3 = "yes"
awk '$3 == "yes" {print $0}' data.txt

🔹 Arithmetic
8. Add all numbers in column 2
awk '{sum += $2} END {print sum}' data.txt

9. Average of column 3
awk '{sum += $3} END {print sum/NR}' data.txt

10. Multiply two columns
awk '{print $1 * $2}' data.txt

🔹 Built-in Variables
Variable	Meaning
$0	Whole line
$1,$2	1st, 2nd column
NR	Current line number
NF	Number of fields in line
FS	Field separator (default = space)
OFS	Output field separator
RS	Record separator (default = newline)
11. Print number of columns in each row
awk '{print NF}' file.txt

12. Change field separator (CSV file)
awk -F, '{print $1,$2}' file.csv

13. Change output field separator
awk 'BEGIN {OFS=" - "} {print $1,$2}' file.txt

🔹 Advanced
14. Print only the first 3 lines
awk 'NR < 4 {print}' file.txt

15. Skip header row (start from 2nd line)
awk 'NR > 1 {print}' file.txt

16. Print last column of each row
awk '{print $NF}' file.txt

17. Replace a word (foo → bar)
awk '{gsub("foo","bar"); print}' file.txt

18. Print only unique values from 1st column
awk '!seen[$1]++' file.txt

🔹 With BEGIN & END
19. Print header + data
awk 'BEGIN {print "Name Age"} {print $1,$2}' file.txt

20. Print total number of lines
awk 'END {print NR}' file.txt

21. Print sum with header
awk 'BEGIN {sum=0} {sum += $2} END {print "Total:", sum}' file.txt


✅ DevOps Use Cases:

Log filtering (kubectl logs | awk ...)

CSV/JSON parsing

Quick reporting from configs

Automating data cleanup

Would you like me to merge this AWK cheat sheet into the big Linux Commands README.md file I started for you earlier, so you get one master guide?

You said:
SED
ChatGPT said:

Nice pick 🔥 — after awk, the other giant in Linux text processing is sed.
Let’s build you a full SED Cheat Sheet with explanations and examples.

🐧 SED Command Cheat Sheet
🔹 What is sed?

sed = Stream EDitor

Used to search, find, replace, delete, and transform text in files.

Works line by line, great for automation.

Basic syntax:

sed 'command' filename

🔹 Basic Usage
1. Print file (default behavior)
sed '' file.txt

2. Replace first occurrence of "foo" with "bar"
sed 's/foo/bar/' file.txt

3. Replace all occurrences of "foo" with "bar"
sed 's/foo/bar/g' file.txt

4. Replace only on line 3
sed '3s/foo/bar/' file.txt

5. Replace between lines 2 and 5
sed '2,5s/foo/bar/g' file.txt

🔹 Deleting
6. Delete line 2
sed '2d' file.txt

7. Delete lines 2–5
sed '2,5d' file.txt

8. Delete all empty lines
sed '/^$/d' file.txt

🔹 Printing
9. Print only line 3
sed -n '3p' file.txt

10. Print lines 5 to 10
sed -n '5,10p' file.txt

11. Print lines matching "error"
sed -n '/error/p' logfile.log

🔹 In-Place Editing
12. Replace text inside the file directly
sed -i 's/foo/bar/g' file.txt

13. Backup original before replacing
sed -i.bak 's/foo/bar/g' file.txt

🔹 Advanced Usage
14. Replace only the second occurrence on each line
sed 's/foo/bar/2' file.txt

15. Replace on lines starting with "Error"
sed '/^Error/s/foo/bar/' file.txt

16. Insert a line before line 3
sed '3i\This is a new line' file.txt

17. Append a line after line 3
sed '3a\This is appended' file.txt

18. Change line 2 completely
sed '2c\This is the new content' file.txt

19. Replace with environment variable (bash expansion)
sed "s/foo/$USER/" file.txt

20. Multiple commands in one go
sed -e 's/foo/bar/g' -e 's/yes/no/g' file.txt

🔹 Common DevOps Use Cases

Mass config edits (sed -i 's/dev/prod/g' config.yaml)

Remove unwanted lines from logs

Add headers/footers to files

Automate replacements in Dockerfiles, YAML, JSON

👉 Think of awk as "Excel formulas for text" and sed as "Find & Replace on steroids".

Do you want me to also merge AWK + SED into your Linux Commands README.md (the big file we’re building), so you’ll have a single master DevOps Linux guide?

You said:
grep 
ChatGPT said:

Ahh perfect 🐧 — you’ve asked about grep, which is literally one of the most-used commands in Linux for DevOps, SRE, and debugging.

🔎 GREP Command Cheat Sheet
🔹 What is grep?

grep = Global Regular Expression Print

Used to search for patterns/strings in files or output.

Supports regex, making it super powerful.

Basic syntax:

grep [options] pattern filename

🔹 Basic Usage
1. Search for a word in a file
grep "error" logfile.log

2. Case-insensitive search
grep -i "error" logfile.log

3. Search recursively in all files of a directory
grep -r "error" /var/log/

4. Count matches
grep -c "error" logfile.log

5. Show line numbers with matches
grep -n "error" logfile.log

🔹 Filtering
6. Show only matching part of the line
grep -o "error" logfile.log

7. Show lines NOT matching pattern
grep -v "error" logfile.log

8. Match multiple patterns
grep -E "error|warning" logfile.log

🔹 Regex Examples
9. Lines starting with "Error"
grep "^Error" logfile.log

10. Lines ending with ".log"
grep "\.log$" logfile.log

11. Match digits
grep "[0-9]" logfile.log

12. Match whole word only
grep -w "error" logfile.log

🔹 Advanced
13. Search in multiple files
grep "error" file1.txt file2.txt

14. Highlight matches (default in many distros)
grep --color=auto "error" logfile.log

15. Use with pipes (common in DevOps)
dmesg | grep "usb"

kubectl logs pod-name | grep "error"

16. Show 2 lines before and after match
grep -C 2 "error" logfile.log

17. Show 3 lines before match
grep -B 3 "error" logfile.log

18. Show 3 lines after match
grep -A 3 "error" logfile.log

🔹 Common DevOps Use Cases

Log monitoring (grep -i "fail" /var/log/syslog)

Debugging Kubernetes/Docker logs (kubectl logs | grep timeout)

Filtering config files for keywords

Quick validation in CI/CD pipelines

👉 Shortcut in one word:
grep = search 🔍


-------------------------------------------------------------------------------------------------------------------------------------------------------------------


# 🐧 Linux Text Processing Commands (AWK, SED, GREP)

This guide covers **three powerful Linux tools** — `awk`, `sed`, and `grep`.  
These are essential for DevOps, automation, and log analysis.  

---

# 🔎 GREP (Search Tool)

## What is grep?
- **`grep` = Global Regular Expression Print**  
- Used to **search for patterns/strings in files or output**.  
- Supports **regex** for advanced filtering.  

### Basic Usage
```bash
grep "error" logfile.log        # Search word
grep -i "error" logfile.log     # Case-insensitive
grep -r "error" /var/log/       # Recursive search
grep -c "error" logfile.log     # Count matches
grep -n "error" logfile.log     # Show line numbers



Filtering
grep -o "error" logfile.log     # Show only matching word
grep -v "error" logfile.log     # Invert match
grep -E "error|warning" file    # Multiple patterns

Regex
grep "^Error" logfile.log       # Starts with Error
grep "\.log$" logfile.log       # Ends with .log
grep "[0-9]" logfile.log        # Any digit
grep -w "error" logfile.log     # Whole word only

Advanced
grep --color=auto "error" logfile.log   # Highlight matches
dmesg | grep "usb"                      # With pipes
grep -C 2 "error" logfile.log           # 2 lines before & after
grep -B 3 "error" logfile.log           # 3 lines before
grep -A 3 "error" logfile.log           # 3 lines after


👉 Shortcut word: grep = search

🐧 SED (Stream Editor)
What is sed?

sed = Stream EDitor

Used to find, replace, delete, and edit text in files.

Works line by line, perfect for automation.

Basic Replace
sed 's/foo/bar/' file.txt       # Replace first occurrence
sed 's/foo/bar/g' file.txt      # Replace all occurrences
sed '3s/foo/bar/' file.txt      # Replace only on line 3
sed '2,5s/foo/bar/g' file.txt   # Replace between lines 2–5

Deleting
sed '2d' file.txt               # Delete line 2
sed '2,5d' file.txt             # Delete lines 2–5
sed '/^$/d' file.txt            # Delete empty lines

Printing
sed -n '3p' file.txt            # Print line 3
sed -n '5,10p' file.txt         # Print lines 5–10
sed -n '/error/p' logfile.log   # Print lines with "error"

In-Place Editing
sed -i 's/foo/bar/g' file.txt       # Edit in file
sed -i.bak 's/foo/bar/g' file.txt   # Backup original

Advanced
sed 's/foo/bar/2' file.txt          # Replace 2nd occurrence only
sed '/^Error/s/foo/bar/' file.txt   # Replace only on lines starting with Error
sed '3i\This is new' file.txt       # Insert before line 3
sed '3a\This is appended' file.txt  # Append after line 3
sed '2c\This line replaced' file.txt # Change line 2 completely
sed "s/foo/$USER/" file.txt         # Replace with env variable
sed -e 's/foo/bar/g' -e 's/yes/no/g' file.txt  # Multiple replacements


👉 Shortcut word: sed = find & replace

🐧 AWK (Text Processor)
What is awk?

awk = A text processing tool

Used for searching, filtering, and processing structured text.

Treats each line as a record, and fields as columns.

Basic Usage
awk '{print}' file.txt           # Print entire file
awk '{print $1}' file.txt        # Print first column
awk '{print $1, $3}' file.txt    # Print multiple columns
awk '{print NR, $0}' file.txt    # Print line numbers

Filtering
awk '/error/ {print}' logfile.log         # Print lines with "error"
awk '$2 > 50 {print $0}' data.txt         # Column 2 greater than 50
awk '$3 == "yes" {print $0}' data.txt     # Column 3 equals "yes"

Arithmetic
awk '{sum += $2} END {print sum}' data.txt     # Sum of column 2
awk '{sum += $3} END {print sum/NR}' data.txt  # Average of column 3
awk '{print $1 * $2}' data.txt                 # Multiply 2 columns

Built-in Variables
Variable	Meaning
$0	Whole line
$1,$2	First, second column
NR	Line number
NF	Number of fields
FS	Field separator
OFS	Output field separator

Examples:

awk '{print NF}' file.txt        # Number of columns
awk -F, '{print $1,$2}' file.csv # Change field separator (CSV)
awk 'BEGIN {OFS=" - "} {print $1,$2}' file.txt # Custom output separator

Advanced
awk 'NR < 4 {print}' file.txt                # First 3 lines
awk 'NR > 1 {print}' file.txt                # Skip header row
awk '{print $NF}' file.txt                   # Last column
awk '{gsub("foo","bar"); print}' file.txt    # Replace word
awk '!seen[$1]++' file.txt                   # Unique values from column 1

BEGIN & END
awk 'BEGIN {print "Name Age"} {print $1,$2}' file.txt  # Header + data
awk 'END {print NR}' file.txt                          # Total lines
awk 'BEGIN {sum=0} {sum+=$2} END {print "Total:", sum}' file.txt


👉 Shortcut word: awk = text processor

🚀 DevOps Use Cases

grep → Search logs/configs quickly

sed → Batch replace or delete in configs/logs

awk → Generate reports, parse structured text, compute sums

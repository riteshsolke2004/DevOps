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

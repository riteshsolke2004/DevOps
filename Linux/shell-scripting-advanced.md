# 🐧 Advanced Shell Scripting Notes

This document covers advanced shell scripting concepts with **examples**.  
Use this for quick reference in your DevOps journey 🚀.

---

## 1. Arrays

Store multiple values inside a single variable.

```bash
#!/bin/bash
fruits=("apple" "banana" "cherry")

echo "First: ${fruits[0]}"
echo "All: ${fruits[@]}"

Another example:

nums=(10 20 30 40)
for n in "${nums[@]}"; do
  echo "Number: $n"
done

2. String Manipulation

Perform substring, length, replace.

#!/bin/bash
str="DevOpsJourney"

echo "Length: ${#str}"
echo "Substring (0-5): ${str:0:5}"
echo "Replace: ${str/DevOps/Cloud}"


Another example:

filename="report.log"
echo "Extension: ${filename##*.}"
echo "Base name: ${filename%.*}"

3. Case Statements

Cleaner than multiple if-else.

#!/bin/bash
echo "Enter a choice (start/stop/restart): "
read choice

case $choice in
  start) echo "Starting service..." ;;
  stop) echo "Stopping service..." ;;
  restart) echo "Restarting service..." ;;
  *) echo "Invalid choice" ;;
esac


Another example:

#!/bin/bash
echo "Enter day (Mon/Tue): "
read day
case $day in
  Mon) echo "Start of week" ;;
  Tue) echo "Second day" ;;
  *) echo "Other day" ;;
esac

4. Loops with Files

Read files line by line.

#!/bin/bash
while read line; do
  echo "Line: $line"
done < myfile.txt


Another example:

#!/bin/bash
for file in *.txt; do
  echo "Processing $file"
done

5. Command Substitution

Store command output in variables.

#!/bin/bash
date_today=$(date +%Y-%m-%d)
echo "Today is $date_today"


Another example:

#!/bin/bash
count=$(ls | wc -l)
echo "Files in directory: $count"

6. Exit Status & Error Handling

Use $? to check last command status.

#!/bin/bash
ls /notfound 2>/dev/null

if [ $? -ne 0 ]; then
  echo "Error: Directory not found!"
fi


Another example:

#!/bin/bash
ping -c 1 google.com
if [ $? -eq 0 ]; then
  echo "Internet available"
else
  echo "No internet"
fi

7. Functions with Return Codes

Encapsulate logic in reusable functions.

#!/bin/bash
check_even() {
  if (( $1 % 2 == 0 )); then
    return 0
  else
    return 1
  fi
}

check_even 4
if [ $? -eq 0 ]; then
  echo "Even number"
else
  echo "Odd number"
fi


Another example:

#!/bin/bash
greet() {
  echo "Hello, $1!"
}

greet "Ritu"

8. Traps & Signals

Handle signals like CTRL+C.

#!/bin/bash
trap "echo 'You pressed CTRL+C! Exiting...'; exit" SIGINT

while true; do
  echo "Running..."
  sleep 2
done


Another example:

#!/bin/bash
trap "echo 'Script terminated!'; exit" SIGTERM

while true; do
  sleep 5
done

9. Debugging Scripts

Enable debugging with set -x.

#!/bin/bash
set -x
echo "Debugging this line"
set +x


Another example:

bash -x myscript.sh

10. Scheduling & Automation

Automate tasks with cron.

crontab -e
# Run backup.sh every day at midnight
0 0 * * * /home/user/backup.sh


Another example:

crontab -e
# Run cleanup.sh every 5 minutes
*/5 * * * * /home/user/cleanup.sh

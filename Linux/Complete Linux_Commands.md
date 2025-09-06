# 🐧 Linux Commands Cheat Sheet (Basic → Advanced)

A complete guide of **Linux commands** from basic to advanced.  
Great for **beginners, DevOps engineers, and system administrators** 🚀  

---

## 📂 1. File & Directory Management

### Basic
```bash
pwd              # Show current directory
ls               # List files
ls -l            # Detailed list
ls -a            # Show hidden files
cd /path         # Change directory
mkdir newdir     # Create new directory
rmdir olddir     # Remove empty directory
touch file.txt   # Create empty file
cp file1 file2   # Copy file
mv file1 file2   # Move/Rename file
rm file.txt      # Delete file



Advanced
tree             # Show directory structure
find / -name file.txt   # Search file by name
du -sh *         # Show disk usage of files/dirs
df -h            # Show free disk space
stat file.txt    # Detailed file info

📝 2. File Viewing & Editing
cat file.txt          # Show file content
less file.txt         # Scroll through file
head -n 10 file.txt   # Show first 10 lines
tail -n 10 file.txt   # Show last 10 lines
tail -f log.txt       # Live monitor logs
nano file.txt         # Edit file (nano editor)
vim file.txt          # Edit file (vim editor)

👥 3. User & Permission Management
whoami              # Show current user
who                 # Show logged-in users
id                  # Show user/group IDs
adduser newuser     # Add new user
passwd newuser      # Change password
su - newuser        # Switch user
groups              # Show groups
chmod 755 file.sh   # Change permissions
chown user:group file.txt   # Change ownership

⚡ 4. Process Management
ps aux             # Show all processes
top                # Real-time process monitor
htop               # Interactive process monitor
kill 1234          # Kill process by PID
kill -9 1234       # Force kill process
jobs               # Show background jobs
bg %1              # Resume job in background
fg %1              # Resume job in foreground

🌐 5. Networking Commands
ping google.com          # Test connectivity
traceroute google.com    # Show packet path
nslookup google.com      # DNS lookup
dig google.com           # Advanced DNS lookup
ifconfig                 # Show interfaces (legacy)
ip addr show             # Show IP addresses
iwconfig                 # Show Wi-Fi settings
ifplugstatus eth0        # Check cable connection
hostname                 # Show hostname
netstat -tulnp           # Show connections
ss -tulnp                # Modern netstat
telnet google.com 80     # Test port connection
nc -zv google.com 443    # Port scan with netcat
nmap 192.168.1.1         # Scan network
arp -a                   # Show IP ↔ MAC mapping
curl -I https://site.com # Get HTTP headers
wget https://file.com    # Download file
whois google.com         # Domain details
iptables -L -v           # Show firewall rules

💾 6. Package Management
Debian/Ubuntu
apt update             # Update package lists
apt upgrade            # Upgrade packages
apt install nginx      # Install package
apt remove nginx       # Remove package

RHEL/CentOS
yum install httpd      # Install package
yum update             # Update packages
yum remove httpd       # Remove package

🖥️ 7. System Information
uname -a          # Kernel and system info
uptime            # System uptime
dmesg | less      # Kernel messages
lscpu             # CPU details
lsblk             # Block devices
free -h           # Memory usage
df -h             # Disk usage
du -sh /home      # Directory size
cat /etc/os-release   # OS version

🔍 8. Search & Filters
grep "text" file.txt        # Search text in file
grep -r "error" /var/log    # Recursive search
sort file.txt               # Sort file
uniq file.txt               # Remove duplicates
wc -l file.txt              # Count lines
cut -d: -f1 /etc/passwd     # Cut by delimiter
awk '{print $1}' file.txt   # Print first column
sed 's/foo/bar/g' file.txt  # Replace text

🗜️ 9. Compression & Archiving
tar -cvf files.tar file1 file2   # Create tar archive
tar -xvf files.tar               # Extract tar archive
gzip file.txt                    # Compress file
gunzip file.txt.gz               # Decompress file
zip archive.zip file1 file2      # Create zip
unzip archive.zip                # Extract zip

🔄 10. Shell & Scripting Basics
Variables
name="Ritu"
echo "Hello $name"

Conditions
if [ -f file.txt ]; then
  echo "File exists"
fi

Loops
for i in {1..5}; do
  echo "Number $i"
done

Functions
myfunc() {
  echo "Hello from function"
}
myfunc

Script Example
#!/bin/bash
echo "System Info:"
uname -a
df -h
free -h

🔧 11. Monitoring & Logs
dmesg | tail         # Kernel messages
journalctl -xe       # System logs
tail -f /var/log/syslog   # Live log monitoring
vmstat 5             # CPU/memory stats every 5 sec
iostat               # Disk I/O stats
watch -n 2 df -h     # Monitor disk usage

⚙️ 12. Advanced Administration
crontab -e              # Edit cron jobs
crontab -l              # List cron jobs
at now + 2 minutes      # Schedule one-time job
systemctl status nginx  # Check service status
systemctl start nginx   # Start service
systemctl stop nginx    # Stop service
systemctl enable nginx  # Enable service at boot

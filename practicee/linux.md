**Linux-Based Theory**

1. **What is UNIX?**
   - UNIX is a multi-user, multitasking operating system designed for efficiency and security. It serves as the foundation for various operating systems like Linux and macOS.

2. **Why do organizations use Linux instead of Windows?**
   - Linux is preferred for its security, open-source nature, cost-effectiveness, flexibility, and reliability compared to Windows.

3. **What is the PPID in Linux?**
   - The PPID (Parent Process ID) is the identifier of the parent process that spawned a child process.

4. **What are links in Linux?**
   - Links in Linux are references to files and can be symbolic (soft) or hard links.

5. **Explain the df and du commands.**
   - `df` displays disk space usage.
   - `du` shows directory/file space consumption.

6. **What are ps, top, and htop commands?**
   - `ps` lists currently running processes.
   - `top` provides real-time system monitoring.
   - `htop` is an interactive process viewer with more features.

7. **What are the first two columns when we run ps?**
   - The first two columns are `PID` (Process ID) and `TTY` (terminal controlling the process).

8. **How do you check how much space is left on a system?**
   - Using the `df -h` command.

9. **What is the free command in Linux?**
   - The `free` command displays memory usage.

10. **What is the use of top and htop commands?**
   - They monitor system performance and resource usage in real time.

11. **What does the df command show? Does it show RAM or hard disk usage?**
   - It shows hard disk usage.

12. **What fields does the df command display?**
   - Filesystem, Size, Used, Available, Use%, Mounted on.

**Linux-Based Commands**

1. **How do you list the top 3 processes in Linux?**
   - `ps aux --sort=-%cpu | head -4`

2. **How do you identify all available users in a database?**
   - `cat /etc/passwd | cut -d: -f1`

3. **How do you find available ports?**
   - `netstat -tuln`

4. **How do you identify host details?**
   - `hostnamectl`

5. **How do you change the directory to the root directory and list all files?**
   - `cd / && ls -al`

6. **Create a file using vi and add two columns separated by space.**
   - `vi file.txt`, then input data and save.

7. **How to get only the second column from the created file?**
   - `awk '{print $2}' file.txt`

8. **How to replace file words with new words without opening a file?**
   - `sed -i 's/oldword/newword/g' file.txt`

9. **How do you find the count of a word (“Wiley”) in a file?**
   - `grep -o 'Wiley' file.txt | wc -l`

10. **Linux command to find a file older than 30 days.**
   - `find /path -type f -mtime +30`

11. **Linux command to retrieve unique values in the 5th column of a CSV file.**
   - `cut -d, -f5 file.csv | sort | uniq`

12. **How do you list files in such a way that the most recent file is at the bottom?**
   - `ls -ltr`

13. **How do you replace all occurrences of a pattern with another in a file?**
   - `sed -i 's/pattern/replacement/g' file.txt`

14. **How do you use grep?**
   - `grep 'pattern' file.txt`

15. **How do you delete lines from 10 to the end of a file?**
   - `sed '10,$d' file.txt`

16. **How do you print from line 10 to the end of a file?**
   - `sed -n '10,$p' file.txt`

17. **Linux command to get the count of processes running by three different users.**
   - `ps -eo user | sort | uniq -c | grep -E 'user1|user2|user3'`

18. **Command to check memory usage.**
   - `free -m`

19. **How do you find background processes running?**
   - `jobs -l`

20. **If a process is running for 24 hours and you need to kill it, how do you do that?**
   - `kill -9 <PID>`

21. **Server not working properly but was fine yesterday, what is your approach?**
   - Check logs (`/var/log`), restart services, and monitor resource usage.


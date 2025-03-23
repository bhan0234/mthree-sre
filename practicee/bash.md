**Linux and SQL Questions & Answers**

**1. How can you distinguish whether fields are comma-separated or pipe-separated in a file?**
   - You can use the `head` or `cat` command along with `grep` or `awk` to check the delimiter:
     ```bash
     head -n 5 filename.txt | grep -o ',' | wc -l
     head -n 5 filename.txt | grep -o '|' | wc -l
     ```
     The output with the higher count indicates the delimiter used in the file.

**2. How do you print the third column from a file with three columns separated by commas?**
   - You can use the `cut` or `awk` command:
     ```bash
     cut -d',' -f3 filename.txt
     awk -F',' '{print $3}' filename.txt
     ```

**3. Given a file, how do you replace a word?**
   - You can use the `sed` command:
     ```bash
     sed -i 's/oldword/newword/g' filename.txt
     ```
     This replaces all occurrences of "oldword" with "newword" in the file.

**4. Command to get the second occurrence of your name in a file.**
   - You can use `awk` to extract the second occurrence:
     ```bash
     awk '/yourname/{count++; if(count==2) print $0}' filename.txt
     ```

**5. How do you get the total count of occurrences of your name in a file?**
   - You can use the `grep` or `awk` command:
     ```bash
     grep -o 'yourname' filename.txt | wc -l
     awk '{count += gsub(/yourname/, "&")} END {print count}' filename.txt
     ```


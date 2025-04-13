# SED Command Reference Guide (Beginner to Advanced)

## Beginner Level Commands

```bash
sed -e '/^#/ d' my_file
```
Delete all lines starting with `#` (comments).

```bash
sed '/^$/d' myfile2.txt
```
Delete empty lines.

```bash
sed '5d' myfile2.txt
```
Delete line number 5.

```bash
sed -n '1,2p' myfile.txt
```
Print only lines 1 and 2.

```bash
sed -e '1,100 command' my_file
```
Apply command from line 1 to 100.

```bash
sed '3,6d' file.txt
```
Delete lines 3 to 6.

```bash
sed -n '3p' file.txt
```
Print only line 3.

```bash
sed -n '7p;9p' file.txt
```
Print lines 7 and 9.

```bash
sed -e 's/input/output/' my_file
```
Replace only the first occurrence of `input` with `output`.

```bash
sed 's/world/sed/g' myfile2.txt
```
Replace all occurrences of `world` with `sed` and display result.

## Intermediate Level Commands

```bash
sed 's/world/sed/g' myfile2.txt > mynewfile.txt
```
Save replaced output to a new file.

```bash
sed -i 's/world/sed/g' myfile2.text
```
Replace `world` with `sed` permanently in the file.

```bash
sed '2s/world/sed/' myfile2.txt
```
Replace only on line 2.

```bash
sed -e 's/\/bin/\/usr\/local\/bin/' my_script > new_script
```
Replace `/bin` with `/usr/local/bin` and save to a new file.

```bash
sed -e '2s/.*/This is the new line 2/' -e '5d' myfile2.txt
```
Modify line 2 and delete line 5.

```bash
sed -e 's/foo/bar/' -e 's/hello/hi/' myfile.txt
```
Chain multiple replacements.

```bash
sed -e 's/[0-9]*/(&)/' my_file
```
Surround digits with brackets.

```bash
sed 's/hello/& friend/' sample.txt
```
Append "friend" after "hello".

```bash
sed 's/hello/HEY &/' sample.txt
```
Insert "HEY" before "hello".

```bash
sed 's/$/ APPENDED/' sample.txt
```
Append text at the end of every line.

```bash
sed '/hello/  s/$/ APPENDED/' sample.txt
```
Append text if line contains "hello".

```bash
sed -n -e '/^d/ p'
```
Print lines that begin with "d".

```bash
sed -e '11,$ d' my_file
```
Delete all lines from line 11 onwards.

## Advanced Level Commands

```bash
sed '/banana/i\grape' example.txt
```
Insert a line with "grape" before a line containing "banana".

```bash
sed '/banana/a\grape' example.txt
```
Append a line with "grape" after a line containing "banana".

```bash
sed '3i This is a new line' file.txt
```
Insert "This is a new line" before line 3.

```bash
sed '2a This comes after line 2' file.txt
```
Append "This comes after line 2" after line 2.

```bash
sed '4c newline' filename
```
Replace line 4 with "newline".

```bash
sed -n -e '/boot$/,/mach/p' a_file
```
Print lines from the one ending with "boot" to the one containing "mach".


**Command Flags Reference**

- `g` → Replace all matches in line
- `1, 2, etc.` → Replace Nth match only (e.g., `s/foo/bar/2`)
- `p` → Print only if substitution was made
- `w file` → Write changed lines to a new file
- `I` → Case-insensitive match (GNU sed)
- `e` → Execute replacement as a command (GNU sed)

General Uses of `sed`

- Search & replace
- Print specific lines
- Delete lines
- Insert or append lines
- Replace entire line
- Modify based on pattern match
- Chain multiple actions

### 🧠 HackerRank Special Sed Examples

#### 1. Reverse number groups:

```bash
sed -E 's/([0-9]{4}) ([0-9]{4}) ([0-9]{4}) ([0-9]{4})/\4 \3 \2 \1/'
```

#### 2. Mask all but last group:

```bash
sed -E 's/([0-9]{4}) ([0-9]{4}) ([0-9]{4}) ([0-9]{4})/**** **** **** \4/g'
```

#### 3. Surround 'thy' with curly braces (case-insensitive):

```bash
sed -E 's/\bthy\b/{&}/gI'
```

#### 4. Using group reference:

```bash
sed -E 's/(thy)/{\1}/gI'
s/thy/{&}/Ig    - & is a placeholder
sed -E 's/\bthy\b/{&}/gI'   -     \b: word boundary (works in GNU sed)
```

# AWK Command Reference Guide (Easy to Hard)

## Basic Printing and Output

```bash
awk '{print $0}' myfile.txt
```
Prints each line as-is. Equivalent to `cat myfile.txt`.

```bash
awk '{print $1, $2}' myfile.txt
```
Prints the first and second columns.

```bash
awk '{print $1 "\t" $2}' myfile.txt
awk '{print $1 "    " $2}' file1.txt
```
Prints first and second columns with spacing in between.

```bash
awk '{print NR, $0}' myfile.txt
```
Prints line number followed by the content.

## Field Separators and Format

```bash
awk -F',' '{print $1, $2}' myfile.txt
```
Specifies comma as field separator.

```bash
awk 'BEGIN{FS=":"; OFS="-"} {print $1,$6,$7}' /etc/passwd
```
Changes input separator to ':' and output separator to '-'.

```bash
awk 'BEGIN { FS=":"; OFS="-" } { print $0 }' /etc/passwd
```
Prints the entire line as-is with custom FS and OFS.

## Filtering and Matching Patterns

```bash
awk '/great/ {print}' myfile.txt
```
Prints lines containing the word "great".

```bash
awk '!/success/' data.txt
```
Prints lines that do NOT contain the word "success".

```bash
awk '$0 ~ /world/ {print $0}' myfile.txt
```
Prints lines where the entire line contains "world".

```bash
awk '{ if($1 == "world") print $0 }' myfile.txt
```
Prints line if the first column equals "world".

```bash
awk '$3 == "FAIL" && $2 == "user1" { print $0 }' data.txt
```
Prints lines where the 3rd column is FAIL and 2nd is user1.

```bash
awk '$2 > 50 { print $1, $2 }' scores.txt
```
Prints lines where the second column is greater than 50.

## Range and Skipping

```bash
awk 'NR==2, NR==4 {print NR, $0}' myfile.txt
```
Prints lines from 2 to 4 (inclusive).

```bash
awk -F , 'NR>1{print $2}' filename
```
Skips the first line and prints the second column.

```bash
awk 'NR == 1 { getline } { print $0 }' file.txt
```
Skips the first line (header) and prints the rest.

```bash
awk 'BEGIN{count=0} {count++} END {print "Total lines:", count}' file
```
Counts total lines using END block.

## Looping and Counting

```bash
awk '{for(i=1;i<=NF;i++) if($i=="word") count++} END{print count}' file
```
Counts the number of times "word" appears in a file.

## Getline and Line Manipulation

```bash
awk '{ getline; print $0 }' file.txt
```
Skips to the next line before printing.

```bash
awk 'NR == 1 { getline } { print $0 }' file.txt
```
Skips the first line and prints the rest.

```bash
ls -l | awk '{for (i=1;i<3;i++) {getline}; print NR,$0}'
```
Skips two lines and prints every third line.

```bash
awk -v skip=3 '{for (i=1;i<skip;i++) {getline}; print $0}' a_file
```
Skips a customizable number of lines using a variable.

## Summing and Reporting

```bash
ls -l | awk 'BEGIN {sum=0} {sum=sum+$5} END {print sum}'
```
Sums up the file sizes in a directory.

```bash
awk -F',' '{sum += $2;} END{print sum;}' myfile.txt
```
Sums up values in the second column.

## Script Execution

```bash
awk -f myscript.awk input.txt
```
Executes an AWK script from a file on the given input.

# GREP Command Reference Guide (Beginner to Advanced)

## Basic GREP Usage

```bash
grep -l "boo" *
```
Search for the pattern "boo" in all files in the current directory. Outputs filenames with matches.

```bash
grep -rl "boo" .
```
Recursively search for "boo" starting from the current directory. Outputs filenames with matches.

```bash
grep "ma" namelist.txt
```
Case-sensitive search for "ma" in `namelist.txt`.

```bash
grep -i "ma" namelist.txt
```
Case-insensitive search for "ma".

```bash
grep -c "ma" namelist.txt
```
Count the number of lines containing "ma".

```bash
grep -n "boo" a_file
```
Display matching lines and their line numbers.

```bash
grep -rl "404" /var/log
```
Recursively list files in `/var/log` where "404" is found.

```bash
grep -r "404" /var/log
```
Recursively print actual lines in `/var/log` containing "404".

```bash
grep GOOG stock_investments.txt | wc -l
```
Count the number of lines containing "GOOG".

```bash
grep -wc T stock_investments.txt
```
Count the exact word "T" in the file.

```bash
grep -o "word" filename | wc -l
```
Count total occurrences of "word" (not full lines).

```bash
grep -lir spy .
```
List files recursively (`-r`) and case-insensitively (`-i`) containing "spy".

```bash
grep -lvir spy .
```
List files that **do not** contain "spy" (reverse match with `-v`).

```bash
grep -x "boo" a_file
```
Match only lines where the entire line is exactly "boo".

```bash
grep -A2 "mach" a_file
```
Print lines containing "mach" and 2 lines **after** each match.

```bash
grep -o -b "pattern" file
```
Print only the matched pattern and its byte offset in the file.

## Context Options Summary

| Option | Description               |
|--------|---------------------------|
| `-A N` | N lines **After** the match |
| `-B N` | N lines **Before** the match |
| `-C N` | N lines Before & After     |

## Extended GREP (egrep / grep -E)

```bash
egrep "cat|dog" animals.txt
```
Search for either "cat" or "dog".

```bash
grep -E "cat|dog" animals.txt
```
Same as above using `-E` flag.

```bash
echo "gogogogo go gooo" | grep -E "(go){2,4}"
```
Match "go" repeated 2 to 4 times.

```bash
echo "gogogogo go gooo" | grep "\(go\)\{2,4\}"
```
Same as above using basic grep with escaped regex.

## Fixed GREP (fgrep / grep -F)

```bash
grep -F "a.b" file.txt
```
Search for the literal string "a.b" (dot not treated as regex).

```bash
fgrep "a.b" file.txt
```
Equivalent command using deprecated `fgrep`.

## Zipped Files: zgrep

```bash
zgrep "error" logs.gz
```
Search for "error" in a `.gz` compressed file.

```bash
zgrep -i "timeout" server_logs.gz
```
Case-insensitive search for "timeout" inside a `.gz` compressed log file.

# GREP & AWK: HackerRank Problem Commands

## GREP - A (Matching Specific Words)

```bash
grep -Ei '\b(the|that|then|those)\b'
```
Case-insensitive (`-i`) match for exact whole words `the`, `that`, `then`, or `those` using word boundaries (`\b`).

```bash
grep -Ewi 'the|that|then|those'
```
Case-insensitive (`-i`) and whole word (`-w`) match for any of the listed words.

---

## GREP - B (Backreferences)

```bash
grep '\([0-9]\)\s*\1'
```
Finds repeated digit patterns with optional whitespace between them using backreferences.

---

## AWK - 1 (Check Missing Scores)

```bash
awk '{ if( $2 == "" || $3 == "" || $4 == "") print "Not all scores are available for", $1 }'
```
Checks if any of the score fields (2, 3, or 4) are missing and reports the student's name if so.

---

## AWK - 2 (Pass or Fail Based on Scores)

```bash
awk '{
if ( $2 >= 50 && $4 >= 50 && $3 >= 50 ){
   print $1,": Pass"
} else {
   print $1 ,": Fail"
}
}'
```
Checks if all scores are >= 50 to determine pass/fail.

---

## AWK - 3 (Grading System Based on Average)

```bash
awk '
BEGIN {
  print "Name Score1 Score2 Score3 : Grade"
}
{
  total = 0
  for (i = 2; i <= 4; i++) {
    total += $i
  }
  avg = total / 3

  if (avg >= 80) {
    grade = "A"
  } else if (avg >= 60) {
    grade = "B"
  } else if (avg >= 50) {
    grade = "C"
  } else {
    grade = "FAIL"
  }

  print $1, $2, $3, $4, ":", grade
}' scores.txt
```
Grades students based on the average of three scores and prints results in a formatted table.

---

## AWK - 4 (Merge Every Two Lines)

### Version 1 (Using Variable Storage)
```bash
awk '{
if(NR % 2 != 0){
  line = $0
} else {
  print line ";" $0
}
}'
```
Merges every two lines with a semicolon. Stores the odd line and prints it along with the next (even) line.

### Version 2 (Controlling ORS)
```bash
awk '{ if (NR % 2) { ORS=";" } else { ORS="\n" } } { print }'
```
Sets the Output Record Separator to `;` or newline depending on the line number.





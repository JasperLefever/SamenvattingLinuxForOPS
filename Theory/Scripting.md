# Scripting

## I/O Redirection

### 1. **Standard Streams**

- **`stdin`**: Standard input, represented as `0`.
- **`stdout`**: Standard output, represented as `1`.
- **`stderr`**: Standard error, represented as `2`.

### 2. **Output Redirection**

- **Redirect stdout to a file**:

  ```bash
  echo "Hello" > file.txt  # Creates or overwrites file.txt with "Hello"
  ```

  Equivalent to:

  ```bash
  echo "Hello" 1> file.txt
  ```

- **Append stdout to a file**:

  ```bash
  echo "More text" >> file.txt
  ```

- **Prevent Overwriting**:
  - Enable `noclobber` to prevent overwriting:
    ```bash
    set -o noclobber
    ```
  - To override `noclobber`, use `>|`:
    ```bash
    echo "Overwrite" >| file.txt
    ```

### 3. **Error Redirection**

- **Redirect stderr to a file**:

  ```bash
  command 2> error.txt
  ```

- **Redirect both stdout and stderr to a file**:

  ```bash
  command > file.txt 2>&1
  ```

- **Order Matters**:
  ```bash
  command > file.txt 2>&1  # Redirects both stdout and stderr to file.txt
  command 2>&1 > file.txt  # Redirects only stdout to file.txt
  ```

> In case of this >file 2>&1 we are doing redirection of stdout (1) to file, but then also telling stderr(2) to be redirected to the same place as stdout ! So the purpose may be the same, but the idea slightly different. In other words "John, go to school; Suzzie go where John goes".

### 4. **Combining stdout and stderr**

- **Direct both to a file**:
  ```bash
  command &> combined.txt
  ```

### 5. **Using Pipes with Redirection**

- **Pipe both stdout and stderr**:
  ```bash
  command 2>&1 | grep "pattern"
  ```

### 6. **Input Redirection**

- **Redirect stdin from a file**:

  ```bash
  command < input.txt
  ```

- **Here Document (`<<`)**: Pass multiple lines to a command until a specific marker.

  ```bash
  cat <<EOF > file.txt
  line1
  line2
  EOF
  ```

- **Here String (`<<<`)**: Pass a single string directly to a command.
  ```bash
  base64 <<< "text to encode"
  ```

### 7. **Quick Ways to Clear a File**

- **Clear a file**:
  ```bash
  > file.txt
  ```
- **Clear a file with `noclobber` set**:
  ```bash
  >| file.txt
  ```

## Filters

### 1. **cat** and **tac**

- **cat**: Used to display the content of a file or concatenate multiple files.
- **tac**: Displays the content of a file in reverse order (line by line).

```bash
# Display the contents of count.txt
cat count.txt

# Concatenate multiple files and display
cat file1.txt file2.txt

# Display count.txt in reverse order
tac count.txt
```

**Example Output**:

```plaintext
# Output of tac count.txt
five
four
three
two
one
```

---

### 2. **head** and **tail**

- **head**: Displays the first few lines of a file (default is 10 lines).
- **tail**: Displays the last few lines of a file.

```bash
# Display the first 3 lines of count.txt
head -3 count.txt

# Display the last 3 lines of count.txt
tail -3 count.txt

# Start displaying count.txt from line 3
tail -n+3 count.txt
```

---

### 3. **sort** and **uniq**

- **sort**: Sorts lines in a file.
- **uniq**: Removes duplicate lines (requires sorted input to work effectively).

```bash
# Sort the contents of a file
sort names.txt

# Sort and remove duplicates, showing occurrences
sort names.txt | uniq -c
```

**Example**:

```plaintext
# Assuming names.txt contains:
Alice
Bob
Alice
Carol

# Output of sort names.txt | uniq -c
  2 Alice
  1 Bob
  1 Carol
```

---

### 4. **grep** (Search Text)

- **grep**: Filters lines based on a search pattern.

```bash
# Find lines with the word "Alice" in names.txt
grep "Alice" names.txt

# Case-insensitive search for "alice"
grep -i "alice" names.txt

# Display lines not containing "Alice"
grep -v "Alice" names.txt
```

**Example Output**:

```plaintext
# Assuming names.txt contains:
Alice
Bob
Carol

# Output of grep -v "Alice" names.txt
Bob
Carol
```

---

### 5. **cut** (Extract Columns)

- **cut**: Extracts specified columns from text based on a delimiter.

```bash
# Extract first and third columns (username and UID) from /etc/passwd
cut -d: -f1,3 /etc/passwd

# Extract first names from a comma-separated file names.csv
cut -d',' -f1 names.csv
```

**Example Output**:

```plaintext
# Assuming names.csv contains:
Alice,25
Bob,30
Carol,22

# Output of cut -d',' -f1 names.csv
Alice
Bob
Carol
```

---

### 6. **tr** (Translate/Transform Characters)

- **tr**: Used for transforming characters in text streams.

```bash
# Convert lowercase letters to uppercase
cat names.txt | tr 'a-z' 'A-Z'

# Replace spaces with underscores
echo "Hello World" | tr ' ' '_'
```

**Example Output**:

```plaintext
# Assuming names.txt contains:
Alice
Bob

# Output of cat names.txt | tr 'a-z' 'A-Z'
ALICE
BOB
```

---

### 7. **sed** (Stream Editor for Substitution)

- **sed**: Used for search and replace operations on text streams.

```bash
# Replace first per line "Alice" with "Alicia" in names.txt
sed 's/Alice/Alicia/' names.txt

# Replace all occurrences of "old" with "new" in a text
echo "old old" | sed 's/old/new/g'

# Delete lines containing "remove" in a file
sed '/remove/d' file.txt
```

**Example Output**:

```plaintext
# Input text: "old old"
# Output of echo "old old" | sed 's/old/new/g'
new new
```

---

### 8. **awk** (Powerful Text Processing Language)

- **awk**: Extracts and processes fields, often used for data extraction and reporting.

```bash
# Print first and third fields of /etc/passwd
awk -F: '{print $1, $3}' /etc/passwd

# Sum numbers in the second column
awk '{sum += $2} END {print sum}' numbers.txt
```

**Example**:

```plaintext
# Assuming numbers.txt contains:
apple 5
banana 3
cherry 8

# Output of awk '{sum += $2} END {print sum}' numbers.txt
16
```

---

### 9. **tee** (Duplicate Output)

- **tee**: Writes the output to both the standard output and a file.

```bash
# Write output to stdout and save to file.txt
echo "Hello" | tee file.txt

# Save output of a command to multiple files
echo "Hello" | tee file1.txt file2.txt
```

**Example Output**:

```plaintext
# Content of file.txt after echo "Hello" | tee file.txt
Hello
```

---

### 10. **comm** (Compare Files Line by Line)

- **comm**: Compares two sorted files and shows their differences.

```bash
# Show lines common to both files
comm -12 file1.txt file2.txt

# Show lines unique to file1.txt
comm -23 file1.txt file2.txt
```

**Example**:

```plaintext
# Assuming file1.txt contains:
Alice
Bob

# And file2.txt contains:
Bob
Carol

# Output of comm -23 file1.txt file2.txt
Alice
```

## Regex

> SEE COURSE

## File globbing

### File Globbing Summary

File globbing is a way to use wildcard patterns to dynamically generate lists of file and directory names in the shell. Globbing relies on special characters (`*`, `?`, `[ ]`) to match patterns across filenames. This summary explains key wildcard characters and the impact of locale settings.

#### Key Wildcard Characters:

1. **Asterisk (`*`)**:

   - Matches zero or more characters.
   - Example: `ls file*` matches any file starting with "file" (e.g., `file1`, `filex`, `fileabc`).

2. **Question Mark (`?`)**:

   - Matches exactly one character.
   - Example: `ls File?` matches files like `File4` and `FileA`.

3. **Square Brackets (`[ ]`)**:
   - Matches any single character listed between brackets.
   - Example: `ls File[5A]` matches `File5` and `FileA`.
   - Ranges can be used (e.g., `[a-z]` for lowercase letters) or to exclude characters (e.g., `[!5]` to exclude `5`).

---

#### Ranges and Character Classes:

- **Character Ranges** (`[a-z]`, `[0-9]`):

  - Define a range for matching characters.
  - Example: `ls file[a-z]*` matches files with lowercase letters following "file".

- **Named Character Classes** (`[[:lower:]]`, `[[:upper:]]`, `[[:digit:]]`):

  - Use predefined character classes for flexible matching.
  - `[[:lower:]]` matches lowercase letters, including international characters (e.g., æ, ø, å) if locale supports it.

  [[:alnum:]], [[:alpha:]], [[:blank:]], [[:cntrl:]], [[:digit:]], [[:graph:]], [[:lower:]], [[:print:]], [[:punct:]], [[:space:]], [[:upper:]], [[:xdigit:]]

---

#### Locale Influence on Globbing:

- **$LANG Variable**:
  - Locale settings (e.g., `en_US.UTF-8`, `da_DK.UTF-8`) affect character matching and sorting.
  - Example: In `en_US.UTF-8`, Danish characters like `æ`, `ø`, and `å` match `[[:lower:]]` and display correctly.

---

#### Preventing Globbing:

- To prevent wildcard expansion, use quotes or escape characters.
  - Examples: `echo "*"`, `echo \*`, or `echo '*'` will print `*` instead of listing files.

## Shell Variables

### Summary: Shell Variables

This chapter provides an overview of how to manage environment variables in the shell, focusing on their creation, use, and manipulation.

#### Key Concepts:

- **Dollar Sign ($):** The dollar sign is used to reference environment variables. The shell replaces `$VAR` with the value of the variable `VAR`. If the variable doesn't exist, it returns nothing.

  Example:

  - `$HOSTNAME` returns the computer's hostname.
  - `$USER` gives the current username.
  - `$HOME` shows the user's home directory.

- **Case Sensitivity:** Shell variables are case-sensitive. `$USER` and `$user` would refer to different variables.

- **Creating Variables:**

  - A variable can be created by simply assigning a value, e.g., `MyVar=555`.
  - Use `echo $MyVar` to print its value.

- **Quotes:**

  - **Double Quotes (""):** Allow variable expansion. For example, `"We are in $city today."` will replace `$city` with its value.
  - **Single Quotes (''):** Prevent variable expansion. `'We are in $city today.'` will literally print `$city`.

- **Viewing Variables:**

  - The `set` command lists all variables, including non-exported ones.
  - `env` lists exported variables and can be used to start a shell with a clean environment (e.g., `env -i bash`).

- **Unsetting Variables:**

  - Use `unset` to remove a variable from the environment. For example, `unset MyVar` will delete `MyVar`.

- **PS1 and Customizing Prompt:**

  - The `$PS1` variable controls the shell prompt, which can be customized using escape sequences (e.g., `\u` for username, `\w` for the current directory).

- **PATH Variable:**

  - The `$PATH` variable contains directories where the shell looks for executable files. Directories are separated by colons.
  - You can add directories to `$PATH`, e.g., `PATH=$PATH:.` to include the current directory.

- **Exporting Variables:**

  - Use the `export` command to make a variable available to child shells. For example, `export var4=four` will make `var4` accessible in a new shell.

- **Delineating Variables:**

  - When using variables next to other text (e.g., `$prefixman`), curly braces `{}` can be used to avoid ambiguity, e.g., `${prefix}man`.

- **Unbound Variables:**
  - If you reference an undefined variable, it will display nothing by default. You can use `set -u` (or `set -o nounset`) to generate an error for unbound variables.

In summary, managing shell variables effectively involves understanding how to create, reference, and manipulate them, customize the shell prompt, and use commands like `env`, `export`, and `unset` to control the environment.

## Intro to scripting

#### **Hello World Script**

```bash
echo "Hello World"
```

- **Make it executable**:
  ```bash
  chmod +x hello_world.sh
  ./hello_world.sh
  ```

---

#### **She-Bang (Interpreter)**

- **Specify the shell interpreter** at the beginning of the script:

  ```bash
  #!/bin/bash
  ```

  - For Python: `#!/usr/bin/env python3`
  - For Korn shell: `#!/bin/ksh`

- **Using `env`** to find the interpreter location:
  ```bash
  #!/usr/bin/env bash
  ```

---

#### **Comments**

- **Single-line comments**:

  ```bash
  # This is a comment
  ```

---

#### **Variables**

- **Using pre-defined variables**:

  ```bash
  echo "Hello ${USER}"
  ```

- **Variable assignment** (no spaces around `=`):

  ```bash
  user="Tux"
  ```

- **Accessing variables**:
  ```bash
  echo "${user}"
  ```

---

#### **Quoting**

- **Double quotes**: Expands variables and supports escape sequences.

  ```bash
  echo "Hello ${USER}"
  ```

- **Single quotes**: Does not expand variables, treated literally.

  ```bash
  echo 'Hello ${USER}'
  ```

- **Quoting for filenames with spaces**:
  ```bash
  file="my file.txt"
  touch "${file}"  # Correct way
  ```

---

#### **Command Substitution**

- **Old style (backticks)**:

  ```bash
  result=`date`
  ```

- **Recommended (using `$()`)**:
  ```bash
  result=$(date)
  ```

---

#### **Sourcing a Script**

- **Run in the current shell** (affects the environment):
  ```bash
  source script.sh
  ```
  or
  ```bash
  . script.sh
  ```

---

#### **Troubleshooting & Debugging**

- **Check script syntax** (without running it):

  ```bash
  bash -n script.sh
  ```

- **Show expanded commands** (for debugging):

  ```bash
  bash -x script.sh
  ```

- **Enable debugging in the script**:

  ```bash
  set -x  # Enable
  set +x  # Disable
  ```

- **ShellCheck**: Use for static analysis to catch common errors:
  ```bash
  shellcheck script.sh
  ```

---

#### **Strict Mode (Recommended for Debugging)**

- Enable strict mode to catch errors early:

  ```bash
  #!/bin/bash --
  set -o nounset    # Exit on unbound variable
  set -o errexit    # Exit on command failure
  set -o pipefail   # Exit on pipeline failure
  ```

  Shortened:

  ```bash
  set -euo pipefail
  ```

---

#### **Security: Prevent Interpreter Spoofing**

- **Use `--` after she-bang** to prevent option processing:
  ```bash
  #!/usr/bin/env bash --
  ```

---

#### **Variable Best Practices**

- **Uppercase for environment variables**:

  ```bash
  USER="Tux"
  ```

- **Lowercase for local variables**:

  ```bash
  user="Tux"
  ```

- **Use underscores for multi-word variables**:
  ```bash
  current_user="Tux"
  ```

#### **Other Tips**

- **File Extensions**: It's good practice to use `.sh` for shell scripts, but not necessary for execution.

## Test Command

> evaluate logical expressions

- **Syntax**:

  ```bash
  test <expression>
  ```

  or

  ```bash
  [ <expression> ]
  ```

> TRUE is 0, FALSE is 1
> see using `echo $?` to see the return value

- **Examples**:

  ```bash
  test 1 -eq 1  # 1 equals 1
  test 1 -ne 1  # 1 not equals 1
  test -f file.txt  # TRUE if file is a regular file
  test -d dir  # TRUE if directory exists
  test -z ""  # TRUE if string is empty
  test -n "text"  # TRUE if string is not empty
  test 10 -gt 5  # 10 is greater than 5
  test 10 -lt 5  # 10 is less than 5
  test 10 -ge 10  # 10 is greater than or equal to 10
  test 10 -le 10  # 10 is less than or equal to 10
  test -w file.txt  # TRUE if file is writable
  test -r file.txt  # TRUE if file is readable
  test -e file.txt  # TRUE if file exists
  test file1.txt -nt file.txt  # TRUE if file1 is newer than file
  test "abc" = "abc"  # TRUE if strings are equal
  test "${1}" != "def"  # TRUE if first argument is not "def"
  test 66 -ge 18 -a 66 -le 60  # 66 is greater than or equal to 18 AND 66 is less than or equal to 60 -> better to use &&
  test 66 -ge 18 -o 66 -le 60  # 66 is greater than or equal to 18 OR 66 is less than or equal to 60 -> better to use ||
  ```

> **Note**: Always quote variables to avoid errors.
> **Note**: `[` is a command, so it requires spaces around it.
> **Note**: numerical values need to be integers. Floating point numbers are not supported.

## Conditional Statements

### **If-Else Statement**

- **Syntax**:

  ```bash
  if [ <condition> ]
  then
    # Commands
  else
    # Commands
  fi
  ```

### **If-Elif Statement**

- **Syntax**:

  ```bash
  if [ <condition1> ]
  then
    # Commands
  elif [ <condition2> ]
  then
    # Commands
  else
    # Commands
  fi
  ```

### **Case Statement**

- **Syntax**:

  ```bash
  case <variable> in
    pattern1)
      # Commands
      ;;
    pattern2)
      # Commands
      ;;
    *)
      # Default
      ;;
  esac
  ```

## Loops

### **For Loop**

- **Syntax**:

  ```bash
  for var in item1 item2 item3
  do
    # Commands
  done
  ```

> item1, item2, item3 can be a list of items or a range. e.g., `{1..5}, seq 1 5`

### **While Loop**

- **Syntax**:

  ```bash
  while [ <condition> ]
  do
    # Commands
  done
  ```

> endless loop: `while true or while :`

### **Until Loop**

- **Syntax**:

  ```bash
  until [ <condition> ]
  do
    # Commands
  done
  ```

> equivalent of the do-while loop in other languages

## Arguments

### **Positional Parameters**

- **Accessing Arguments**:

  - `$0`: Script name
  - `$1`, `$2`, ...: First, second arguments, and so on.
  - `$@`: All arguments as separate words.
  - `$*`: All arguments as a single word.
  - `$#`: Number of arguments.
  - `$$`: PID of the script.
  - `$?`: Exit status of the last command.

> use {} to avoid ambiguity: `${10}`

### Shift Command

- **Shift Arguments**:

  - `shift`: Shifts arguments to the left (e.g., `$2` becomes `$1`).

## Runtime Input

### **Read Command**

- **Read User Input**:

  ```bash
  read -p "Enter your name: " -r name
  ```

  - `-p`: Prompt message
  - `-r`: Raw input (disables backslash escaping)

- **Read file content**:

  ```bash
  while read -r line
  do
    echo "$line"
  done < inputfile.txt
  ```

## Sourcing configuration files

- **Source a Configuration File**:7

  ```
  # config.sh
  user="Tux"

  num=5
  ```

  ```bash
  source config.sh

  echo "User: $user"
  echo "Number: $num"
  ```

  or

  ```bash
  . config.sh

  echo "User: $user"
  echo "Number: $num"
  ```

## Get script options

### **Getopts Command**

- **Syntax**:

  ```bash
  while getopts ":afz" opt
  do
    case $opt in
      a)
        echo "Option a"
        ;;
      f)
        echo "Option f"
        ;;
      z)
        echo "Option z"
        ;;
      :)
        echo "Option -$OPTARG requires an argument."
        ;;
      \?)
        echo "Invalid option: -$OPTARG"
        ;;
    esac
  done
  ```

  - **Options**: `a:`, `b:`: Options that require arguments.
  - **Colon**: Indicates options that require arguments.
  - **Question Mark**: Invalid option.
  - **Colon at the Beginning**: Silent error handling.

## Functions

### **Function Definition**

- **Syntax**:

  ```bash
  function_name() {
    # Commands
  }
  ```

  or

  ```bash
  function function_name {
    # Commands
  }
  ```

- **Example**:

  ```bash
  greet() {
    echo "Hello, $1"
  }

  greet "Tux"
  ```

- **Parameters**: `$1`, `$2`, ...: Access function arguments.

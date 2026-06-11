# 🖥️ Terminal Keybindings Cheat Sheet

## 🧭 Navigation
| Shortcut      | Description                              |
|---------------|------------------------------------------|
| `Ctrl + A`    | Move to the beginning of the line         |
| `Ctrl + E`    | Move to the end of the line               |
| `Ctrl + B`    | Move back one character                   |
| `Ctrl + F`    | Move forward one character                |
| `Alt + B`     | Move back one word                        |
| `Alt + F`     | Move forward one word                     |

---

## ✏️ Editing
| Shortcut      | Description                                      |
|---------------|--------------------------------------------------|
| `Ctrl + U`    | Cut/delete from cursor to beginning of line      |
| `Ctrl + K`    | Cut/delete from cursor to end of line            |
| `Ctrl + W`    | Cut/delete the word before the cursor            |
| `Ctrl + Y`    | Paste the last cut text                          |
| `Ctrl + L`    | Clear the screen                                 |

---

## 🕘 History
| Shortcut      | Description                                      |
|---------------|--------------------------------------------------|
| `Ctrl + R`    | Search command history (reverse search)          |
| `Ctrl + G`    | Escape from history search mode                  |
| `Ctrl + P`    | Go to the previous command in history            |
| `Ctrl + N`    | Go to the next command in history                |
| `Ctrl + C`    | Terminate the current command                    |

---

## 📥 Input and Output
| Command/Symbol | Description                                                          |
|---|---|
| `read filename` | Stores user input in variable filename                               |
| `-e` | Checks if the file exists                                           |
| `cat "$filename"` | Displays file content                                              |
| `cat > "$filename"` | Redirects output to create/write file                             |

**Creating/Writing files with `cat > "$filename"`:**
- Terminal enters input mode
- Type content and press Enter for new lines
- Press `Ctrl + D` to save and exit
- Press `Ctrl + C` to cancel without saving

---

## 🔄 Redirection Operators
| Operator | Description                                  |
|---|---|
| `>` | Redirect stdout to a file (overwrite)        |
| `>>` | Redirect stdout to a file (append)          |
| `2>` | Redirect stderr (error messages)             |
| `&>` | Redirect both stdout and stderr              |

---

## 🔐 File Permissions

### chmod - Change file/directory permissions
| Command | Description                                          |
|---|---|
| `chmod 755 filename` | Owner: rwx, Group: r-x, Others: r-x             |
| `chmod 644 filename` | Owner: rw-, Group: r--, Others: r--             |
| `chmod +x filename` | Add execute permission to all users              |
| `chmod -x filename` | Remove execute permission from all users         |
| `chmod u+rw filename` | Add read/write to user (owner)                  |
| `chmod g+r filename` | Add read to group                                |
| `chmod o-w filename` | Remove write from others                         |

### Permission Numbers
| Position | Value | Meaning |
|---|---|---|
| r (read) | 4 | Can read file/list directory |
| w (write) | 2 | Can write/modify file |
| x (execute) | 1 | Can execute file/access directory |

**How numbers work:** rwxrwxrwx → First 3 (owner) + Second 3 (group) + Third 3 (others)

| Example | Calculation | Permissions |
|---|---|---|
| 7 | 4+2+1 | rwx (read, write, execute) |
| 6 | 4+2 | rw- (read, write) |
| 5 | 4+1 | r-x (read, execute) |
| 4 | 4 | r-- (read only) |

### chown - Change file owner
| Command | Description |
|---|---|
| `chown username filename` | Change owner to username |
| `chown username:groupname filename` | Change owner and group |
| `chown -R username directory` | Recursively change permissions in directory |

### ls -l - Display file details
| Format | Meaning |
|---|---|
| `-rw-r--r--` | First char: file type; next 3: owner; next 3: group; last 3: others |

---

## 🔢 Sort and group

> `sort` only sorts the output, not modifying the file itself.

### Grouping Lines by Value

| Command / Pipeline | Description |
|---|---|
| `sort input.txt` | Group identical lines together sequentially |
| `sort input.txt \| uniq -c` | Group lines and show the occurrence count for each |
| `sort input.txt \| uniq -c \| sort -nr` | Group lines and list by highest frequency first |
| `sort input.txt \| uniq -c \| sort -n` | Group lines and list by lowest frequency first |
| `sort input.txt \| uniq -d` | Group lines and output **only** the duplicates |
| `sort input.txt \| uniq -u` | Group lines and output **only** the completely unique lines |
| `sort -f input.txt \| uniq -i -c` | Group lines case-insensitively (e.g., treating 'A' and 'a' as same) |
| `sort -k 2,2 input.txt` | Group lines based strictly on the values in column 2 |
| `sort -t ',' -k 3,3 input.csv` | Group by column 3 using a custom delimiter (comma) |

---

### Sorting Output Lines by Date

| Command / Pipeline | Date Format Targeted | Description |
|---|---|---|
| `sort input.txt` | `YYYY-MM-DD` (ISO 8601) | Chronological sort (oldest to newest) |
| `sort -r input.txt` | `YYYY-MM-DD` (ISO 8601) | Reverse chronological sort (newest to oldest) |
| `sort -M input.txt` | `Jan`, `Feb`, `Mar` | Sorts alphabetically by 3-letter locale month names |
| `sort -k 1M -k 2n input.txt` | `Month DD` (e.g., Oct 29) | Sorts by month column first, then numerically by day |
| `sort -k 1M -k 2n -k 3 input.txt` | `Month DD HH:MM:SS` | Standard syslog format sorting down to the second |
| `sort -t '/' -k 3,3n -k 2,2n -k 1,1n input.txt` | `DD/MM/YYYY` | Extracts and sorts by year, then month, then day |
| `sort -t '/' -k 3,3n -k 1,1n -k 2,2n input.txt` | `MM/DD/YYYY` | Extracts and sorts by US year, then month, then day |
| `sort -n input.txt` | `1718049900` (Unix Epoch) | Sorts timestamps chronologically via raw seconds |
| `sort -k 4M -k 5n input.txt` | Mixed Data Columns | Targets the date when it is hidden in columns 4 and 5 |

> * -t '/': Specifies the delimiter and breaks the line into columns using / as the separator.
> * -k 3,3n: Sorts numerically (n) on the 3rd column (Year).
> * -k 2,2n: Breaks ties using the 2nd column (Month).
> * -k 1,1n: Breaks ties using the 1st column (Day).


## 🔍 uniq - Remove or report duplicate lines
| Command | Description |
|---|---|
| `uniq filename` | Display file with duplicate consecutive lines removed |
| `uniq -c filename` | Count occurrences of each line (prefix count to each line) |
| `uniq -d filename` | Show only lines that appear more than once (duplicates) |
| `uniq -u filename` | Show only unique lines (lines that appear only once) |
| `uniq -i filename` | Case-insensitive comparison |
| `uniq -f N filename` | Skip first N fields when comparing lines |
| `uniq -s N filename` | Skip first N characters when comparing lines |

**Note:** uniq works on consecutive duplicate lines. Use `sort filename | uniq` to remove all duplicates in a file.
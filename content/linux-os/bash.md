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

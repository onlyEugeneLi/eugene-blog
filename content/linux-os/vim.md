# 📝 Vim Keybindings Cheat Sheet

## 🧭 Normal Mode
| Key         | Description                                     |
|-------------|-------------------------------------------------|
| `i`         | Enter insert mode at the current cursor position |
| `x`         | Delete the character under the cursor           |
| `dd`        | Delete the current line                         |
| `yy`        | Copy the current line                           |
| `p`         | Paste the copied or deleted text **below** the current line |
| `u`         | Undo the last change                            |
| `Ctrl + R`  | Redo the last undo                              |
| `:set nu` or `:set number` | Display line numbers            |

---

## 🔧 Command Mode (type `:` to enter)
| Command     | Description                                     |
|-------------|-------------------------------------------------|
| `:w`        | Save the file                                   |
| `:q`        | Quit Vim                                        |
| `:q!`       | Quit Vim **without saving** changes             |
| `:wq` or `:x` | Save **and** quit Vim                         |
| `:s/old/new/g` | Replace all occurrences of `"old"` with `"new"` |

---

## 🖍️ Visual Mode (press `v` to enter)
| Key         | Description                                     |
|-------------|-------------------------------------------------|
| `v`         | Enter visual mode to select text                |
| `y`         | Copy the selected text                          |
| `d`         | Delete the selected text                        |
| `p`         | Paste over selected text (replaces selection)   |

---

💡 **Tip:** Use `Esc` anytime to return to normal mode.

# 📝 Vim Commands for DevOps

This repository contains a complete collection of **Vim commands** that are especially useful for **DevOps engineers**.  
Whether you are editing configuration files, debugging YAML, managing Kubernetes manifests, or writing Dockerfiles — Vim will be your best friend once you master it 🚀.

---

## 📂 Repository Contents
- `VimCommands.md` → Full categorized list of Vim commands.
- `CheatSheet.pdf` (optional) → Printable one-page cheat sheet (to be added).
- Example configs (`examples/`) → Sample YAML, JSON, and config files to practice.


---

## ⚡ Why Vim for DevOps?
- **Lightweight**: Vim is available on almost all Linux/Unix systems by default.
- **Fast Editing**: Quick edits in SSH sessions, Docker containers, or CI/CD pipelines.
- **Config Friendly**: Perfect for YAML, JSON, Dockerfile, Jenkinsfile, Ansible, and Kubernetes manifests.
- **Customizable**: Add `.vimrc` for syntax highlighting, indentation, and productivity plugins.

---

## 🔹 Common Commands (Quick Glance)

### File Operations
```vim
:w      " Save file
:q      " Quit
:wq     " Save and quit
:q!     " Quit without saving
:e file " Open another file

### Navigation
gg      " Go to top
G       " Go to bottom
:n      " Go to line n
0       " Start of line
$       " End of line

### Editing
i       " Insert mode
a       " Append after cursor
o       " New line below
dd      " Delete line
yy      " Yank (copy) line
p       " Paste below
u       " Undo
Ctrl+r  " Redo

### Search and Replace

/word           " Search forward
?word           " Search backward
:%s/old/new/g   " Replace all in file


🔹 Basic Vim Modes

i → Insert mode (start typing)

a → Append after cursor

o → Open new line below

Esc → Back to Normal mode

: → Command mode

🔹 File Operations

:e filename → Open file

:w → Save (write)

:q → Quit

:wq or ZZ → Save & quit

:q! → Quit without saving

:x → Save & exit (same as :wq)

:saveas newfile → Save as new file

:r filename → Read/insert contents of another file

🔹 Navigation (very handy in YAML, JSON, configs)

h → Left

l → Right

j → Down

k → Up

0 → Beginning of line

^ → First non-blank char of line

$ → End of line

gg → Top of file

G → Bottom of file

:n → Go to line n (e.g., :25)

🔹 Searching & Replacing

/word → Search forward

?word → Search backward

n → Next match

N → Previous match

:%s/old/new/g → Replace all in file

:s/old/new/g → Replace in current line

:%s/old/new/gc → Replace all with confirm

🔹 Editing & Deleting

x → Delete char

dd → Delete line

ndd → Delete n lines (e.g., 3dd)

dw → Delete word

D → Delete till end of line

yy → Yank (copy) line

nyy → Yank n lines

p → Paste after cursor

P → Paste before cursor

u → Undo

Ctrl + r → Redo

🔹 Visual Mode (super useful for YAML/JSON indentation)

v → Visual mode (select text)

V → Visual line mode

Ctrl + v → Visual block mode (useful for multi-line edits in configs)

y → Yank selected

d → Delete selected

> → Indent selected block

< → Un-indent selected block

🔹 Indentation & Formatting

>> → Indent current line

<< → Un-indent current line

= → Auto-indent

gg=G → Auto-indent whole file (great for messy YAML)

🔹 Buffers, Windows & Tabs (multi-file editing, DevOps style)

:ls → List buffers

:b n → Switch to buffer n

:bd → Delete buffer

:sp filename → Split window horizontally

:vsp filename → Split window vertically

Ctrl + w w → Switch between windows

:tabnew filename → Open new tab

gt → Next tab

gT → Previous tab

🔹 Marks & Macros (time savers)

ma → Mark position as a

`a → Jump to mark a

qa → Record macro in register a

q → Stop recording

@a → Run macro a

@@ → Repeat last macro

🔹 Useful DevOps Shortcuts

:set nu → Show line numbers

:set nonu → Hide line numbers

:set paste → Avoid auto-indent issues when pasting YAML/JSON

:syntax on → Enable syntax highlighting (great for Dockerfiles, YAML, etc.)

:set expandtab → Convert tabs to spaces (YAML friendly)

:set tabstop=2 shiftwidth=2 → Indent with 2 spaces (K8s/Ansible standard)

:set tabstop=4 shiftwidth=4 → Indent with 4 spaces (Python/Dockerfile standard)

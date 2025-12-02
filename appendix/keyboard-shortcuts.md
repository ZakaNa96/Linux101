# Keyboard Shortcuts Reference

> Essential keyboard shortcuts for terminal, bash, and text editors.

---

## Terminal Control Shortcuts

### Process Control

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel/interrupt current command |
| `Ctrl+Z` | Suspend current process (send to background) |
| `Ctrl+D` | End of input (EOF) / Exit shell |
| `Ctrl+\` | Quit (stronger than Ctrl+C, sends SIGQUIT) |

### Screen Control

| Shortcut | Action |
|----------|--------|
| `Ctrl+L` | Clear screen (same as `clear` command) |
| `Ctrl+S` | Stop screen output (freeze) |
| `Ctrl+Q` | Resume screen output (unfreeze) |

---

## Bash Line Editing (Emacs Mode)

### Cursor Movement

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Move to beginning of line |
| `Ctrl+E` | Move to end of line |
| `Ctrl+B` | Move backward one character |
| `Ctrl+F` | Move forward one character |
| `Alt+B` | Move backward one word |
| `Alt+F` | Move forward one word |
| `Ctrl+XX` | Toggle between start of line and current position |

### Deleting Text

| Shortcut | Action |
|----------|--------|
| `Ctrl+D` | Delete character under cursor |
| `Ctrl+H` | Delete character before cursor (backspace) |
| `Ctrl+W` | Delete word before cursor |
| `Alt+D` | Delete word after cursor |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+U` | Delete from cursor to start of line |
| `Ctrl+Y` | Paste (yank) last deleted text |

### Text Manipulation

| Shortcut | Action |
|----------|--------|
| `Ctrl+T` | Swap character with previous |
| `Alt+T` | Swap word with previous |
| `Alt+U` | Uppercase word |
| `Alt+L` | Lowercase word |
| `Alt+C` | Capitalize word |
| `Ctrl+_` | Undo last edit |

---

## Bash History Navigation

### Basic History

| Shortcut | Action |
|----------|--------|
| `↑` / `Ctrl+P` | Previous command in history |
| `↓` / `Ctrl+N` | Next command in history |
| `Ctrl+R` | Reverse search history |
| `Ctrl+S` | Forward search history (if enabled) |
| `Ctrl+G` | Cancel history search |
| `Alt+.` | Insert last argument of previous command |
| `Alt+number+.` | Insert nth argument of previous command |

### History Search

```bash
# While in Ctrl+R search mode:
# - Type to search
# - Ctrl+R again for next match
# - Ctrl+G to cancel
# - Enter to execute
# - Right arrow to edit before executing
```

### History Commands

| Command | Action |
|---------|--------|
| `history` | Show command history |
| `!!` | Repeat last command |
| `!n` | Repeat command number n |
| `!string` | Repeat last command starting with string |
| `!?string` | Repeat last command containing string |
| `^old^new` | Replace old with new in last command |

---

## Tab Completion

| Shortcut | Action |
|----------|--------|
| `Tab` | Auto-complete command, file, or directory |
| `Tab Tab` | Show all possible completions |
| `Alt+?` | Show completions (same as double Tab) |
| `Alt+*` | Insert all possible completions |

---

## nano Shortcuts

### File Operations

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Save file (Write Out) |
| `Ctrl+X` | Exit nano |
| `Ctrl+R` | Insert another file |
| `Ctrl+T` | Open file browser (when saving) |

### Navigation

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Go to beginning of line |
| `Ctrl+E` | Go to end of line |
| `Ctrl+Y` | Page up |
| `Ctrl+V` | Page down |
| `Ctrl+_` | Go to line number |
| `Ctrl+C` | Show current line number |
| `Alt+\` | Go to first line |
| `Alt+/` | Go to last line |

### Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+K` | Cut current line |
| `Ctrl+U` | Paste (uncut) |
| `Alt+6` | Copy current line |
| `Ctrl+J` | Justify paragraph |
| `Alt+U` | Undo |
| `Alt+E` | Redo |
| `Ctrl+\` | Search and replace |
| `Alt+3` | Toggle line numbers |

### Search

| Shortcut | Action |
|----------|--------|
| `Ctrl+W` | Search forward |
| `Alt+W` | Repeat search |
| `Ctrl+\` | Search and replace |

### Selection

| Shortcut | Action |
|----------|--------|
| `Alt+A` | Start selection (set mark) |
| `Ctrl+6` | Start selection (alternative) |
| `Ctrl+K` | Cut selection |
| `Alt+6` | Copy selection |

---

## vim Shortcuts

### Mode Switching

| Key | Action |
|-----|--------|
| `Esc` | Return to Normal mode |
| `i` | Insert mode (before cursor) |
| `a` | Insert mode (after cursor) |
| `I` | Insert mode (start of line) |
| `A` | Insert mode (end of line) |
| `o` | Insert mode (new line below) |
| `O` | Insert mode (new line above) |
| `v` | Visual mode (character) |
| `V` | Visual mode (line) |
| `Ctrl+V` | Visual mode (block) |
| `:` | Command mode |

### Navigation (Normal Mode)

| Key | Action |
|-----|--------|
| `h j k l` | Left, down, up, right |
| `w` | Next word start |
| `b` | Previous word start |
| `e` | Next word end |
| `0` | Start of line |
| `^` | First non-blank character |
| `$` | End of line |
| `gg` | First line of file |
| `G` | Last line of file |
| `{number}G` | Go to line number |
| `Ctrl+F` | Page down |
| `Ctrl+B` | Page up |
| `Ctrl+D` | Half page down |
| `Ctrl+U` | Half page up |
| `%` | Jump to matching bracket |

### Editing (Normal Mode)

| Key | Action |
|-----|--------|
| `x` | Delete character |
| `dd` | Delete line |
| `dw` | Delete word |
| `d$` or `D` | Delete to end of line |
| `d0` | Delete to start of line |
| `yy` | Copy (yank) line |
| `yw` | Copy word |
| `y$` | Copy to end of line |
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `u` | Undo |
| `Ctrl+R` | Redo |
| `.` | Repeat last command |
| `r{char}` | Replace character |
| `R` | Enter Replace mode |
| `~` | Toggle case |
| `>>` | Indent line |
| `<<` | Unindent line |
| `J` | Join lines |

### Search (Normal Mode)

| Key | Action |
|-----|--------|
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next match |
| `N` | Previous match |
| `*` | Search word under cursor forward |
| `#` | Search word under cursor backward |

### File Operations (Command Mode)

| Command | Action |
|---------|--------|
| `:w` | Save |
| `:q` | Quit |
| `:wq` or `:x` | Save and quit |
| `:q!` | Quit without saving |
| `:w filename` | Save as |
| `:e filename` | Open file |
| `:r filename` | Insert file content |

### Quick vim Reference Card

```
MODES: i=Insert, v=Visual, :=Command, Esc=Normal

SAVE/QUIT: :w (save) :q (quit) :wq (both) :q! (force quit)

MOVE: h←  j↓  k↑  l→  w=word  b=back  0=start  $=end  gg=top  G=bottom

EDIT: x=del  dd=del line  yy=copy line  p=paste  u=undo  .=repeat

SEARCH: /text  n=next  N=prev
```

---

## less / man Page Navigation

| Key | Action |
|-----|--------|
| `Space` or `f` | Page down |
| `b` | Page up |
| `d` | Half page down |
| `u` | Half page up |
| `g` | Go to first line |
| `G` | Go to last line |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next search match |
| `N` | Previous search match |
| `q` | Quit |
| `h` | Help |
| `j` / `↓` | One line down |
| `k` / `↑` | One line up |

---

## Screen (Terminal Multiplexer)

> Screen prefix key is `Ctrl+A`

### Session Management

| Shortcut | Action |
|----------|--------|
| `screen` | Start new session |
| `screen -S name` | Start named session |
| `screen -ls` | List sessions |
| `screen -r name` | Reattach to session |
| `Ctrl+A d` | Detach from session |
| `Ctrl+A \` | Kill all windows and exit |

### Window Management

| Shortcut | Action |
|----------|--------|
| `Ctrl+A c` | Create new window |
| `Ctrl+A n` | Next window |
| `Ctrl+A p` | Previous window |
| `Ctrl+A 0-9` | Switch to window 0-9 |
| `Ctrl+A "` | List all windows |
| `Ctrl+A A` | Rename current window |
| `Ctrl+A k` | Kill current window |

### Split Screen

| Shortcut | Action |
|----------|--------|
| `Ctrl+A S` | Split horizontally |
| `Ctrl+A |` | Split vertically |
| `Ctrl+A Tab` | Switch between splits |
| `Ctrl+A X` | Remove current split |
| `Ctrl+A Q` | Remove all splits except current |

---

## tmux (Terminal Multiplexer)

> tmux prefix key is `Ctrl+B` by default

### Session Management

| Shortcut | Action |
|----------|--------|
| `tmux` | Start new session |
| `tmux new -s name` | Start named session |
| `tmux ls` | List sessions |
| `tmux attach -t name` | Attach to session |
| `Ctrl+B d` | Detach from session |

### Window Management

| Shortcut | Action |
|----------|--------|
| `Ctrl+B c` | Create new window |
| `Ctrl+B n` | Next window |
| `Ctrl+B p` | Previous window |
| `Ctrl+B 0-9` | Switch to window 0-9 |
| `Ctrl+B w` | List windows |
| `Ctrl+B ,` | Rename window |
| `Ctrl+B &` | Kill window |

### Pane Management

| Shortcut | Action |
|----------|--------|
| `Ctrl+B %` | Split vertically |
| `Ctrl+B "` | Split horizontally |
| `Ctrl+B ←↑↓→` | Move between panes |
| `Ctrl+B o` | Next pane |
| `Ctrl+B x` | Kill current pane |
| `Ctrl+B z` | Toggle pane zoom |
| `Ctrl+B {` | Move pane left |
| `Ctrl+B }` | Move pane right |
| `Ctrl+B Space` | Toggle pane layouts |

### Copy Mode

| Shortcut | Action |
|----------|--------|
| `Ctrl+B [` | Enter copy mode |
| `q` | Exit copy mode |
| `Space` | Start selection |
| `Enter` | Copy selection |
| `Ctrl+B ]` | Paste |

---

## Quick Reference Card

```
TERMINAL:
  Ctrl+C = cancel       Ctrl+L = clear
  Ctrl+Z = suspend      Ctrl+D = exit/EOF

BASH EDITING:
  Ctrl+A = start line   Ctrl+E = end line
  Ctrl+U = delete left  Ctrl+K = delete right
  Ctrl+W = delete word  Ctrl+Y = paste
  Ctrl+R = search hist  Tab = complete

NANO:
  Ctrl+O = save         Ctrl+X = exit
  Ctrl+K = cut          Ctrl+U = paste
  Ctrl+W = search       Ctrl+G = help

VIM:
  i = insert mode       Esc = normal mode
  :w = save             :q = quit
  dd = delete line      yy = copy line
  p = paste             u = undo

LESS/MAN:
  Space = page down     b = page up
  /pattern = search     q = quit

TMUX (prefix Ctrl+B):
  c = new window        n/p = next/prev
  " = split horiz       % = split vert
  d = detach            x = kill pane
```

---

*Reference: Linux 101 Course - All Modules*
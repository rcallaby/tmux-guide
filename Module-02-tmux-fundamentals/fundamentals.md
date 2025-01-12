# Tmux Fundamentals

tmux, short for "terminal multiplexer," is a powerful tool for managing multiple terminal sessions within a single window. It allows users to create and switch between **sessions**, which act as workspaces containing one or more **windows** (akin to tabs). Each window can be split into **panes**, enabling multitasking within a single terminal. tmux sessions persist in the background, making it ideal for remote work or long-running tasks, as you can detach and reattach sessions at any time. Navigation and control are centered around a **prefix key** (default: `Ctrl-b`), followed by commands for actions like creating windows, splitting panes, or resizing layouts. Advanced features include **copy mode** for capturing terminal output, **mouse support** for easier interaction, and extensive **customization** via a configuration file (`~/.tmux.conf`). tmux is indispensable for developers, sysadmins, and anyone seeking an organized and efficient terminal workflow. Mastering tmux involves understanding its hierarchy (sessions, windows, panes), learning essential key bindings, and customizing it for your workflow.

## Understanding tmux terminology

Here’s a **comprehensive summary of tmux terminology** that explains all key concepts and commands in detail. tmux (Terminal Multiplexer) is a powerful tool for managing multiple terminal sessions in a single window, making it highly popular among developers and sysadmins. Below are the core terms and their detailed explanations:

---

### **1. Session**
- **Definition**: A **session** is the top-level tmux construct. It encapsulates a workspace containing one or more windows and panes.
- **Purpose**: Sessions allow you to preserve your terminal work environment even if you detach from tmux or lose your terminal connection.
- **Commands**:
  - `tmux new -s <session-name>`: Create a new session with a specific name.
  - `tmux list-sessions` (or `tmux ls`): List all active sessions.
  - `tmux attach -t <session-name>`: Reattach to an existing session.
  - `tmux kill-session -t <session-name>`: Kill a specific session.
  - `tmux switch-client -t <session-name>`: Switch to another session.

---

### **2. Window**
- **Definition**: A **window** is a collection of one or more panes within a session. Each window is like a tab in a terminal emulator.
- **Purpose**: Windows help organize different tasks within a session (e.g., one window for editing code, another for running tests).
- **Commands**:
  - `tmux new-window` (or `Ctrl-b c`): Create a new window.
  - `tmux rename-window <new-name>` (or `Ctrl-b ,`): Rename the current window.
  - `tmux select-window -t <window-index>` (or `Ctrl-b <number>`): Switch to a specific window by index.
  - `tmux list-windows`: List all windows in the current session.
  - `tmux kill-window -t <window-index>`: Close a specific window.

---

### **3. Pane**
- **Definition**: A **pane** is a subdivision of a window. A single window can have multiple panes arranged in vertical or horizontal splits.
- **Purpose**: Panes allow multitasking within a window, such as running multiple commands or monitoring logs while coding.
- **Commands**:
  - `Ctrl-b %`: Split the current pane vertically.
  - `Ctrl-b "`: Split the current pane horizontally.
  - `Ctrl-b o`: Move to the next pane.
  - `Ctrl-b x`: Close the current pane.
  - `Ctrl-b z`: Zoom into the current pane (toggle between full-screen and split view).
  - `Ctrl-b {` and `Ctrl-b }`: Rearrange panes (swap left/right or up/down).
  - `tmux select-pane -t <pane-index>`: Switch to a specific pane.

---

### **4. Attach/Detach**
- **Attach**:
  - **Definition**: Connect to an existing tmux session.
  - **Command**: `tmux attach -t <session-name>` or simply `tmux a` (if there's only one session).
  
- **Detach**:
  - **Definition**: Disconnect from the tmux session while keeping it running in the background.
  - **Command**: `Ctrl-b d` (detach the current session).

- **Use Case**: If you're working on a remote server and want to maintain your work environment, you can detach before logging out, then reattach later.

---

### **5. Prefix Key (`Ctrl-b`)**
- **Definition**: The **prefix key** is a special key combination that precedes all tmux commands. By default, it's `Ctrl-b`.
- **Purpose**: Prevents conflicts with terminal shortcuts by ensuring that tmux commands are not executed accidentally.
- **Customization**: You can change the prefix in your `~/.tmux.conf`:
  ```bash
  set-option -g prefix C-a
  unbind C-b
  bind C-a send-prefix
  ```

---

### **6. Key Bindings**
- **Definition**: Keyboard shortcuts used to control tmux.
- **Default Bindings**:
  - `Ctrl-b c`: Create a new window.
  - `Ctrl-b n`: Switch to the next window.
  - `Ctrl-b p`: Switch to the previous window.
  - `Ctrl-b &`: Kill the current window.
  - `Ctrl-b w`: Show a list of windows and navigate interactively.
  - `Ctrl-b [`: Enter copy mode (see **Copy Mode** below).

---

### **7. Copy Mode**
- **Definition**: A mode that allows you to scroll through terminal output and copy text.
- **Commands**:
  - `Ctrl-b [`: Enter copy mode.
  - Navigation:
    - Arrow keys or `hjkl`: Move cursor.
    - `Ctrl-u` and `Ctrl-d`: Scroll up or down by half a screen.
    - `g` and `G`: Jump to the start or end of the buffer.
  - Copy:
    - Move to the start of the text, press `Space` to begin selection, move to the end, and press `Enter` to copy.
    - Paste: `Ctrl-b ]`.

---

### **8. Status Bar**
- **Definition**: The bar at the bottom of the tmux interface that displays information such as session name, window list, and active pane.
- **Customization**:
  - You can modify the status bar in `~/.tmux.conf`:
    ```bash
    set -g status-bg black
    set -g status-fg white
    set -g status-left "Session: #S "
    set -g status-right "#(date +'%Y-%m-%d %H:%M')"
    ```

---

### **9. Configuration File (`~/.tmux.conf`)**
- **Definition**: A file to customize tmux behavior.
- **Common Settings**:
  - Change prefix: `set -g prefix C-a`.
  - Enable mouse support: `set -g mouse on`.
  - Set scrollback buffer size: `set -g history-limit 10000`.

---

### **10. Mouse Mode**
- **Definition**: A mode that allows you to interact with tmux using the mouse for actions like resizing panes, switching windows, and scrolling.
- **Enable**:
  ```bash
  set -g mouse on
  ```

---

### **11. Layouts**
- **Definition**: Preset arrangements for panes in a window.
- **Commands**:
  - `Ctrl-b Space`: Cycle through layouts (even-horizontal, even-vertical, etc.).
  - `tmux select-layout <layout-name>`: Set a specific layout (`tiled`, `main-horizontal`, `main-vertical`, etc.).

---

### **12. Buffers**
- **Definition**: Temporary storage for copied text.
- **Commands**:
  - `tmux show-buffer`: Display the last copied buffer.
  - `tmux save-buffer <file>`: Save the buffer to a file.
  - `tmux list-buffers`: List all buffers.
  - `tmux delete-buffer -b <buffer-index>`: Delete a specific buffer.

---

### **13. Resizing Panes**
- **Commands**:
  - `Ctrl-b : resize-pane -D` (down).
  - `Ctrl-b : resize-pane -U` (up).
  - `Ctrl-b : resize-pane -L` (left).
  - `Ctrl-b : resize-pane -R` (right).
  - You can also use `Ctrl-b Alt` combined with arrow keys for resizing interactively.

---

### **14. Scripting and Automation**
- **Definition**: tmux can execute predefined commands using scripts.
- **Example**:
  ```bash
  tmux new-session -d -s mysession
  tmux send-keys -t mysession "htop" C-m
  tmux split-window -h
  tmux send-keys "tail -f /var/log/syslog" C-m
  tmux attach -t mysession
  ```

---

### **15. Environment Variables**
- **Definition**: You can pass environment variables to sessions or panes.
- **Command**:
  - `tmux set-environment <variable-name> <value>`: Set a variable.
  - `tmux show-environment`: Display all variables.

---

## Navigating tmux


Navigating in tmux revolves around efficiently moving between **sessions**, **windows**, and **panes**. Below is a comprehensive breakdown of tmux navigation commands:

---

### **Session Navigation**
- **Switching Sessions**:  
  - `tmux switch-client -t [session-name]`: Switch to a specific session by name.  
  - Prefix + `(` or `)` (default: `Ctrl-b` + `(` or `)`): Move to the previous or next session, respectively.
- **Listing Sessions**:  
  - `tmux list-sessions`: Display all available sessions.
  - Prefix + `s`: Open an interactive list of sessions to switch to.
- **Detaching and Reattaching**:  
  - `tmux detach` or Prefix + `d`: Detach the current session.  
  - `tmux attach -t [session-name]`: Reattach to a specific session.  
  - `tmux attach`: Reattach to the most recently used session.

---

### **Window Navigation**
- **Switching Windows**:  
  - Prefix + `n`: Move to the next window in the session.  
  - Prefix + `p`: Move to the previous window in the session.  
  - Prefix + `[0-9]`: Jump directly to a window by its index.  
  - Prefix + `l`: Return to the last accessed window.
- **Listing Windows**:  
  - Prefix + `w`: Open a list of windows for quick selection.
- **Cycling Through Windows**:  
  - Use `n` or `p` repeatedly after the prefix to cycle through windows in order.

---

### **Pane Navigation**
- **Switching Between Panes**:  
  - Prefix + Arrow Key: Move between panes in the direction of the arrow key.  
  - Prefix + `o`: Move to the next pane in cyclic order.  
  - Prefix + `;`: Toggle between the last two panes used.
- **Mouse Support (if enabled)**:  
  - Click on a pane to switch focus when mouse mode is active (set `set -g mouse on` in `.tmux.conf`).

---

### **Zooming and Resizing Panes**
- **Zoom a Pane**:  
  - Prefix + `z`: Toggle zoom on the active pane to maximize or restore it.
- **Resizing Panes**:  
  - Prefix + `Ctrl-Arrow`: Resize the pane in the direction of the arrow key.
- **Swap Panes**:  
  - Prefix + `{` or `}`: Swap the current pane with the previous or next one, respectively.

---

### **Copy Mode Navigation**  
Copy mode allows you to scroll and navigate terminal output:  
- Prefix + `[` to enter copy mode.  
- Use arrow keys or `hjkl` (vim-style navigation) to move the cursor.  
- Press `q` to exit copy mode.

---

### **Tips for Efficient Navigation**
1. **Key Binding Customization**:  
   - Add custom shortcuts in `~/.tmux.conf` to streamline frequently used navigation commands.
   
2. **Mouse Support**:  
   - Enabling mouse support (`set -g mouse on`) allows switching panes, scrolling, and selecting windows interactively.

3. **Session Awareness**:  
   - Use descriptive session and window names (`tmux rename-session` or `tmux rename-window`) to simplify navigation in larger workflows.

## Managing Windows

## Managing Panes




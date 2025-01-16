# Tmux Advanced Features

Below you will find a guide on using some of tmux more advanced features. I have found the vast majority of people new to tmux were unaware of its true power and capabalities. This will give you a good overview of what tmux can do. Hopefully, you will find some of the material below to be helpful.

## Customizing tmux
Tmux, a terminal multiplexer, is a powerful tool for managing multiple terminal sessions in a single window. Customizing Tmux to suit your workflow involves understanding its configuration file, plugins, and key bindings. Below is a detailed guide to customizing Tmux like an expert.

---

### **1. Tmux Configuration Basics**
The configuration file for Tmux is located at `~/.tmux.conf`. This file is read when Tmux starts, and it controls the behavior, appearance, and key bindings of Tmux.

#### **Reload Configuration**
To reload your Tmux configuration without restarting Tmux, add this line to `~/.tmux.conf`:
```bash
bind r source-file ~/.tmux.conf \; display-message "Configuration reloaded!"
```
Pressing `Prefix + r` will reload the configuration.

#### **Set a Custom Prefix Key**
The default prefix key in Tmux is `Ctrl-b`. Many users prefer changing it to `Ctrl-a` for easier access:
```bash
set-option -g prefix C-a
unbind C-b
bind C-a send-prefix
```

---

### **2. Customizing Appearance**

#### **Set the Default Terminal**
Tmux works best with a true-color terminal. Add this to ensure compatibility:
```bash
set-option -g default-terminal "screen-256color"
```

#### **Customize the Status Bar**
The status bar displays session information and can be customized extensively.

- Change the color scheme:
  ```bash
  set-option -g status-bg black
  set-option -g status-fg green
  set-option -g status-style bold
  ```

- Display session, window, and system information:
  ```bash
  set-option -g status-left "Session: #S "
  set-option -g status-right "CPU: #(mpstat | awk '$3 ~ /[0-9.]+/ { print 100 - $13 }')% | #[fg=yellow]#(date +'%Y-%m-%d %H:%M')"
  ```

- Add plugins for battery percentage or weather (more on plugins below).

---

### **3. Managing Key Bindings**
Key bindings allow you to execute commands quickly. Below are common customizations:

#### **Split Windows**
By default, Tmux splits panes with `Prefix + %` (horizontal) and `Prefix + " ` (vertical). You can simplify this:
```bash
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %
```

#### **Resize Panes**
Enable easier resizing of panes:
```bash
bind -r H resize-pane -L
bind -r J resize-pane -D
bind -r K resize-pane -U
bind -r L resize-pane -R
```
The `-r` flag makes the bindings repeatable when held.

#### **Navigation**
Navigate panes using `vim`-like bindings:
```bash
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
unbind C-h
unbind C-j
unbind C-k
unbind C-l
```

---

### **4. Plugins for Extending Functionality**
To extend Tmux's capabilities, use the [Tmux Plugin Manager (TPM)](https://github.com/tmux-plugins/tpm).

#### **Install TPM**
1. Clone the TPM repository:
   ```bash
   git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
   ```

2. Add the following to `~/.tmux.conf`:
   ```bash
   # TPM configuration
   set -g @plugin 'tmux-plugins/tpm'
   set -g @plugin 'tmux-plugins/tmux-sensible'  # Default sensible settings
   run '~/.tmux/plugins/tpm/tpm'
   ```

3. Press `Prefix + I` to install plugins.

#### **Popular Plugins**
- **[tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect):** Save and restore sessions.
  ```bash
  set -g @plugin 'tmux-plugins/tmux-resurrect'
  ```
  Use `Prefix + Ctrl-s` to save and `Prefix + Ctrl-r` to restore.

- **[tmux-continuum](https://github.com/tmux-plugins/tmux-continuum):** Automatic session saving.
  ```bash
  set -g @plugin 'tmux-plugins/tmux-continuum'
  ```
  Combine this with `tmux-resurrect`.

- **[tmux-battery](https://github.com/tmux-plugins/tmux-battery):** Show battery status in the status bar.
  ```bash
  set -g @plugin 'tmux-plugins/tmux-battery'
  ```

- **[tmux-yank](https://github.com/tmux-plugins/tmux-yank):** Copy text to system clipboard.
  ```bash
  set -g @plugin 'tmux-plugins/tmux-yank'
  ```

---

### **5. Scripting and Hooks**

#### **Hooks**
Hooks automate tasks when events occur. For example, rename windows automatically:
```bash
set-hook -g pane-focus-in 'if-shell "[[ #{pane_current_command} == ssh ]]" "rename-window ssh" "rename-window #D"'
```

#### **Scripting**
Tmux supports scripting via shell commands. Example: create a new session with predefined windows:
```bash
tmux new-session -d -s dev
tmux rename-window -t dev:0 'Editor'
tmux send-keys -t dev:0 'vim' C-m
tmux new-window -t dev -n 'Server'
tmux send-keys -t dev:1 'python3 -m http.server' C-m
tmux attach -t dev
```

Save this to a script (e.g., `tmux-setup.sh`) and execute it to launch your customized environment.

---

### **6. Mouse Support**
To enable mouse interaction for resizing panes and selecting windows:
```bash
set-option -g mouse on
```

---

### **7. Advanced Customizations**

#### **Clipboard Integration**
On Linux, use `xclip` or `xsel` for clipboard integration:
```bash
bind C-c run "tmux show-buffer | xclip -selection clipboard"
```

On macOS:
```bash
bind C-c run "tmux save-buffer - | pbcopy"
```

#### **Session and Window Options**
Customize session behavior:
```bash
set-option -g base-index 1         # Start numbering windows at 1
setw -g automatic-rename on       # Automatically rename windows
set-option -g history-limit 10000 # Increase scrollback history
```

#### **True Color Support**
Ensure true-color support for modern terminals:
```bash
set-option -ga terminal-overrides ",xterm-256color:Tc"
```

---

### **8. Debugging and Performance**
If Tmux feels slow, optimize it by disabling features you don’t use:
- Turn off mouse support if not needed.
- Use fewer plugins or avoid plugins that poll for updates frequently.
- Increase performance by ensuring your terminal emulator supports hardware acceleration.

Debug any issues by launching Tmux with verbose logging:
```bash
tmux -vv new-session
```

---

### **9. Backup and Portability**
Keep your `~/.tmux.conf` file in version control for easy backup and portability. For example:
```bash
git init ~/.tmux-config
git add ~/.tmux.conf
git commit -m "Initial Tmux configuration"
```

---

By tailoring your Tmux configuration with these customizations, you can create a highly efficient and personalized terminal workflow. Let me know if you'd like detailed instructions on any specific aspect!


## Scripting and Automation
---

## **1. Basics of Scripting in Tmux**

### **Tmux Commands**
At its core, Tmux scripts are shell scripts that execute Tmux commands. Commonly used commands include:
- **`tmux new-session`**: Create a new session.
- **`tmux new-window`**: Open a new window within a session.
- **`tmux split-window`**: Split the current window into panes.
- **`tmux send-keys`**: Send commands to a pane or window.
- **`tmux attach-session`**: Attach to a running session.

### **Example: Basic Tmux Script**
Create a script to set up a development environment with predefined windows and commands:
```bash
#!/bin/bash

SESSION_NAME="dev"

# Start a new session in detached mode
tmux new-session -d -s $SESSION_NAME

# Create a window for editing
tmux rename-window -t $SESSION_NAME:0 'Editor'
tmux send-keys -t $SESSION_NAME:0 'vim' C-m

# Create a window for running the server
tmux new-window -t $SESSION_NAME -n 'Server'
tmux send-keys -t $SESSION_NAME:1 'python3 -m http.server' C-m

# Create a window for logs
tmux new-window -t $SESSION_NAME -n 'Logs'
tmux send-keys -t $SESSION_NAME:2 'tail -f /var/log/syslog' C-m

# Attach to the session
tmux attach -t $SESSION_NAME
```

### **Execution**
1. Save this script as `setup_tmux.sh`.
2. Make it executable:
   ```bash
   chmod +x setup_tmux.sh
   ```
3. Run the script:
   ```bash
   ./setup_tmux.sh
   ```

---

## **2. Automating Session Management**

### **Automatically Restore Sessions**
Tmux’s scripting can restore predefined session layouts for recurring tasks.

#### **Example: Session Layout Script**
```bash
#!/bin/bash

SESSION="workspace"

# Check if session already exists
tmux has-session -t $SESSION 2>/dev/null
if [ $? != 0 ]; then
  tmux new-session -d -s $SESSION

  # Set up windows
  tmux rename-window -t $SESSION:0 'Main'
  tmux send-keys -t $SESSION:0 'htop' C-m

  tmux new-window -t $SESSION:1 -n 'Code'
  tmux send-keys -t $SESSION:1 'cd ~/projects && code .' C-m

  tmux new-window -t $SESSION:2 -n 'Logs'
  tmux send-keys -t $SESSION:2 'tail -f /var/log/syslog' C-m
fi

# Attach to session
tmux attach-session -t $SESSION
```

#### **Usage**
Run this script anytime you want to restore your session. If the session already exists, it simply attaches to it.

---

## **3. Dynamic Pane and Window Creation**

Tmux scripting supports creating dynamic layouts and executing commands based on conditions.

### **Dynamic Pane Creation Script**
```bash
#!/bin/bash

SESSION="dynamic"

tmux new-session -d -s $SESSION
tmux rename-window -t $SESSION:0 'Dynamic'

# Split window into dynamic panes
for i in $(seq 1 3); do
  tmux split-window -t $SESSION:0
  tmux send-keys -t $SESSION:0.$i "echo 'Pane $i initialized'" C-m
  tmux select-layout -t $SESSION:0 tiled
done

tmux attach -t $SESSION
```

---

## **4. Automation with Hooks**

Hooks allow Tmux to execute commands automatically when certain events occur.

### **Example: Auto-Rename Windows**
Rename a window automatically based on the program running in the active pane:
```bash
set-hook -g pane-focus-in 'if-shell "[[ #{pane_current_command} == vim ]]" "rename-window Editor" "rename-window Default"'
```

### **Example: Run Commands on Session Creation**
Run a script whenever a new session starts:
```bash
set-hook -g session-created "run-shell ~/scripts/on-session-created.sh"
```

---

## **5. Automating with External Scripts**

You can integrate Tmux with other tools by running external scripts or commands.

### **Example: Dynamic Layouts from Configuration**
Load a Tmux session layout from a JSON file:
```bash
#!/bin/bash

CONFIG="session_config.json"

# Parse JSON and create Tmux session
SESSION=$(jq -r '.session_name' $CONFIG)
tmux new-session -d -s $SESSION

jq -c '.windows[]' $CONFIG | while read -r window; do
  NAME=$(echo $window | jq -r '.name')
  COMMAND=$(echo $window | jq -r '.command')

  tmux new-window -t $SESSION -n "$NAME"
  tmux send-keys -t $SESSION:"$NAME" "$COMMAND" C-m
done

tmux attach -t $SESSION
```

#### Example `session_config.json`:
```json
{
  "session_name": "project",
  "windows": [
    { "name": "Editor", "command": "vim" },
    { "name": "Server", "command": "npm start" },
    { "name": "Logs", "command": "tail -f /var/log/syslog" }
  ]
}
```

---

## **6. Scheduling Tmux Scripts**

Use `cron` or `systemd` to schedule Tmux scripts.

### **Example: Start a Session on System Boot**
Create a `tmux.service` file for systemd:
```ini
[Unit]
Description=Tmux workspace setup
After=multi-user.target

[Service]
User=yourusername
Type=idle
ExecStart=/path/to/setup_tmux.sh

[Install]
WantedBy=default.target
```
Enable the service:
```bash
sudo systemctl enable tmux.service
```

---

## **7. Tmux and SSH Integration**

Tmux scripts can automate SSH workflows, making it easy to set up remote development environments.

### **Example: Automate SSH Multiplexing**
```bash
#!/bin/bash

SESSION="remote"

tmux has-session -t $SESSION 2>/dev/null
if [ $? != 0 ]; then
  tmux new-session -d -s $SESSION
  tmux rename-window -t $SESSION:0 'Remote Server'
  tmux send-keys -t $SESSION:0 'ssh user@server.com' C-m

  tmux new-window -t $SESSION -n 'Logs'
  tmux send-keys -t $SESSION:1 'ssh user@server.com "tail -f /var/log/app.log"' C-m
fi

tmux attach -t $SESSION
```

---

## **8. Advanced Debugging and Logging**

For complex Tmux scripts, enable verbose logging:
```bash
tmux -vv new-session
```

You can also log outputs of commands:
```bash
tmux capture-pane -p > pane_output.log
```

---

## **9. Modularizing Tmux Scripts**

Break larger Tmux scripts into smaller, reusable modules:
- **Session setup:** `setup_session.sh`
- **Window setup:** `setup_window.sh`
- **Pane setup:** `setup_pane.sh`

Use a central script to orchestrate these:
```bash
#!/bin/bash
source setup_session.sh
source setup_window.sh
source setup_pane.sh
```

---

## **10. Real-World Example: Multi-Project Workspace**

Create a script to manage multiple projects in a single Tmux session:
```bash
#!/bin/bash

SESSION="projects"

tmux new-session -d -s $SESSION -n "Project 1"
tmux send-keys -t $SESSION:0 "cd ~/project1 && vim" C-m

tmux new-window -t $SESSION -n "Project 2"
tmux send-keys -t $SESSION:1 "cd ~/project2 && code ." C-m

tmux new-window -t $SESSION -n "Project 3"
tmux send-keys -t $SESSION:2 "cd ~/project3 && ./run.sh" C-m

tmux attach -t $SESSION
```

---

## Working with remote servers

## Copy-Paste in tmux

## tmux Plugins


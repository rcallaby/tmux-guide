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
---

## **Why Use `tmux` for Remote Servers?**

1. **Session Persistence**: Connections to remote servers can be unstable. `tmux` ensures your processes continue running even if you lose connectivity.
2. **Multiple Sessions**: You can manage multiple terminal sessions within a single SSH connection.
3. **Session Sharing**: Collaborate with others by sharing `tmux` sessions.
4. **Session Management**: Split windows, panes, and organize your workflow efficiently.
5. **Logging**: Capture session output to log files for debugging or documentation.

---

## **Installing `tmux`**

Before using `tmux`, ensure it is installed on the remote server.

```bash
# On Debian/Ubuntu
sudo apt update && sudo apt install tmux

# On CentOS/Red Hat
sudo yum install tmux

# On macOS (if managing remote servers locally)
brew install tmux
```

To verify the installation:
```bash
tmux -V
```

---

## **Starting and Managing Sessions**

### **1. Starting a `tmux` Session**
To start a new `tmux` session:
```bash
tmux new -s <session_name>
```
- Replace `<session_name>` with a meaningful name, e.g., `backup`, `webserver`, etc.
- Example:
  ```bash
  tmux new -s myserver
  ```

If you omit `-s <session_name>`, the session will be unnamed.

---

### **2. Detaching from a Session**
To detach from a running session without terminating it, press:
```
Ctrl-b d
```

This returns you to the normal terminal, but your `tmux` session and its processes remain active in the background.

---

### **3. Reattaching to a Session**
If your SSH connection drops or you detach, reattach to your session using:
```bash
tmux attach -t <session_name>
```
- List all active sessions:
  ```bash
  tmux list-sessions
  ```
  Example output:
  ```
  myserver: 1 windows (created Tue Jan 18 14:00:00 2025) [80x24]
  ```

  To reattach to the session `myserver`:
  ```bash
  tmux attach -t myserver
  ```

---

### **4. Renaming a Session**
To rename a session:
1. Attach to the session.
2. Press:
   ```
   Ctrl-b :
   ```
3. Type:
   ```
   rename-session <new_name>
   ```
4. Press Enter.

---

## **Window and Pane Management**

### **1. Splitting Windows**
- **Horizontal split**: Creates a new pane below the current one.
  ```bash
  Ctrl-b "
  ```
- **Vertical split**: Creates a new pane to the right.
  ```bash
  Ctrl-b %
  ```

### **2. Navigating Between Panes**
- Move between panes using:
  ```
  Ctrl-b <arrow_key>
  ```
- Resize panes:
  ```
  Ctrl-b :resize-pane -D
  Ctrl-b :resize-pane -U
  Ctrl-b :resize-pane -L
  Ctrl-b :resize-pane -R
  ```

---

### **3. Creating and Managing Windows**
Windows are like tabs in a browser, each with its own layout.

- **Create a new window**:
  ```bash
  Ctrl-b c
  ```
- **Switch between windows**:
  ```bash
  Ctrl-b n  # Next window
  Ctrl-b p  # Previous window
  Ctrl-b <number>  # Switch to specific window
  ```
- **Rename a window**:
  ```bash
  Ctrl-b ,
  ```

---

### **4. Closing Panes and Windows**
- Close the current pane or window by terminating the process running in it (e.g., type `exit`).
- To kill the entire session:
  ```bash
  tmux kill-session -t <session_name>
  ```

---

## **Advanced Features**

### **1. Persistent Workflow Across SSH Sessions**
Imagine you are monitoring logs on a remote server, and your SSH session disconnects. Here's how `tmux` ensures continuity:
1. Start `tmux`:
   ```bash
   tmux new -s logs
   ```
2. Run your command (e.g., tail logs):
   ```bash
   tail -f /var/log/syslog
   ```
3. Detach using `Ctrl-b d`.
4. Reattach later:
   ```bash
   tmux attach -t logs
   ```

---

### **2. Session Logging**
To log all output in a `tmux` session:
```bash
Ctrl-b :
```
Then type:
```bash
pipe-pane -o 'cat >> ~/tmux-session.log'
```
This writes all session output to `~/tmux-session.log`.

---

### **3. Sharing Sessions**
Share a session with another user:
1. Ensure both users can SSH into the server.
2. Start a `tmux` session and grant permissions:
   ```bash
   chmod 777 /tmp/tmux-<UID>/
   ```
   Replace `<UID>` with your user ID (`echo $UID`).
3. The second user attaches to the session:
   ```bash
   tmux attach -t <session_name>
   ```

---

### **4. Customizing `tmux`**
Modify the `~/.tmux.conf` file for personalized settings. Example:
```bash
set -g mouse on  # Enable mouse support
set -g history-limit 10000  # Increase scrollback buffer
bind r source-file ~/.tmux.conf  # Reload config
```
Reload the configuration:
```bash
Ctrl-b :
source-file ~/.tmux.conf
```

---

## **Common Scenarios for Remote Servers**

### **1. Running Long-Running Tasks**
Run a long process in a `tmux` session:
```bash
tmux new -s backup
rsync -avz /source /destination
```
Detach from the session and let the process run in the background.

### **2. Monitoring Multiple Logs**
Split panes to monitor logs from multiple files:
```bash
tmux new -s logs
Ctrl-b %
tail -f /var/log/nginx/access.log
Ctrl-b "
tail -f /var/log/nginx/error.log
```

### **3. Managing Multiple Servers**
Use a single `tmux` session to manage multiple servers:
```bash
tmux new -s admin
Ctrl-b c  # New window
ssh user@server1
Ctrl-b c
ssh user@server2
```

Switch between windows to manage servers.

---

## **Best Practices**

1. **Name Sessions Meaningfully**: Use descriptive names to identify sessions easily.
2. **Document Your Workflow**: Log critical sessions to maintain a record.
3. **Use `tmux` with SSH Multiplexing**: Combine `tmux` with SSH multiplexing (`~/.ssh/config`) to reduce SSH connection overhead.

---

## **Troubleshooting**

1. **Cannot Reattach to a Session**:
   - Ensure no other `tmux` session is attached.
   - Check active sessions:
     ```bash
     tmux list-sessions
     ```
   - Force detach others:
     ```bash
     tmux attach -t <session_name> -d
     ```

2. **Lost SSH Connection**:
   - Simply reattach to the session after reconnecting.

3. **Panes Not Resizing Correctly**:
   - Force a redraw:
     ```bash
     Ctrl-b r
     ```

---

`tmux` is indispensable for managing remote server workflows. With its session persistence, multitasking capabilities, and flexibility, you can ensure productivity and reliability when working with remote environments.

## tmux Plugins

# Working with `tmux` Plugins: A Comprehensive Guide

Plugins extend the functionality of `tmux`, providing additional features to improve productivity and customization. From simplifying workflows to enhancing visuals, `tmux` plugins can transform how you interact with remote servers.

This guide focuses exclusively on working with `tmux` plugins, including installation, management, and essential plugins tailored for remote server tasks.

---

## **Why Use Plugins with `tmux`?**

- **Enhanced Usability**: Plugins add features like session management, notifications, and theme customization.
- **Improved Efficiency**: Automate repetitive tasks and streamline workflows.
- **Ease of Monitoring**: Display server stats, logs, and other critical information directly in `tmux`.

---

## **Setting Up a Plugin Manager**

The most popular way to manage `tmux` plugins is with **[TPM (Tmux Plugin Manager)]**. TPM simplifies plugin installation, updates, and removal.

### **Installing TPM**

1. Clone the TPM repository:
   ```bash
   git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
   ```

2. Add the following lines to your `~/.tmux.conf` file:
   ```bash
   # TPM configuration
   set -g @plugin 'tmux-plugins/tpm'

   # List plugins here
   run '~/.tmux/plugins/tpm/tpm'
   ```

3. Reload your `~/.tmux.conf`:
   ```bash
   tmux source-file ~/.tmux.conf
   ```

---

## **Installing and Managing Plugins**

### **1. Installing Plugins**
- Add the plugin to the `~/.tmux.conf` file under the `@plugin` section. For example:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-sensible'
  ```
- Reload the configuration:
  ```bash
  tmux source-file ~/.tmux.conf
  ```
- Press `Ctrl-b I` to install the plugin. This fetches and installs all listed plugins.

---

### **2. Updating Plugins**
- Press `Ctrl-b U` to update all plugins to their latest versions.

---

### **3. Removing Plugins**
1. Remove the plugin entry from your `~/.tmux.conf`.
2. Reload the configuration:
   ```bash
   tmux source-file ~/.tmux.conf
   ```
3. Press `Ctrl-b alt-u` to uninstall the removed plugin.

---

## **Essential Plugins for Remote Servers**

### **1. `tmux-sensible`**
- Repository: [tmux-plugins/tmux-sensible](https://github.com/tmux-plugins/tmux-sensible)
- **Why Use It?** Simplifies `tmux` configuration by enabling a collection of sane defaults.
- **Features**:
  - Optimized keybindings.
  - Automatically sets up clipboard and mouse integration.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-sensible'
  ```

---

### **2. `tmux-resurrect`**
- Repository: [tmux-plugins/tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)
- **Why Use It?** Persist `tmux` sessions, panes, and processes across reboots.
- **Features**:
  - Restore sessions exactly as they were.
  - Critical for maintaining workflows on remote servers after disconnects or reboots.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-resurrect'
  ```
- Save sessions:
  ```bash
  Ctrl-b Ctrl-s
  ```
- Restore sessions:
  ```bash
  Ctrl-b Ctrl-r
  ```

---

### **3. `tmux-continuum`**
- Repository: [tmux-plugins/tmux-continuum](https://github.com/tmux-plugins/tmux-continuum)
- **Why Use It?** Automates saving and restoring sessions.
- **Features**:
  - Periodically saves sessions without manual intervention.
  - Seamlessly integrates with `tmux-resurrect`.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-continuum'
  set -g @continuum-restore 'on'
  ```

---

### **4. `tmux-cpu`**
- Repository: [tmux-plugins/tmux-cpu](https://github.com/tmux-plugins/tmux-cpu)
- **Why Use It?** Displays CPU usage and load averages directly in the `tmux` status bar.
- **Features**:
  - Monitor CPU usage without leaving `tmux`.
  - Useful for diagnosing performance issues on remote servers.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-cpu'
  ```

---

### **5. `tmux-battery`**
- Repository: [tmux-plugins/tmux-battery](https://github.com/tmux-plugins/tmux-battery)
- **Why Use It?** Shows battery status in the status bar.
- **Features**:
  - Essential for managing workflows on laptops while connected to remote servers.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-battery'
  ```

---

### **6. `tmux-yank`**
- Repository: [tmux-plugins/tmux-yank](https://github.com/tmux-plugins/tmux-yank)
- **Why Use It?** Simplifies copying text from `tmux` to the system clipboard.
- **Features**:
  - Copy directly into the clipboard using:
    ```bash
    Ctrl-b y
    ```
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-yank'
  ```

---

### **7. `tmux-themepack`**
- Repository: [jimeh/tmux-themepack](https://github.com/jimeh/tmux-themepack)
- **Why Use It?** Provides a collection of pre-designed themes for the `tmux` status bar.
- **Features**:
  - Quickly switch between visually appealing themes.
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'jimeh/tmux-themepack'
  ```

---

### **8. `tmux-logging`**
- Repository: [tmux-plugins/tmux-logging](https://github.com/tmux-plugins/tmux-logging)
- **Why Use It?** Enables easy logging of session output.
- **Features**:
  - Log all terminal activity for later analysis.
  - Start/stop logging:
    ```bash
    Ctrl-b Shift-p
    ```
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-logging'
  ```

---

### **9. `tmux-urlview`**
- Repository: [tmux-plugins/tmux-urlview](https://github.com/tmux-plugins/tmux-urlview)
- **Why Use It?** Extracts and lists URLs in the current `tmux` session.
- **Features**:
  - Opens URLs directly from `tmux`.
  - Press:
    ```bash
    Ctrl-b u
    ```
- Add to `~/.tmux.conf`:
  ```bash
  set -g @plugin 'tmux-plugins/tmux-urlview'
  ```

---

## **Customizing Plugins**

### **Plugin Configuration**
Many plugins allow additional customization through `@` variables in the `~/.tmux.conf`. For example, configuring `tmux-continuum`:
```bash
set -g @continuum-restore 'on'
set -g @resurrect-capture-pane-contents 'on'
```

Refer to individual plugin documentation for configuration options.

---

## **Troubleshooting Plugins**

1. **Plugin Not Loading**:
   - Verify `~/.tmux.conf` for typos.
   - Reload `~/.tmux.conf`:
     ```bash
     tmux source-file ~/.tmux.conf
     ```

2. **TPM Not Installing Plugins**:
   - Ensure TPM is installed correctly at `~/.tmux/plugins/tpm`.
   - Check internet connectivity on the remote server.

3. **Conflicting Plugins**:
   - Remove conflicting plugins from `~/.tmux.conf`.
   - Reload configuration and reinstall plugins.

---

`tmux` plugins are invaluable for tailoring `tmux` to your specific needs, especially when managing remote servers. With TPM and the right plugins, you can create a highly efficient, personalized workflow that simplifies server administration, session management, and monitoring.


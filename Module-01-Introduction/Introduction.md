# Introduction

Welcome to the *Tmux Guide*, your comprehensive resource for mastering **tmux**, the powerful terminal multiplexer that revolutionizes the way you manage your command-line workflows. Whether you're new to tmux or looking to refine your skills, this guide will walk you through its core concepts, from creating and navigating sessions, panes, and windows to advanced customizations and scripting. Designed for developers, sysadmins, and power users alike, tmux allows you to multitask efficiently, keep sessions alive, and supercharge your productivity in any terminal environment. Let’s dive in and unlock the full potential of tmux!

## What is tmux?

Tmux, short for "terminal multiplexer," is a versatile command-line tool that enables users to manage multiple terminal sessions within a single window. It allows you to split your terminal into panes, switch between windows seamlessly, and keep sessions running in the background even after disconnecting from the system. This makes tmux an indispensable tool for developers, system administrators, and anyone working extensively in a Unix-like terminal environment. It supports features like session persistence, window management, and extensive customization, making it a powerful alternative to traditional terminal workflows.

**A Brief History and Applications**  
Tmux was created by Nicholas Marriott in 2007 as a modern alternative to GNU Screen, an older terminal multiplexer. Written in C, tmux was designed with simplicity, performance, and extensibility in mind. Over the years, it has gained popularity due to its active development, robust features, and better support for modern workflows compared to its predecessors. Tmux is widely used in fields like software development, DevOps, and system administration, where managing multiple terminal sessions or maintaining long-running processes is crucial. Its compatibility with SSH and ease of integration into scripts and workflows make it a favorite among professionals who value efficiency and multitasking in their command-line environments.

## Why use tmux?
  
Tmux is a game-changer for professionals across all roles in the Information Technology field due to its ability to streamline workflows and maximize productivity. System administrators can use tmux to monitor multiple servers simultaneously, keeping long-running processes alive even after disconnection or rebooting their local machines. Developers benefit from its ability to create multiple panes and windows, allowing them to code, debug, and test applications side by side in a single terminal session. For network engineers, tmux is invaluable for managing multiple SSH sessions to various devices, ensuring seamless multitasking without the need for multiple terminal windows. Its session persistence means that you can reconnect to ongoing tasks, reducing downtime and enhancing efficiency in daily operations.

Moreover, tmux is equally beneficial in roles focused on security, DevOps, and data analysis. Security professionals can use tmux to run tools like Nmap, Wireshark, or custom scripts while simultaneously analyzing logs or capturing traffic. DevOps engineers leverage tmux to monitor CI/CD pipelines, manage Docker containers, and troubleshoot cloud-based infrastructure in real time. Data analysts working in command-line environments find tmux useful for running queries, visualizing results, and debugging workflows in parallel. By providing a unified, organized, and efficient environment for multitasking, tmux becomes an essential tool for anyone in IT, regardless of their specialization. Its adaptability and support for modern workflows ensure it can be tailored to fit the unique demands of any technical role.

## Installing tmux
---

### **Linux**
Tmux is widely available on most Linux distributions through their package managers. Here's how to install it on the most common distros:

#### **Debian/Ubuntu**
1. **Update the package lists**:
   ```bash
   sudo apt update
   ```
2. **Install tmux**:
   ```bash
   sudo apt install tmux -y
   ```
3. **Verify the installation**:
   ```bash
   tmux -V
   ```
   This should output the installed version of tmux.

#### **CentOS/RHEL (using `dnf` or `yum`)**
1. **Ensure EPEL repository is enabled**:
   - For CentOS 8+ and RHEL 8+, use:
     ```bash
     sudo dnf install epel-release -y
     ```
   - For CentOS 7 and RHEL 7:
     ```bash
     sudo yum install epel-release -y
     ```
2. **Install tmux**:
   ```bash
   sudo yum install tmux -y   # For CentOS 7
   sudo dnf install tmux -y   # For CentOS/RHEL 8+
   ```
3. **Verify the installation**:
   ```bash
   tmux -V
   ```

#### **Fedora**
1. **Install tmux via dnf**:
   ```bash
   sudo dnf install tmux -y
   ```
2. **Verify the installation**:
   ```bash
   tmux -V
   ```

#### **Arch Linux**
1. **Install tmux using `pacman`**:
   ```bash
   sudo pacman -S tmux
   ```
2. **Verify the installation**:
   ```bash
   tmux -V
   ```

#### **openSUSE**
1. **Install tmux via `zypper`**:
   ```bash
   sudo zypper install tmux
   ```
2. **Verify the installation**:
   ```bash
   tmux -V
   ```

#### **Building Tmux from Source on Linux**
If your package manager doesn’t provide the latest version:
1. **Install dependencies**:
   ```bash
   sudo apt install -y build-essential libevent-dev ncurses-dev
   ```
2. **Download the latest source code**:
   Visit the [tmux GitHub releases page](https://github.com/tmux/tmux/releases) and download the latest `.tar.gz` file.
   ```bash
   wget https://github.com/tmux/tmux/releases/download/<version>/tmux-<version>.tar.gz
   ```
3. **Extract and build**:
   ```bash
   tar -xvzf tmux-<version>.tar.gz
   cd tmux-<version>
   ./configure
   make
   sudo make install
   ```
4. **Verify the installation**:
   ```bash
   tmux -V
   ```

---

### **macOS**
There are several ways to install Tmux on macOS:

#### **Using Homebrew (Recommended)**
1. **Install Homebrew (if not already installed)**:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. **Install tmux**:
   ```bash
   brew install tmux
   ```
3. **Verify the installation**:
   ```bash
   tmux -V
   ```

#### **Building Tmux from Source**
1. **Install Xcode command-line tools**:
   ```bash
   xcode-select --install
   ```
2. **Install dependencies via Homebrew**:
   ```bash
   brew install libevent ncurses
   ```
3. **Download and build the source code** (as described in the Linux section).

---

### **Windows**
On Windows, you can use **tmux** via WSL (Windows Subsystem for Linux) or install it as part of Cygwin or MSYS2.

#### **Using Windows Subsystem for Linux (WSL)**
1. **Install WSL and a Linux Distribution**:
   - Open PowerShell (as Administrator):
     ```powershell
     wsl --install
     ```
   - Install a Linux distribution (e.g., Ubuntu) from the Microsoft Store.
2. **Open your WSL terminal**:
   - Launch your Linux distro (e.g., Ubuntu).
3. **Follow Linux instructions above** to install tmux within the WSL environment.

#### **Using Cygwin**
1. **Download and install Cygwin**:
   - Download the Cygwin installer from [cygwin.com](https://cygwin.com/).
2. **Install tmux during Cygwin setup**:
   - In the Cygwin package selection screen, search for `tmux` and mark it for installation.
3. **Finish installation and run tmux**:
   ```bash
   tmux -V
   ```

#### **Using MSYS2**
1. **Install MSYS2**:
   - Download it from [msys2.org](https://www.msys2.org/).
2. **Update the package database**:
   ```bash
   pacman -Syu
   ```
3. **Install tmux**:
   ```bash
   pacman -S tmux
   ```
4. **Verify the installation**:
   ```bash
   tmux -V
   ```

---

### **FreeBSD**
Tmux is included in the FreeBSD Ports Collection:
1. **Install tmux via `pkg`**:
   ```bash
   sudo pkg install tmux
   ```
2. **Verify the installation**:
   ```bash
   tmux -V
   ```

---

### **OpenBSD**
Tmux is part of the base system on OpenBSD, so no installation is required. Simply run:
```bash
tmux -V
```

---

### **NetBSD**
1. **Install tmux via `pkgsrc`**:
   ```bash
   pkgin install tmux
   ```
2. **Verify the installation**:
   ```bash
   tmux -V
   ```

---

## First steps using tmux

### **1. Starting Tmux**
To launch Tmux, simply open your terminal and type:
```bash
tmux
```
This starts a new Tmux session. You’ll notice a status bar at the bottom of the screen. By default, this bar displays:
- The current session name (e.g., `[0]`).
- Active window (e.g., `0:bash`).
- Current time and date.

**Exiting Tmux:** To close a Tmux session, type `exit` in the terminal or close all windows within the session.

---

### **2. Tmux Basics**
Tmux uses **prefix commands** to execute its features. By default, the prefix is `Ctrl+b`. After pressing the prefix, you issue commands. For example:
- `Ctrl+b c` creates a new window.
- `Ctrl+b d` detaches the session.

#### Common Prefix Commands:
| Command            | Key Combination   | Description                                 |
|--------------------|-------------------|---------------------------------------------|
| **New Window**     | `Ctrl+b c`       | Creates a new window.                      |
| **Switch Windows** | `Ctrl+b n`       | Moves to the next window.                  |
|                    | `Ctrl+b p`       | Moves to the previous window.              |
| **List Windows**   | `Ctrl+b w`       | Displays a list of all windows.            |
| **Detach Session** | `Ctrl+b d`       | Detaches the current Tmux session.         |
| **Kill Session**   | `Ctrl+b :`       | Enter command mode, then type `kill-session`. |

---

### **3. Working with Sessions**
#### **Creating a New Session**
You can start a new session with a custom name:
```bash
tmux new -s session_name
```

#### **Detaching from a Session**
If you want to leave a Tmux session but keep it running:
```bash
Ctrl+b d
```
You can reattach later.

#### **Listing Sessions**
To see all active Tmux sessions, run:
```bash
tmux list-sessions
```

#### **Reattaching to a Session**
To return to a detached session:
```bash
tmux attach -t session_name
```

#### **Renaming a Session**
You can rename the current session:
```bash
Ctrl+b $
```
Type the new session name and press Enter.

---

### **4. Working with Windows**
Windows in Tmux are like tabs in a web browser. Each window can run a separate shell or process.

#### **Creating a New Window**
Press:
```bash
Ctrl+b c
```

#### **Switching Between Windows**
- `Ctrl+b n` for the next window.
- `Ctrl+b p` for the previous window.
- `Ctrl+b 0-9` to jump directly to a window by number.

#### **Renaming a Window**
To rename a window:
```bash
Ctrl+b ,
```
Type a new name and press Enter.

#### **Closing a Window**
Simply type `exit` in the terminal running inside the window, or press `Ctrl+d`.

---

### **5. Splitting Panes**
Panes allow you to divide a window into multiple regions. You can have horizontal or vertical splits.

#### **Create a Vertical Split**
Press:
```bash
Ctrl+b %
```

#### **Create a Horizontal Split**
Press:
```bash
Ctrl+b "
```

#### **Navigating Between Panes**
Use the arrow keys after pressing the prefix:
```bash
Ctrl+b [Arrow Key]
```

#### **Resizing Panes**
1. Press `Ctrl+b` and hold down one of the following keys:
   - `Ctrl+b <Arrow Key>`: Resize pane in the corresponding direction.

#### **Closing a Pane**
Focus on the pane you want to close and type:
```bash
exit
```

---

### **6. Scrolling and Copy Mode**
Tmux allows you to scroll back and copy text directly from the terminal.

#### **Enter Copy Mode**
Press:
```bash
Ctrl+b [
```

#### **Scroll in Copy Mode**
- Use the arrow keys or `PgUp`/`PgDn` to scroll.
- Press `q` to exit copy mode.

#### **Copying Text**
1. Enter copy mode: `Ctrl+b [`.
2. Navigate to the beginning of the text using arrow keys.
3. Press `Space` to start selecting.
4. Move the cursor to highlight the desired text.
5. Press `Enter` to copy.

#### **Pasting Text**
To paste copied text:
```bash
Ctrl+b ]
```

---

### **7. Customizing Tmux**
You can customize Tmux through a configuration file:
1. Create a `.tmux.conf` file in your home directory:
   ```bash
   touch ~/.tmux.conf
   ```
2. Add customizations. For example:
   ```bash
   # Use Ctrl+a as the prefix
   set-option -g prefix C-a
   unbind-key C-b
   bind-key C-a send-prefix

   # Enable mouse support
   set-option -g mouse on
   ```
3. Reload the configuration:
   ```bash
   tmux source-file ~/.tmux.conf
   ```

---

### **8. Exiting Tmux**
- To close all panes and windows, type `exit` in each terminal or use `Ctrl+d`.
- To kill a session:
  ```bash
  tmux kill-session -t session_name
  ```

---

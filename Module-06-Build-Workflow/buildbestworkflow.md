# Build your ideal tmux workflow

## Setting up your workflow
Setting up a professional workflow with tmux, Vim/Neovim, and ancillary tools like SSH can significantly enhance your productivity as a developer. This guide provides a detailed, step-by-step approach to configuring and integrating these tools, complete with code examples and reliable references.

## Prerequisites

Ensure that the following tools are installed on your system:

- **tmux**: A terminal multiplexer that allows you to manage multiple terminal sessions within a single window.
- **Vim or Neovim**: Highly configurable text editors. Neovim is an extension of Vim with additional features.
- **SSH**: Secure Shell for remote server access.

Installation commands vary by operating system. For example, on Ubuntu, you can install these tools using:

```bash
sudo apt-get update
sudo apt-get install tmux neovim openssh-client
```


## Step 1: Configure tmux

### 1.1 Create a tmux Configuration File

tmux uses a configuration file located at `~/.tmux.conf`. Create and open this file:

```bash
touch ~/.tmux.conf
nano ~/.tmux.conf
```


### 1.2 Basic tmux Settings

Add the following settings to your `~/.tmux.conf` file to enhance usability:

```bash
# Set prefix key to Ctrl+a
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# Enable mouse support
set -g mouse on

# Split panes using | and -
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# Switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
```


These configurations set the prefix key to `Ctrl+a`, enable mouse support, and customize pane splitting and navigation. citeturn0search10

### 1.3 Automate tmux Session Creation

To streamline your workflow, create scripts to automate tmux session creation for specific projects. For example, create a script `tmux-myproject` with the following content:

```bash
#!/bin/bash

SESSION_NAME=myproject
PROJECT_PATH=~/projects/myproject

tmux has-session -t $SESSION_NAME 2>/dev/null

if [ $? != 0 ]; then
    tmux new-session -s $SESSION_NAME -n editor -d
    tmux send-keys -t $SESSION_NAME "cd $PROJECT_PATH" C-m
    tmux send-keys -t $SESSION_NAME "nvim" C-m
    tmux split-window -v -p 30 -t $SESSION_NAME
    tmux split-pane -h -t $SESSION_NAME:1.2
    tmux send-keys -t $SESSION_NAME:1.2 "cd $PROJECT_PATH" C-m
fi

tmux attach -t $SESSION_NAME
```


This script checks if a tmux session named `myproject` exists. If not, it creates one with predefined panes and starts Neovim in the main pane. citeturn0search0

## Step 2: Configure Neovim

### 2.1 Install a Plugin Manager

Use a plugin manager like [vim-plug](https://github.com/junegunn/vim-plug) to manage Neovim plugins. Install it with:

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```


### 2.2 Configure Neovim with Essential Plugins

Create or edit the Neovim configuration file at `~/.config/nvim/init.vim` and add the following:

```vim
call plug#begin('~/.vim/plugged')

" File explorer
Plug 'preservim/nerdtree'

" Status line
Plug 'vim-airline/vim-airline'

" Git integration
Plug 'tpope/vim-fugitive'

" Syntax highlighting
Plug 'sheerun/vim-polyglot'

" Auto-completion
Plug 'neoclide/coc.nvim', {'branch': 'release'}

call plug#end()

" Map leader key to space
let mapleader = "\<Space>"

" NERDTree toggle
nnoremap <leader>n :NERDTreeToggle<CR>

" Save file with Ctrl+s
nnoremap <C-s> :w<CR>
inoremap <C-s> <Esc>:w<CR>l

" Close buffer with Ctrl+w
nnoremap <C-w> :bd<CR>
```


These configurations set up essential plugins and key mappings to enhance your Neovim experience. citeturn0search8

### 2.3 Install Plugins

Launch Neovim and run `:PlugInstall` to install the specified plugins.

## Step 3: Integrate tmux and Neovim

### 3.1 Seamless Navigation Between tmux and Neovim

To navigate seamlessly between tmux panes and Neovim splits, use the [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) plugin. Add it to your `init.vim`:

```vim
Plug 'christoomey/vim-tmux-navigator'
```


Ensure your `~/.tmux.conf` includes:

```bash
# Smart pane switching with awareness of vim splits
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^(vim|n?vim)$'"

bind -n C-h if-shell "$is_vim" 'send-keys C-h'  'select-pane -L'
bind -n C-j if-shell "$is_vim" 'send-keys C-j'  'select-pane -D'
bind -n C-k if-shell "$is_vim 
```


## Automating your workflow
Automating your development workflow by scripting `tmux` sessions can significantly enhance productivity and consistency across projects. This guide will walk you through creating a Bash script that sets up a `tmux` session with customized windows and panes, tailored to your project's needs.

## Prerequisites

Ensure that `tmux` is installed on your system. If not, you can install it using your package manager. For example, on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install tmux
```


## Step 1: Create the Bash Script

1. **Open your terminal**.

2. **Create a new Bash script file**, e.g., `setup_tmux_session.sh`:

   ```bash
   touch setup_tmux_session.sh
   ```


3. **Make the script executable**:

   ```bash
   chmod +x setup_tmux_session.sh
   ```


4. **Open the script in your preferred text editor**:

   ```bash
   nano setup_tmux_session.sh
   ```


## Step 2: Define the Script's Content

In the `setup_tmux_session.sh` file, add the following content:

```bash
#!/bin/bash

# Define session name and project path
SESSION_NAME="my_project"
PROJECT_PATH="$HOME/path/to/your/project"

# Check if the tmux session already exists
tmux has-session -t $SESSION_NAME 2>/dev/null

if [ $? != 0 ]; then
    # Create a new detached tmux session
    tmux new-session -d -s $SESSION_NAME -n editor

    # Navigate to project directory and open Neovim in the main window
    tmux send-keys -t $SESSION_NAME "cd $PROJECT_PATH" C-m
    tmux send-keys -t $SESSION_NAME "nvim" C-m

    # Split the window horizontally for a terminal
    tmux split-window -h -t $SESSION_NAME
    tmux send-keys -t $SESSION_NAME "cd $PROJECT_PATH" C-m

    # Split the terminal pane vertically for running a server or other tasks
    tmux split-window -v -t $SESSION_NAME
    tmux send-keys -t $SESSION_NAME "cd $PROJECT_PATH" C-m

    # Select the main editor pane
    tmux select-pane -t $SESSION_NAME:0.0
fi

# Attach to the tmux session
tmux attach -t $SESSION_NAME
```


**Explanation**:

- The script defines a `tmux` session named `my_project` and sets the project directory.

- It checks if the session already exists to prevent duplicate sessions.

- If the session doesn't exist, it creates a new detached session with a window named `editor`.

- In the `editor` window, it navigates to the project directory and opens Neovim.

- It then splits the window horizontally to create a pane for terminal commands and further splits the new pane vertically for running a server or other background tasks.

- Finally, it selects the main editor pane and attaches to the `tmux` session.

## Step 3: Customize the Script

Modify the following variables to suit your project:

- `SESSION_NAME`: Set this to a unique name for your project session.

- `PROJECT_PATH`: Set this to the absolute path of your project directory.

You can also customize the panes to run specific commands, such as starting a development server or monitoring logs.

## Step 4: Execute the Script

1. **Navigate to the directory containing `setup_tmux_session.sh`**:

   ```bash
   cd /path/to/your/script
   ```


2. **Run the script**:

   ```bash
   ./setup_tmux_session.sh
   ```


This will set up and attach to the `tmux` session as defined in your script.

## Additional Tips

- **Automate Script Execution**: To further streamline your workflow, consider adding the script to your system's startup applications or creating an alias in your shell configuration file (e.g., `.bashrc` or `.zshrc`) for quick execution.

- **Use `tmux` Plugins**: Enhance your `tmux` experience by using plugins like `tmux-resurrect` to save and restore sessions, or `tmuxinator` to manage complex session configurations.

- **Explore `tmux` Scripting**: For more advanced automation, delve into `tmux`'s scripting capabilities to create intricate workflows tailored to your development needs.

By automating your `tmux` sessions, you can ensure a consistent and efficient development environment, reducing setup time and minimizing errors.

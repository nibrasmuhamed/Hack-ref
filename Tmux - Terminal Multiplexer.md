# Tmux 
tmux, the terminal multiplexer, is easily one of the most used tools by the Linux community (and not just pentesters!). While not a malicious tool, tmux makes running simultaneous tasks throughout a pentest incredibly easy. In this primer room, we'll walk through the process of installing and using some of the most common key combinations used in tmux

tmux can be installed in debian based distro using this command
```
sudo apt install tmux
```
[Tmux cheatsheet](https://acloudguru.com/blog/engineering/tmux-cheat-sheet?utm_source=legacyla&utm_medium=redirect&utm_campaign=one_platform)


Here we have keycombinations, it's called key bindings. by default it is set to `control + B`, just tap key bindigs and release, now go for your pereference.


### Panes
Panes are instace of splitview
we can split terminal (pane) vertical and horizontal
vertical split
```
cntl+B %
```
horizontal split
```
cntl + B "
```
moving along panes are supercool
to move left 
```
cntl + B Leftarrow
```
exit 
```
exit or cntl + b x
```

### windows
windows are like new terminal tabs
to create new window 
```
cntl + B c
```
to switch windows, just press keybindigs and window index(number pointed to window on bottom)
```
cntl + B <num>
```
renaming window
```
cntl + B ,
```
to kill one window `exit`
```
cntl + b &
```
to list windows
```
cntl + b w
```
swich between windows
```
cntl + b N
cntl + b p
```
to go next & previous window repectively

### tmux session
session are set of terminal or windows opened by tmux at time. it can be backgrounded.
session are the real power of tmux.session can be called windows in real terminal emulator scenario

to detash a session
```
cntl + B d
```
to see all sessions running on background
```
tmux ls
```
to attach a session
```
tmux attach -t 0 <0 can be differs, as you have the ability to rename sessions>
```
to rename session
```
tmux rename-session -t <id of session> newname
```
to name a session on beginning
```
tmux new -s name
```
to kill a session
```
tmux kill-session -t id
```
move to next session
```
cntl + b )
```
move to previous session
```
cntl + b (
```
 
 ### tmux copy mode
 
 to enter copy mode
 ```
 cntl + b [
 ```
 to paste from buffer
 ```
 cntl + b ]
 ```
 list paste buffers `#`
 start selection `space`
 copy selection `return`
 search `/`
 to quit `q`
 clear selection `esc`
 go to top `g`
 go to bottom `G`

Tmux Options

```color
In tmux, a session is displayed on screen by a client and all sessions are managed by a single server.  The
     server and each client are separate processes which communicate through a socket in /tmp.

     The options are as follows:

     -2            Force tmux to assume the terminal supports 256 colours.

     -C            Start in control mode (see the CONTROL MODE section).  Given twice (-CC) disables echo.

     -c shell-command
                   Execute shell-command using the default shell.  If necessary, the tmux server will be started
                   to retrieve the default-shell option.  This option is for compatibility with sh(1) when tmux
                   is used as a login shell.

     -f file       Specify an alternative configuration file.  By default, tmux loads the system configuration
                   file from /etc/tmux.conf, if present, then looks for a user configuration file at
                   ~/.tmux.conf.

                   The configuration file is a set of tmux commands which are executed in sequence when the
                   server is first started.  tmux loads configuration files once when the server process has
                   started.  The source-file command may be used to load a file later.

                   tmux shows any error messages from commands in configuration files in the first session cre‐
                   ated, and continues to process the rest of the configuration file.

     -L socket-name
                   tmux stores the server socket in a directory under TMUX_TMPDIR or /tmp if it is unset.  The
                   default socket is named default.  This option allows a different socket name to be specified,
                   allowing several independent tmux servers to be run.  Unlike -S a full path is not necessary:
                   the sockets are all created in the same directory.

                   If the socket is accidentally removed, the SIGUSR1 signal may be sent to the tmux server
                   process to recreate it (note that this will fail if any parent directories are missing).

     -l            Behave as a login shell.  This flag currently has no effect and is for compatibility with
                   other shells when using tmux as a login shell.

     -S socket-path
                   Specify a full alternative path to the server socket.  If -S is specified, the default socket
                   directory is not used and any -L flag is ignored.

     -u            Write UTF-8 output to the terminal even if the first environment variable of LC_ALL,
                   LC_CTYPE, or LANG that is set does not contain "UTF-8" or "UTF8".

     -v            Request verbose logging.  Log messages will be saved into tmux-client-PID.log and
                   tmux-server-PID.log files in the current directory, where PID is the PID of the server or
                   client process.  If -v is specified twice, an additional tmux-out-PID.log file is generated
                   with a copy of everything tmux writes to the terminal.

                   The SIGUSR2 signal may be sent to the tmux server process to toggle logging between on (as if
                   -v was given) and off.

     -V            Report the tmux version.

     command [flags]
```

Tmux Default KeyBindings

```color
		   C-b         Send the prefix key (C-b) through to the application.
           C-o         Rotate the panes in the current window forwards.
           C-z         Suspend the tmux client.
           !           Break the current pane out of the window.
           "           Split the current pane into two, top and bottom.
           #           List all paste buffers.
           $           Rename the current session.
           %           Split the current pane into two, left and right.
           &           Kill the current window.
           '           Prompt for a window index to select.
           (           Switch the attached client to the previous session.
           )           Switch the attached client to the next session.
           ,           Rename the current window.
           -           Delete the most recently copied buffer of text.
           .           Prompt for an index to move the current window.
           0 to 9      Select windows 0 to 9.
           :           Enter the tmux command prompt.
           ;           Move to the previously active pane.
           =           Choose which buffer to paste interactively from a list.
           ?           List all key bindings.
           D           Choose a client to detach.
           L           Switch the attached client back to the last session.
           [           Enter copy mode to copy text or view the history.
           ]           Paste the most recently copied buffer of text.
           c           Create a new window.
           d           Detach the current client.
           f           Prompt to search for text in open windows.
           i           Display some information about the current window.
           l           Move to the previously selected window.
           m           Mark the current pane (see select-pane -m).
           M           Clear the marked pane.
           n           Change to the next window.
           o           Select the next pane in the current window.
           p           Change to the previous window.
           q           Briefly display pane indexes.
           r           Force redraw of the attached client.
           s           Select a new session for the attached client interactively.
           t           Show the time.
           w           Choose the current window interactively.
           x           Kill the current pane.
           z           Toggle zoom state of the current pane.
           {           Swap the current pane with the previous pane.
           }           Swap the current pane with the next pane.
           ~           Show previous messages from tmux, if any.
           Page Up     Enter copy mode and scroll one page up.
           Up, Down
           Left, Right
                       Change to the pane above, below, to the left, or to the right of the current pane.
           M-1 to M-5  Arrange panes in one of the five preset layouts: even-horizontal, even-vertical, main-
                       horizontal, main-vertical, or tiled.
           Space       Arrange the current window in the next preset layout.
           M-n         Move to the next window with a bell or activity marker.
           M-o         Rotate the panes in the current window backwards.
           M-p         Move to the previous window with a bell or activity marker.
           C-Up, C-Down
           C-Left, C-Right
                       Resize the current pane in steps of one cell.
           M-Up, M-Down
           M-Left, M-Right
                       Resize the current pane in steps of five cells.

     Key bindings may be changed with the bind-key and unbind-key commands.
```

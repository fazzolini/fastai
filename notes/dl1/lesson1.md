# Lesson 1

## Setting Up Development Workflow

This section has no direct connection to deep learning. Here are some useful tips and tricks to organize efficient learning and development.

### Set up persistent sessions with `tmux`

* `tmux -a` attach to a current session
* `Ctrl+b s` list all sessions
* `Ctrl+b d` detach from a session
* `Ctrl+b w` list windows in a session
* `tmux new -s session_name` create a new session with a given name
* `tmux a -t session_name` attach to a given session
* `tmux kill-session -t session_name` kill a given session
* `killall tmux` kill all tmux sessions

### Run Jupyter notebook on the server
`jupyter notebook --no-browser` launch an instance of Jupyter notebook on the server, then map ports (see instructions below) to connect from a remote machine over ssh.

### Use `ssh` to connect to a remote server

[Install and configure SSH](http://linux-sys-adm.com/ubuntu-16.04-lts-how-to-install-and-configure-ssh/). Note:

* **server settings** are in file `/etc/ssh/sshd_config`
* **client settings** are in file `/etc/ssh/ssh_config`.

`ssh -v username@192.1.1.1` connect to a remote server (needs public keys set up on both machines)

`ssh -N -L localhost:8889:localhost:8888 username@192.1.1.1` map over ssh `localhost:8888` on a server to `localhost:8889` on a local machine. Use to connect to a Jupyter notebook server running on the server.

### Set up PyCharm to [work with files remotely](https://blog.jetbrains.com/pycharm/2015/03/feature-spotlight-python-remote-development-with-pycharm/) on the server

`ftp -p 192.168.8.168 20000` to connect to FTP running on the server on port `20000`. First need to [set up vsftpd](https://www.digitalocean.com/community/tutorials/how-to-set-up-vsftpd-for-a-user-s-directory-on-ubuntu-16-04) on the server.

This feature allows to use full power of the IDE to work with files on the remote server. Autoupload can be set up so that all local changes will be automatically uploaded to remote. Then you can push local changes to Github and, because remote is synced to local by PyCharm, `git status` on the server will give `OK`, so this saves the hassle of pushing and pulling on the server. 

### Use `grep` to count the number of files in a folder

```bash
!ls -l {PATH} | grep ^[^dt] | wc -l
```

* The first part lists all files in a directory in a long format, starting with the access rights which have the first letter `d` if it is a directory and `-` if it is a file.
* Second, he output is [piped](http://www.linfo.org/pipes.html) into `grep` that will match all the lines that **do not start** with `d` or `t`. This means matching all the files in a given folder. The `t` is needed because the first line of output of `ls -l` has the total number of items, so the `t` in `^[^dt]` will address this corner case.
* Lastly, the output of `grep` is piped into `wc -l` command that counts the number of lines.

```bash 
ls -ap | grep -v / | egrep "^\.."
```

* list all hidden files in a directory
* `ls -ap` lists all (`-a`) files and directories, it puts `/` after path of each directory (`-p`)
* `grep -v /` does ***inverse match*** of directories. It matches everything that is not a directory.
* `egrep "^\.."` matches all lines that start with `.`

```bash 
ls -ap | egrep "^\..*/$""
```

* list only hidden directories


### Linux: Useful Nuggets

* [www.linfo.org](http://www.linfo.org/) is a very good source of information about Linux
* [https://explainshell.com](www.explainshell.com) is *awesome* for explaining bash commands.
* [Pipe](http://www.linfo.org/pipes.html) is a type of [redirection](http://www.linfo.org/redirection.html).
* Each command has [3 streams](https://ryanstutorials.net/linuxtutorial/piping.php): standard input `STDIN(0)`, standard output `STDOUT(1)` and standard error `STDERR(2)`. 
* `<` input redirect, `>` output redirect (overwrite), `>>` append.
* `2>`, `2>>` standard error redirect operators. `2` in this case is a *file descriptor* of `STDERR`.
* [wildcards](http://www.linfo.org/wildcard.html) – `*`, `?` and `[]`.
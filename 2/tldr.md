# [tldr](https://github.com/tldr-pages/tldr)

## top

```shell
tldr top

top

Display dynamic real-time information about running processes.
More information: <https://ss64.com/osx/top.html>.

- Start top, all options are available in the interface:
    top

- Start top sorting processes by internal memory size (default order - process ID):
    top -o mem

- Start top sorting processes first by CPU, then by running time:
    top -o cpu -O time

- Start top displaying only processes owned by given user:
    top -user user_name

- Get help about interactive commands:
    ?
```

## ssh

```shell
tldr ssh

ssh

Secure Shell is a protocol used to securely log onto remote systems.
It can be used for logging or executing commands on a remote server.
More information: <https://man.openbsd.org/ssh>.

- Connect to a remote server:
    ssh username@remote_host

- Connect to a remote server with a specific identity (private key):
    ssh -i path/to/key_file username@remote_host

- Connect to a remote server using a specific port:
    ssh username@remote_host -p 2222

- Run a command on a remote server with a [t]ty allocation allowing interaction with the remote command:
    ssh username@remote_host -t command command_arguments

- SSH tunneling: Dynamic port forwarding (SOCKS proxy on `localhost:1080`):
    ssh -D 1080 username@remote_host

- SSH tunneling: Forward a specific port (`localhost:9999` to `example.org:80`) along with disabling pseudo-[T]ty allocation and executio[N] of remote commands:
    ssh -L 9999:example.org:80 -N -T username@remote_host

- SSH jumping: Connect through a jumphost to a remote server (Multiple jump hops may be specified separated by comma characters):
    ssh -J username@jump_host username@remote_host

- Agent forwarding: Forward the authentication information to the remote machine (see `man ssh_config` for available options):
    ssh -A username@remote_host
```

## rsync

```shell
tldr rsync

rsync

Transfer files either to or from a remote host (not between two remote hosts).
Can transfer single files, or multiple files matching a pattern.
More information: <https://manned.org/rsync>.

- Transfer file from local to remote host:
    rsync path/to/local_file remote_host:path/to/remote_directory

- Transfer file from remote host to local:
    rsync remote_host:path/to/remote_file path/to/local_directory

- Transfer file in [a]rchive (to preserve attributes) and compressed ([z]ipped) mode with [v]erbose and [h]uman-readable [P]rogress:
    rsync -azvhP path/to/local_file remote_host:path/to/remote_directory

- Transfer a directory and all its children from a remote to local:
    rsync -r remote_host:path/to/remote_directory path/to/local_directory

- Transfer directory contents (but not the directory itself) from a remote to local:
    rsync -r remote_host:path/to/remote_directory/ path/to/local_directory

- Transfer a directory [r]ecursively, in [a]rchive to preserve attributes, resolving contained soft[l]inks , and ignoring already transferred files [u]nless newer:
    rsync -rauL remote_host:path/to/remote_directory path/to/local_directory

- Transfer file over SSH and delete remote files that do not exist locally:
    rsync -e ssh --delete remote_host:path/to/remote_file path/to/local_file

- Transfer file over SSH using a different port than the default and show global progress:
    rsync -e 'ssh -p port' --info=progress2 remote_host:path/to/remote_file path/to/local_file
```

## clear

```shell
tldr clear

clear

Clears the screen of the terminal.
More information: <https://manned.org/clear>.

- Clear the screen (equivalent to pressing Control-L in Bash shell):
    clear

- Clear the screen but keep the terminal's scrollback buffer:
    clear -x

- Indicate the type of terminal to clean (defaults to the value of the environment variable `TERM`):
    clear -T type_of_terminal

- Show the version of `ncurses` used by `clear`:
    clear -V
```

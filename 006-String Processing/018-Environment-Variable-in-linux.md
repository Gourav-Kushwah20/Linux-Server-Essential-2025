# 🐧 Linux Environment Variables  

Environment variables are dynamic values that influence the behavior of processes in a Linux system.  
They are used by the shell and various applications to store configuration settings such as user identity, search paths, and system behavior.  

---

## 🔎 Viewing Environment Variables  

- **Display the current executable search path**  
`Root Login`
```bash
echo $PATH
/root/.local/bin
:/root/bin
:/usr/local/sbin
:/usr/local/bin
:/usr/sbin
:/usr/bin
```
`Normal user Login`
```bash
echo $PATH
/home/sec-learner/.local/bin
:/home/sec-learner/bin
:/usr/local/bin
:/usr/bin
:/usr/local/sbin
:/usr/sbin
```
- **Show the current logged-in user**
```bash    
echo $USER
```

- **Print the current working directory**
```bash
echo $PWD
```
- **Display the user’s home directory**
```bash
echo $HOME
```
## ✍️ Creating and Using Custom Variables

- Assign a value to a shell variable

```bash
var1="My Demo var"
```
- Display the variable’s value

```bash
echo $var1
```

## 🔄 Exporting Variables for Subshells

- Export a variable to make it available to subprocesses

```bash
export target=192.168.1.1
```
- Use the exported variable

```bash
ping -c 4 $target
```
## 📋 Listing All Environment Variables

List all current environment variables

```bash
env
#output:
SHELL=/bin/bash
HISTCONTROL=ignoredups
HISTSIZE=1000
HOSTNAME=CENTOS
PWD=/home/sec-learner
LOGNAME=sec-learner
HOME=/home/sec-learner
LANG=en_IN.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=01;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=01;36:*.au=01;36:*.flac=01;36:*.m4a=01;36:*.mid=01;36:*.midi=01;36:*.mka=01;36:*.mp3=01;36:*.mpc=01;36:*.ogg=01;36:*.ra=01;36:*.wav=01;36:*.oga=01;36:*.opus=01;36:*.spx=01;36:*.xspf=01;36:
TERM=xterm-256color
LESSOPEN=||/usr/bin/lesspipe.sh %s
USER=sec-learner
SHLVL=1
DEBUGINFOD_URLS=https://debuginfod.centos.org/ 
DEBUGINFOD_IMA_CERT_PATH=/etc/keys/ima:
which_declare=declare -f
XDG_DATA_DIRS=/home/sec-learner/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share:/usr/share
PATH=/home/sec-learner/.local/bin:/home/sec-learner/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
target=127.0.0.1
MAIL=/var/spool/mail/sec-learner
BASH_FUNC_which%%=() {  ( alias;
 eval ${which_declare} ) | /usr/bin/which --tty-only --read-alias --read-functions --show-tilde --show-dot $@
}
_=/usr/bin/env
```
## 📑 Common Linux Environment Variables  

| Variable                  | Description |
|----------------------------|-------------|
| `PATH`                    | Search path for commands. |
| `HOME`                    | Home directory of the current user. |
| `USER` / `LOGNAME`        | Current logged-in username. |
| `PWD` / `OLDPWD`          | Present and previous working directory. |
| `LANG`                    | Language and locale setting. |
| `target`                  | Custom exported variable (user-defined). |
| `SSH_CLIENT` / `SSH_TTY`  | SSH session details (IP, port, terminal). |
| `TERM`                    | Terminal type for shell (supports color, layout, etc.). |
| `LS_COLORS`               | Color scheme used by `ls` for different file types. |
| `XDG_*`                   | Desktop session-related variables (used by systemd, etc.). |
| `DEBUGINFOD_URLS`         | Debugging info server URL (used for symbols with debuggers). |
| `BASH_FUNC_*`             | Function definition exported into environment (Bash 4.0+). |

## 🔐 Making Environment Variables Persistent

To make environment variables persist across sessions:

- For the current user, add them to `~/.bashrc` or `~/.bash_profile`:

```bash
export MY_VAR="persistent value"
```
Then reload the file:
```bash
source ~/.bashrc
```
- For system-wide variables, add them to `/etc/environment` (no export keyword needed)
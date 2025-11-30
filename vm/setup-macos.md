## macOS Setup for SimulIDE
### Install [brew](https://brew.sh):
- Make sure you have `admin` right on the macOS account you are using.
- Go to [brew](https://brew.sh) home page and look at the installation commannd. As of this writing, the command is:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
but it may change!
- open terminal
- copy and paste the installation command in terminal.
- input your user password to install, and answer `Yes` to install
- after the installation is finised, brew will show:
```bash
==> Next steps:
- Run these commands in your terminal to add Homebrew to your PATH:
    echo >> /Users/YOURNAME/.zprofile
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/YOURNAME/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
- Run brew help to get started
- Further documentation:
    https://docs.brew.sh
```
- copy the 3 lines (`echo >> `, `echo 'eval ...'`, `eval "$(/opt/...)"'` to your terminal as is.
- then type
```bash
brew help
```
If it prints a lot of text ending by:
```
Further help:
  brew commands
  brew help [COMMAND]
  man brew
  https://docs.brew.sh
```
then your installation is successful.
### Install `sdcc` compiler and `z80dasm` disassembler:
While still at terminal, type:
```
brew install z80dasm
```
and it should return:
```
% brew install z80dasm
✔︎ JSON API formula.jws.json             [Downloaded 31.7MB/ 31.7MB]
✔︎ JSON API cask.jws.json                [Downloaded 15.0MB/ 15.0MB]
==> Fetching downloads for: z80dasm
✔︎ Bottle Manifest z80dasm (1.2.0)       [Downloaded 12.3KB/ 12.3KB]
✔︎ Bottle z80dasm (1.2.0)                [Downloaded 32.9KB/ 32.9KB]
==> Pouring z80dasm--1.2.0.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/z80dasm/1.2.0: 9 files, 110.8KB
==> Running `brew cleanup z80dasm`...
```
Then install `sdcc` in the same way:
```
brew install sdcc
```
This one has a lot more stuff:
```
==> Fetching downloads for: sdcc
✔︎ Bottle Manifest sdcc (4.5.0)                                             [Downloaded   17.9KB/ 17.9KB]
✔︎ Bottle Manifest readline (8.3.1)                                         [Downloaded   12.3KB/ 12.3KB]
✔︎ Bottle Manifest xz (5.8.1)                                               [Downloaded   14.5KB/ 14.5KB]
✔︎ Bottle readline (8.3.1)                                                  [Downloaded  758.2KB/758.2KB]
✔︎ Bottle Manifest gputils (1.5.2)                                          [Downloaded   13.4KB/ 13.4KB]
✔︎ Bottle Manifest lz4 (1.10.0)                                             [Downloaded   13.8KB/ 13.8KB]
✔︎ Bottle Manifest zstd (1.5.7)                                             [Downloaded   16.2KB/ 16.2KB]
✔︎ Bottle zstd (1.5.7)                                                      [Downloaded  788.5KB/788.5KB]
✔︎ Bottle xz (5.8.1)                                                        [Downloaded  748.1KB/748.1KB]
✔︎ Bottle lz4 (1.10.0)                                                      [Downloaded  276.1KB/276.1KB]
✔︎ Bottle sdcc (4.5.0)                                                      [Downloaded   16.9MB/ 16.9MB]
✔︎ Bottle gputils (1.5.2)                                                   [Downloaded   14.7MB/ 14.7MB]
==> Installing dependencies for sdcc: gputils, readline, lz4, xz and zstd
==> Installing sdcc dependency: gputils
==> Pouring gputils--1.5.2.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/gputils/1.5.2: 5,597 files, 144.3MB
==> Installing sdcc dependency: readline
==> Pouring readline--8.3.1.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/readline/8.3.1: 56 files, 2.7MB
==> Installing sdcc dependency: lz4
==> Pouring lz4--1.10.0.arm64_sequoia.bottle.1.tar.gz
🍺  /opt/homebrew/Cellar/lz4/1.10.0: 24 files, 730.7KB
==> Installing sdcc dependency: xz
==> Pouring xz--5.8.1.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/xz/5.8.1: 96 files, 2.6MB
==> Installing sdcc dependency: zstd
==> Pouring zstd--1.5.7.arm64_sequoia.bottle.1.tar.gz
🍺  /opt/homebrew/Cellar/zstd/1.5.7: 32 files, 2.3MB
==> Installing sdcc
==> Pouring sdcc--4.5.0.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/sdcc/4.5.0: 1,942 files, 153.2MB
==> Running `brew cleanup sdcc`...
Disable this behaviour by setting `HOMEBREW_NO_INSTALL_CLEANUP=1`.
Hide these hints with `HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Caveats
Emacs Lisp files have been installed to:
  /opt/homebrew/share/emacs/site-lisp/sdcc
```
### Checking out `sdcc` and `z80dasm`:
In terminal, type:
```
z80dasm -V
```
and you should get:
```
z80dasm, Z80 assembly language generating disassembler 1.2.0
Copyright (C) 1994-2007 Jan Panteltje <panteltje@yahoo.com>
Copyright (C) 2007-2019 Tomaz Solc <tomaz.solc@tablix.org>

This is free software. You may redistribute copies of it under the terms of
the GNU General Public License <http://www.gnu.org/licenses/gpl.html>.
There is NO WARRANTY, to the extent permitted by law.
```
Now, check sdcc:
```
sdcc --version
```
and it should say:
```
% sdcc --version 
SDCC : mcs51/z80/z180/r2k/r2ka/r3ka/sm83/tlcs90/ez80_z80/z80n/r800/ds390/pic16/pic14/TININative/ds400/hc08/s08/stm8/pdk13/pdk14/pdk15/mos6502/mos65c02/f8 TD- 4.5.0 #15242 (Mac OS X ppc)
published under GNU General Public License (GPL)
```
### Test Compilation
Go into `code/` folder of the GitHub repo with terminal and type:
```
make 000-run.bin
```
and you should see:
```
% make 000-run.bin
sdasz80 -l -o default-startup.rel default-startup.s
cp default-startup.rel 000-startup.rel
cp default-startup.lst 000-startup.lst
sdcc -mz80 --std c99 --Werror -c 000-test-mem-nmi.c -o 000-test-mem-nmi.rel
sdcc -mz80 --no-std-crt0 000-startup.rel 000-test-mem-nmi.rel  -Wl-u -Wl-f,000-memmap.ld -o 000-run.ihx

Generating Binary and Disassembly 000-run.bin and 000-run.txs...

sdobjcopy -I ihex -O binary 000-run.ihx 000-run.bin
z80dasm -a -l -t -z -g0 000-run.bin | awk ' BEGIN { count = 0; } /^[ \t]+nop[ \t]+;[0-9a-fA-F]{4}[ \t]+00.*$/ { buffer[count++] = $0; next; } { if (count > 3) { print buffer[0]; print "    ..."; print buffer[count-1]; } else { for (i=0; i<count; i++) print buffer[i]; } count = 0; print $0; } END { if (count > 3) { print buffer[0]; print "    ..."; print buffer[count-1]; } else { for (i=0; i<count; i++) print buffer[i]; } }' > 000-run.txs
Warning: Code might not be 8080 compatible!

Info: 8080 incompatibility warning can be ignored.
```
This means it works! Clean it up by:
```
make clean
```
which deletes all build artifacts:
```
% make clean
rm -f *.asm *.bin *.ihx *.lk *.lst *.map *.noi *.rel *.rst *.sym *.txs
```
### After Installation
`root` and `dev` password is `c` - single character
Become `root` and add `sudo`:
```bash
su -
```
and then
```bash
apt install sudo
usermod -aG sudo dev
```
log out and log in again
### 0. low level setup and sudo without providing password
```bash
sudo visudo
```
scroll below `%sudo   ALL=(ALL:ALL) ALL` and add
```
dev    ALL=(ALL) NOPASSWD: ALL
```
and logout of root shell.

set time zone correctly
```bash
sudo timedatectl set-timezone Asia/Bangkok
```
set locale
```bash
sudo locale-gen en_US.UTF-8
```
```bash
sudo nano /etc/environment
```
and add the following to the top of `/etc/environment` (if it exists)
```bash
LANG="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
LC_CTYPE="en_US.UTF-8"
```
log out and log in, you should see no: `-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory` warnings.
### prettify and add useful bash aliases
> add the following to the tail of `~/.bash_profile`
> ```bash
> if [ -f ~/.bashrc ]; then
>   . ~/.bashrc
> fi
> ```
in `~/.bashrc`, uncomment:
```bash
alias ll='ls -l'
alias la='ls -A'
alias l='ls -CF'
```
> prompt customization to show distro name. add to tail of `.bashrc`
> ```bash
> # --- DISTRO PROMPT CUSTOMIZATION ---
> if [ -f /etc/os-release ]; then
>     . /etc/os-release
>     PS1="(${ID^}) $PS1"
> fi
> ```
### 1. make sure debian is up to date
```bash
sudo apt update
```
```bash
sudo apt upgrade -y
```
### 2. eliminate grub timeout
```bash
sudo nano /etc/default/grub
```
and set `GRUB_TIMEOUT=1`
```bash
sudo update-grub
```
### 3. install utilities besides simulide
```bash
sudo apt install sdcc git z80dasm -y
```
### 4. install simulide to keep its dependency and remove just simulide itself
```bash
sudo apt install simulide
```
then remove it so its dependency still remains:
```bash
sudo apt remove simulide
```
### add shared folder support to UTM
```bash
sudo apt install virtiofsd
```
create share mount point
```bash
sudo mkdir -p /mnt/share
```
Assuming your shared folder tag (UTM “Name”) is `shared`:
```bash
sudo mount -t 9p -o trans=virtio,version=9p2000.L share /mnt/share
```
### 5. add simulide appimage
as a normal user
```bash
mkdir -p ~/.local/share
cd ~/.local/share
```
add appimage simulide downloaded from simulide.com (SimulIDE_1.1.0-SR1_Lin64.tar.gz) to `~/.local/share`
```bash
tar -xvf SimulIDE_1.1.0-SR1_Lin64.tar.gz
```
> #### for Windows VMware
> add `libfuse2t64`
> ```bash
> sudo apt install libfuse2t64
> ```
> #### for UTM
> add `libfuse3`
> ```bash
> sudo apt install fuse3
> ```
#### other stuff that may be necessary
`sudo apt install libgpg-error0`
### 6. add simulide to PATH
do it only once.
```bash
echo 'export PATH="$HOME/.local/share/SimulIDE_1.1.0-SR1_Lin64:$PATH"' >> ~/.bashrc
```
> ### 8. Possible additonal quality of life installations
> `clang cmake`

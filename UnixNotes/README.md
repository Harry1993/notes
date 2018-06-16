# Unix Notes

There should be a content table...

## _XQuery_ command line

```
java -cp saxon9he.jar net.sf.saxon.Query -t -s:samples\data\books.xml -q:samples\query\books-to-html.xq -o:c:\temp.html
```

If you don't want to type `-cp` every time, yo can copy `.jar` file to `/Library/Java/Extensions` on macOS.

## Output redirection

Some of the forms of redirection for the C shell family are:

*	`>`		Redirect standard output
*	`&>`		Redirect standard output and standard error
*	`&>>`		Append standard output and standard error

Examples:

```
who > names
(pwd; ls -l) > out
java DTDGenerator > hw5.dtd
```

## Background jobs

*	Appending an `&` to the command runs the job in the background.
*	Press `CTRL+Z` which will suspend the current foreground job. Execute `bg` to make that command to execute in background.
*	You can list out the background jobs with the command `jobs`.
*	`fg %job-number` can bring a certain job to the foreground.
*	`kill %job-number` kills a certain job

## Compress

### unrar

`% unrar x file.rar` extracts `file.rar` to a new folder named `file`.

### zip & unzip

`zip filename.zip path` zips `path` into `filename.zip`.

`unzip filename.zip` unzips `filename.zip` to current path.

Find and zip: `find ./ -mmin -360 -exec zip filename.zip {} +`

Find and cp: `find ./ -mmin -60 -name '*.eps' -exec cp {} ~/Dropbox/Research/PeB/peb/ \;`

### tar

`tar -jcv -f filename.tar.bz2 path` compresses `path` into `filename.tar.bz2`.

`tar -jxv -f filename.tar.bz2 -C path` decompresses `filename.tar.bz2` to `path`.

`tar -jtv -f filename.tar.bz2` views contents of `filename.tar.bz2`.

`tar -xvz -f file.tar.gz` to decompress a tar.gz file.

### 7z

To extract, run `7za x filename.7z`. Some files have name of `filename.tar.7z`. That means we should first extract with `7za` and then `tar`.

## grep

`grep 'C' reg.log > cc.txt` finds all lines contains 'C'.

`grep -c 'C' reg.log` counts how many lines contains 'C'.

`grep -cP 'E\t' reg.log > ee.txt`. Use `-P` if you want to find `\t`.

`grep 'BQ101\|UR101' reg.log > 101.txt` finds lines contains 'BQ101' OR 'UR101'.

`grep 'R\t' reg.log | grep '0$' > rr.txt` finds lines contains 'R\t' AND _ends with_ 0.

`grep --include=\*.{c,h} --exclude=*.o --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"` searches pattern in all files under given dir.

## Tex

Put `.sty` into `/usr/local/texlive/2010basic/texmf-dist/tex/latex/base/`, then run `texhash`.

## ClipBoard

`pbcopy < file.txt`

`ps aux | pbcopy`

`pbpaste > pastetest.txt`

`pbpaste | grep rcp`

## Regular Expression

### backreference

In vim, if we want to replace, for example,  all `\tar` or `\tau_c` into `$tau$`
or `$tau_c` in current line, we can run

	:$s/\(\\[a-z]|_]*\)/$\1$/gc

where `\1` is called _backreference_. It represents the first pattern that
surround by `()`. `\2` represents the second one.

However, for normal regex, backslash before parenthesis should be omitted.
For more information, refer to [here](http://tex.stackexchange.com/questions/116036/automatically-replace-all-foo-with-absfoo).

## Command sort, grep, awk, sed

### Sort lines where the key is numbers with non-numberic prefix 

Sort lines in a file where the key is numbers with non-numeric prefix, like

	M UR0 abc
	M UR2 cda
	M UR1 ged

just call

	sort -Vnk 2 filename

, which outputs

	M UR0 abc
	M UR1 ged
	M UR2 cda

### Pick lines beginning with certain string, get their certain delimeters, sort them

	grep '^BQ' count8.log | awk '{print $4 "\t" $5}' | sort -nk 2 > count9.log

## bash

*	`[ctrl]+u`	deletes command ahead of cursor
*	`[ctrl]+k`	deletes command behind cursor
*	`df -H`		finds out how much disk space left
*	`du -sh`	finds out how big current dir is.
*	`sudo -i`	enters root mode.
*	`cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'` to ssh
	without password. Or `ssh-copy-id -i ~/.ssh/id_rsa.pub b@B`
*	`ssh-keygen -R <host>` when ssh remote host identification has changed.
*	`async -aP --delete-after ~ b@B` backup directory ~ to server B, refer to this
	[website](https://www.haykranen.nl/2008/05/05/rsync/) and this [shell
script](https://github.com/laurent22/rsync-time-backup) on GitHub.
*	`unset [GLOBAL_VARIABLE_NAME]` to delete global variable.

### If UP arrow for searching command history in zsh enviornment.

According to [this](https://github.com/robbyrussell/oh-my-zsh/issues/1720), we shoud add the following to `.zshrc`:

	if [[ "${terminfo[kcuu1]}" != "" ]]; then
		autoload -U up-line-or-beginning-search
		zle -N up-line-or-beginning-search
		bindkey "${terminfo[kcuu1]}" up-line-or-beginning-search
	fi
	if [[ "${terminfo[kcud1]}" != "" ]]; then
		autoload -U down-line-or-beginning-search
		zle -N down-line-or-beginning-search
		bindkey "${terminfo[kcud1]}" down-line-or-beginning-search
	fi

## VIM

*	`=`		automatically indent.
*	`gq`	automatically cut one line into multiple lines containing at most 80 characters. Usage: enter VISUAL mode, select lines that you want to cut, enter `gq`. Or `gqgq` to cut current line.
*	`:set spell`	spelling check.
*	`%`		jump to the paired parenthese.
*	`;`		jump to the next matched sequence.
*	When replace string including `&`, no `\` ahead for searching. But `\` is needed for replacing. For example, `s/9 & \\infty/\\intfy \& 9/gc`.
*	`^M` issue	Try `:%s/\r//g` 
*	Follow [this](https://github.com/Valloric/YouCompleteMe) to install YouCompleteMe. Then add `set runtimepath+=~/.vim/bundle/YouCompleteMe` in `.vimrc`.
*	To edit in hex mode, use `%!xxd`. Before saving, use `%!xxd -r`. Or see [this](http://vim.wikia.com/wiki/Improved_hex_editing).
*	To preview PDF instantly when editing `.tex`, use [vim-latex-live-preview](https://github.com/xuhdev/vim-latex-live-preview)

### Map hotkeys to do what you want

First, define a function to do something. Take C programming and Latexing for
an example,

```
func C()
	exec "w | !gcc % && ./a.out"
endfunc

func Latex()
	exec "w | !latexmk -pdf % && latexmk -c"
endfunc
```

Now, map the same hotkey to the two functions

```
autocmd FileType c map <C-e> :call C()<CR>
autocmd FileType tex map <C-e> :call Latex()<CR><CR>
```

Here, `autocmd FileType [type]` allows vim to detect the type of the file and
choose which function to call.

## Tmux

### Unable to use `open` command in Tmux

	brew install reattach-to-user-namespace
	echo "set -g default-command \"reattach-to-user-namespace -l ${SHELL}\"" >> ~/.tmux.conf
	tmux kill-server

Explanation can be found [here](http://apple.stackexchange.com/questions/243067/terminal-app-and-tmux-session-cant-use-open-command-without-tmux-it-works?newreg=0952c1c709224fcd992d4ac05e88b9e1).

### `bind-key` issue

The new way should be `bind-key -T copy-mode-vi v send-keys -X begin-selection`. See [this](https://github.com/tmux/tmux/issues/754).

### Sync tmux panes

	:setw synchronize-panes [on/off]

### Split into the current directory

	bind-key c new-window -c "#{pane_current_path}"
	bind-key | split-window -h -c "#{pane_current_path}"
	bind-key - split-window -v -c "#{pane_current_path}"	

### nested tmux

See [this](http://stahlke.org/dan/tmux-nested/).

## Latex

*	To make page size fit to the content, use `\documentclass{standalone}`.
*	Use package `setspace` to alter lines spacing. `\linespread` also works but [note](http://www.tex.ac.uk/FAQ-linespread.html).
*	To compress PDF. Run `gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=small.pdf big.pdf`. See [here](http://tex.stackexchange.com/questions/14429/pdftex-reduce-pdf-size-reduce-image-quality).
*	Use package `etoolbox` to automately add command in any enviornment. See [here](http://mirror.hmc.edu/ctan/macros/latex/contrib/etoolbox/etoolbox.pdf)
*	`vspace{}` sets spacing.
*	[latexmk](http://mg.readthedocs.io/latexmk.html) is nice tool to automake tex files. It also support instant update (Killer Feature).
*	Use `grffile` package to include EPS or PDF as figure. [See](http://www.tex.ac.uk/FAQ-unkgrfextn.html).
*	If we have underscore in .csv, `\usepackage[T1]{fontenc}` and `\csvautobooktabular[respect underscore=true]{data.csv}`. See [this](https://tex.stackexchange.com/questions/204488/csvsimple-handles-underscores-wrong)

## Mutt

IMAP of UNCG address is `y_man@uncg.edu@imap.gmail.com:993`.

SMTP is `smtp://y_man@uncg.edu@smtp.gmail.com:587/`

To save password safely, refer to [this](http://unix.stackexchange.com/questions/20570/mutt-how-to-safely-store-password).

First, Create `~/.mutt/passwords`, where type

	set imap_pass="yourpassword"
	set smtp_pass="yourpassword"

Then encrypte it using GPG. First genereate GPG pub/sec key-pair

	$ gpg --gen-key

Few more steps are needed during this process.

Encrypte the passwords file:

	$ gpg -r your.email@example.com -e ~/.mutt/passwords # or gpg -s --default-key DEADBEE5 input to specify key, before which, run gpg --list-secret-keys
	$ ls ~/.mutt/passwords*
	/home/user/.mutt/passwords   /home/user/.mutt/passwords.gpg
	$ shred ~/.mutt/passwords
	$ rm ~/.mutt/passwords

Add it to your `.muttrc`

	source "gpg -d ~/.mutt/passwords.gpg |"

### "No Authenticators Available" issue on macOS

Add `smtp_authenticators = 'login'` to `muttrc`. `smtp_authenticators = 'gssapi:login'` is slower (may be more secure?).

## macOS

*	To install Zathura, follow [this](https://github.com/zegervdv/homebrew-zathura). Remember to link the plugins into place.

## Ubuntu

### To edit image

use `pinta`.

### To enter Gnome

run `startx`.

### To startup with command line

edit `/etc/default/grub`, change `GRUB_CMDLINE_LINUX_DEFAULT=”quiet splash”`,
to `GRUB_CMDLINE_LINUX_DEFAULT=”text”`. Also, add `GRUB_TERMINAL=console`.
Finally `update-grub` and `systemctl set-default multi-user.target`.

### Gnome xauthority issues

When terminal says `timeout in locking authority file
/home/username/.Xauthority`, try to

	cd /home/username
	mv .Xauthority .Xauthority.old
	touch .Xauthority
	chown username:username .Xauthority 

### Keyboard shortcut changing

Use compizconfig-settings-manager. See [this](http://www.linuxdiyf.com/linux/22726.html).

### Key binding

Use xmodmap. See [this](http://www.cnblogs.com/lzhskywalker/archive/2012/07/20/2600854.html).

### Reverse mouse scrolling direction

Edit `/usr/share/X11/xorg.conf.d/40-libinput.conf`; add `Option "NaturalScrolling" "on"` within the `pointer` Section. I.e., we should have this:

```
Section "InputClass"
        Identifier "libinput pointer catchall"
        MatchIsPointer "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        # Yanmao's setting
        Option "NaturalScrolling" "on"
EndSection
```

### Sound card on Ubuntu

### Printing jobs

Run `lpstat -p` to see the available printers. Run `lpoptions -d [printer]` to set the default printer. Run `lp -d [printer.name] -o sides=two-sided-long-edge [pdf.path]` to print a file. 

#### Reset default sound card

Run `pacmd list-cards` to display the index of your cards. Then `pacmd set-card-profile 2 output:analog-stereo` to set index 2 as the default output. To make it permament, edit `/etc/pulse/default.pa` and add:

```
set-card-profile 2  output:analog-stereo
set-default-sink 2
```

Restart pulseaudio: `pulseaudio -k` and `pulseaudio -D`. See [this](https://askubuntu.com/questions/398030/change-default-sound-device) and [this](https://askubuntu.com/questions/15223/how-can-i-restart-pulseaudio-without-logout).

#### Volume issue

If USB sound card is too loud or mutes at low volume, edit
`/usr/share/pulseaudio/alsa-mixer/paths/analog-output.conf.common`, add line
`volume-limit = 0.005` under `[Element PCM]`. If volume is too low, increase
the number. Run `pulseaudio -k` to make it into effect. See
[this](https://chrisjean.com/fix-for-usb-audio-is-too-loud-and-mutes-at-low-volume-in-ubuntu/).

Or try `alsamixer`. :)

Rathan than `pacmd`, use `pactl`. See [this](https://unix.stackexchange.com/questions/65246/change-pulseaudio-input-output-from-shell) for their differences.

#### For other distributions

If use another sound card, use `aplay -l` to get the device number, then change
`~/.asoundrc` to

	pcm.!default {
        type hw
        card 1
	}

	ctl.!default {
        type hw
        card 1
	}

## MISC

*	Simple calculators `calc` and `bc`.
*	get MD5 `md5 <filename>` or `openssl md5 <filename>`
*	switch Ctrl and Caps Lock: `setxkbmap -layout us -option ctrl:nocaps`
*	to use bash command history, refer to [this](http://www.howtogeek.com/howto/44997/how-to-use-bash-history-to-improve-your-command-line-productivity/).
*	Listen Pandora, use `pianobar`. Refer to [this](https://linuxcritic.wordpress.com/2013/09/08/pianobar-command-line-pandora-client-howto/). Type your password in `~/.config/pianobar/password` and `gpg` it.
*	Before use xfig, run `source /sw/bin/init.sh`.
*	VPN over SSH: `sshuttle -r username@sshserver 0/0`.
*	Use `nload` to monitor network.
*	Burn ISO file to a flash drive or disk, use `dd`. See [this](https://askubuntu.com/questions/372607/how-to-create-a-bootable-ubuntu-usb-flash-drive-from-terminal). Or Etcher on Mac recommanded by Ubuntu.
*	Before `git push`, run `git add .` and then `git commit -m "blahblah"`.
*	Use `sshfs` to mount a sftp drive. E.g., `sshfs -o allow_other -o ro
	username@hostname:/remote/directory/path /local/mount/point`, where `-o
allow_other` option grants other user (e.g., plex) and `-o ro` means read only.
On Mac OS, we need to install `osxfuse` first. See
[this](https://forums.plex.tv/discussion/103322/running-plex-from-vps-accessing-media-from-mounted-sftp-headache)
discussion. To unmount, run `fusermount -uz ./mountpoint`. If forget to unmount before go to sleep, `kill -9 [pid_of_sshfs_process]` first, then `umount -f [mountpoint] `. See [this discussion](https://github.com/osxfuse/osxfuse/issues/45).
*	To delete Plex Server completely, follow [this](https://forums.plex.tv/discussion/31215).
*	To schdule a task regularly, use `cron`. See [this](https://en.wikipedia.org/wiki/Cron).
*	To schdule a one time task, use `at`. See [this](https://tecadmin.net/one-time-task-scheduling-using-at-commad-in-linux/).
*	Google calendar in command line, `gcalcli`.
*	To check a dir is mount point or not, use `mountpoint`. See [this](https://unix.stackexchange.com/questions/38870/how-to-check-if-a-filesystem-is-mounted-with-a-script).
*	To monitor a file being changed, use `watch -n 1 stat -c "%y" thefile`.
*	To create a symbolic link (shortcut), run `ln -s source target`.
*	To mimic pbcopy and pbpaste, add these aliases

```
alias pbcopy='xclip -selection clipboard'
alias pbpaste='xclip -selection clipboard -o'
```

*	To find the PID by process name, run `pgrep -lf <process_name>'. Its co-worker `pkill` can kill the processes given the name.
*	Use `imagemagick` package to take a screenshot. For example, `import ./shot.png` then select an area. The screenshot will be saved as `./shot.png`. Or to shot the full screen, run `imoprt -window root ./fullshot.png`. Or use `scrot`.
*	See [this](https://wiki.ubuntu.com/ScreenCasts/RecordMyDesktop) to learn how to record screen.
*	[Best terminal translator](https://github.com/soimort/translate-shell).
*	To convert EPS to PDF, run `ps2pdf -dEPSCrop input.eps output.pdf`. See [this](http://www.leancrew.com/all-this/2012/03/maintaining-the-boundingbox-in-ps2pdf/).
*	To convert GBK to UTF-8, run `iconv -f GBK -t UTF-8 input.txt > output.txt`.
*	To switch audio device on MacOS, use [switchaudio-osx](https://github.com/deweller/switchaudio-osx).
*	To extract magnet link from a torrent file, use `magnet-link` following [this](https://github.com/ungoldman/magnet-link).
*	To rename multiple files, use `rename`. E.g.,

```
rename -n 's/.*(S01E.*)\ (.*)\ 720p.*/better.call.saul.$1.$2.srt/' ./*
```

to rehearse it. Remove `-n` to actually do it.
*	To replace stupid spaces in filenames, run `find -name "* *" -type f | rename 's/ /_/g'`. See [this](https://stackoverflow.com/questions/2709458/how-to-replace-spaces-in-file-names-using-a-bash-script).
*	To Convert `.dff` to `.flac`, use `dfs2flac`. See [this](https://github.com/hank/dsf2flac). Use bash for loop parallely:

```
for filename in ./*;
	do dsf2flac -i $filename &;
done
```
*	Wake up with password. See [this](https://www.reddit.com/r/i3wm/comments/4uws3f/require_password_on_wake_from_sleep/).

## Useless but cool

*	Use `fbi` to view image in real terminal.
*	Use `fbgs` to view PDF in real terminal.

## Email me when someone ssh in.
Edit `/etc/ssh/sshrc`, add the following:

	ip=`echo $SSH_CONNECTION | cut -d " " -f 1`
	logger -t ssh-wrapper $USER login from $ip
	echo "User $USER just logged in from $ip" | mail -s "ssh alert" yourname@emailserver

## Kick out the ssh-ers.

Run `who -u` to see who there are. And kill their tty's by `kill -9 [pid]`. See [this thread](https://unix.stackexchange.com/questions/615/how-do-you-kick-a-benign-user-off-your-system).

## OpenVPN on Ubuntu

*	Setup an OpenVPN server, use this script `wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`.
If VPN is connected without Internet access, run `sudo ip route add default via xxx.xxx.xxx.xxx`. See [this](https://askubuntu.com/questions/667544/vpn-client-routing-when-use-this-connection-only-for-resources-on-its-network).

*	Connect to an OpenVPN server, check [this](https://www.linux.com/learn/configure-linux-clients-connect-openvpn-server) out.

## MATLAB

`unique` to make values unique.

## SSH to a VM in virtualbox

Accoring to
[this](https://stackoverflow.com/questions/5906441/how-to-ssh-to-a-virtualbox-guest-externally-through-a-host),
Run the following command to add a rule to vboxmanager:

	VBoxManage modifyvm myserver --natpf1 "ssh,tcp,,3022,,22"

where myserver should be the name of the VM. Then you can SSH by

	ssh -p 3022 user@localhost

## Networking

### DNS

Check `/etc/resolv.conf`. They are the DNS servers. It'd better have `8.8.8.8`.
If not, edit `/etc/resolvconf/resolv.conf.d/head`, add `nameserver 8.8.8.8`.
Run `sudo resolvconf -u` to apply the changes. You may wanna reboot.

### nmap

*	`sudo nmap -sS -p 22 192.168.10.0/24` scan the local network for SSH-able hosts.

### Wi-Fi

Run `wpa_passphrase [SSID] [passphrase] > wifi.conf` to save the Wi-Fi config. Run
`wpa_supplicant -B -i interface -c wifi.conf` to connect.

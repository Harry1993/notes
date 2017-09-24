# Unix Notes

This is a document by Yanmao Man, who is learning how to migrate everything
to shell from GUI on his MacBook. Actually he is just being cool.

## _XQuery_ command line

	java -cp saxon9he.jar net.sf.saxon.Query -t -s:samples\data\books.xml -q:samples\query\books-to-html.xq -o:c:\temp.html

If you don't want to type `-cp` every time, yo can copy `.jar` file to `/Library/Java/Extensions` on macOS.

## Output redirection

Some of the forms of redirection for the C shell family are:

*	`\>`		Redirect standard output
*	`\>\&`		Redirect standard output and standard error
*	`\<`		Redirect standard input
*	`\>\!`		Redirect standard output; overwrite file if it exists
*	`\>\&\!`	Redirect standard output and standard error; overwrite file if it exists
*	`\|`		Redirect standard output to another command (pipe)
*	`\>\>`		Append standard output
*	`\>\>\&`	Append standard output and standard error

Examples:

	% who > names
	% (pwd; ls -l) > out
	% java DTDGenerator > hw5.dtd

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

## grep

`grep 'C' reg.log > cc.txt` finds all lines contains 'C'.

`grep -c 'C' reg.log` counts how many lines contains 'C'.

`grep -cP 'E\t' reg.log > ee.txt`. Use `-P` if you want to find `\t`.

`grep 'BQ101\|UR101' reg.log > 101.txt` finds lines contains 'BQ101' OR 'UR101'.

`grep 'R\t' reg.log | grep '0$' > rr.txt` finds lines contains 'R\t' AND _ends with_ 0.

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
*	`cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'`	ssh without password
*	`ssh-keygen -R <host>` when ssh remote host identification has changed.
*	`async -aP --delete-after ~ b@B` backup directory ~ to server B, refer to this
	[website](https://www.haykranen.nl/2008/05/05/rsync/) and this [shell
script](https://github.com/laurent22/rsync-time-backup) on GitHub.

## VIM

*	`=`		automatically indent.
*	`gq`	automatically cut one line into multiple lines containing at most 80 characters. Usage: enter VISUAL mode, select lines that you want to cut, enter `gq`. Or `gqgq` to cut current line.
*	`:set spell`	spelling check.
*	`%`		jump to the paired parenthese.
*	`;`		jump to the next matched sequence.
*	When replace string including `&`, no `\` ahead for searching. But `\` is needed for replacing. For example, `s/9 & \\infty/\\intfy \& 9/gc`.
*	`^M` issue	Try `:%s/\r//g` 
*	Follow [this](https://github.com/Valloric/YouCompleteMe) to install YouCompleteMe. Then add `set runtimepath+=~/.vim/bundle/YouCompleteMe` in `.vimrc`.

## Tmux

### Unable to use `open` command in Tmux

	brew install reattach-to-user-namespace
	echo "set -g default-command \"reattach-to-user-namespace -l ${SHELL}\"" >> ~/.tmux.conf
	tmux kill-server

Explanation can be found [here](http://apple.stackexchange.com/questions/243067/terminal-app-and-tmux-session-cant-use-open-command-without-tmux-it-works?newreg=0952c1c709224fcd992d4ac05e88b9e1).

## Latex

*	To make page size fit to the content, use `\documentclass{standalone}`.
*	Use package `setspace` to alter lines spacing. `\linespread` also works but [note](http://www.tex.ac.uk/FAQ-linespread.html).
*	To compress PDF. Run `gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=small.pdf big.pdf`. See [here](http://tex.stackexchange.com/questions/14429/pdftex-reduce-pdf-size-reduce-image-quality).
*	Use package `etoolbox` to automately add command in any enviornment. See [here](http://mirror.hmc.edu/ctan/macros/latex/contrib/etoolbox/etoolbox.pdf)
*	`vspace{}` sets spacing.

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

	$ gpg -r your.email@example.com -e ~/.mutt/passwords
	$ ls ~/.mutt/passwords*
	/home/user/.mutt/passwords   /home/user/.mutt/passwords.gpg
	$ shred ~/.mutt/passwords
	$ rm ~/.mutt/passwords

Add it to your `.muttrc`

	source "gpg -d ~/.mutt/passwords.gpg |"

## Ubuntu

### To edit image

use `pinta`.

### To enter Gnome

run `startx`.

### To startup with command line

edit `/etc/default/grub`, change `GRUB_CMDLINE_LINUX_DEFAULT=”quiet
splash”`, to `GRUB_CMDLINE_LINUX_DEFAULT=”text”`. Finally `update-grub`.

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

### Sound card on Ubuntu

If USB sound card is too loud or mutes at low volume, edit
`/usr/share/pulseaudio/alsa-mixer/paths/analog-output.conf.common`, add line
`volume-limit = 0.005` under `[Element PCM]`. If volume is too low, increase
the number. Run `pulseaudio -k` to make it into effect. See
[this](https://chrisjean.com/fix-for-usb-audio-is-too-loud-and-mutes-at-low-volume-in-ubuntu/).

Or try `alsamixer`. :)


## MISC

*	Simple calculators `calc` and `bc`.
*	get MD5 `md5 <filename>` or `openssl md5 <filename>`
*	switch Ctrl and Caps Lock: `setxkbmap -layout us -option ctrl:nocaps`
*	to use bash command history, refer to [this](http://www.howtogeek.com/howto/44997/how-to-use-bash-history-to-improve-your-command-line-productivity/).
*	Listen Pandora, use `pianobar`. Refer to [this](https://linuxcritic.wordpress.com/2013/09/08/pianobar-command-line-pandora-client-howto/). Type your password in `~/.config/pianobar/password` and `gpg` it.
*	Before use xfig, run `source /sw/bin/init.sh`.
*	VPN over SSH: `sshuttle -r username@sshserver 0/0`.
*	Use `nload` to monitor network.
*	Burn ISO file to a flash drive or disk, use `dd`. See [this](http://osxdaily.com/2012/03/13/burn-an-iso-image-from-the-command-line/).
*	Before `git push`, run `git add .` and then `git commit -m "blabla"`.

## OpenVPN on Ubuntu

*	Setup an OpenVPN server, use this script `wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`.
If VPN is connected without Internet access, run `sudo ip route add default via xxx.xxx.xxx.xxx`. See [this](https://askubuntu.com/questions/667544/vpn-client-routing-when-use-this-connection-only-for-resources-on-its-network).

*	Connect to an OpenVPN server, check [this](https://www.linux.com/learn/configure-linux-clients-connect-openvpn-server) out.

## MATLAB

`unique` to make values unique.

# Unix Notes

This is a document by Yanmao Man, who is learning how to migrate everything
to shell from GUI on his Macbook. Actually he is just being cool.

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
*	`df -H`		finds out hwo much disk space left
*	`sudo -i`	enters root mode.
*	`cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'`	ssh without password
*	`async -aP ~ b@B` backup directory ~ to server B, refer to this
	[website](https://www.haykranen.nl/2008/05/05/rsync/) and this [shell
script](https://github.com/laurent22/rsync-time-backup) on GitHub.

## VIM

*	`=`		automatically indent.
*	`gq`	automatically cut one line into multiple lines containing at most 80 characters. Usage: enter VISUAL mode, select lines that you want to cut, enter `gq`. Or `gqgq` to cut current line.
*	`:set spell`	spelling check.
*	`%`		jump to the paired parenthese.
*	`;`		jump to the next matched sequence.

## Latex

*	To make page size fit to the content, use `\documentclass{standalone}`.

## MISC

*	Simple calculators `calc` and `bc`.
*	get MD5 `md5 <filename>` or `openssl md5 <filename>`

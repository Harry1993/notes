# Unix Notes

This is a document by Yanmao Man, who is learning how to migrate everything
to shell from GUI on his Macbook. Actually he is just being cool.

## _XQuery_ command line

	java -cp saxon9he.jar net.sf.saxon.Query -t -s:samples\data\books.xml -q:samples\query\books-to-html.xq -o:c:\temp.html

If you don't want to type `-cp` every time, yo can copy `.jar` file to `/Library/Java/Extensions` on OS X.

## Output redirection

Some of the forms of redirection for the C shell family are:

*	\>		Redirect standard output
*	\>\&	Redirect standard output and standard error
*	\<		Redirect standard input
*	\>\!	Redirect standard output; overwrite file if it exists
*	\>\&\!	Redirect standard output and standard error; overwrite file if it exists
*	\|		Redirect standard output to another command (pipe)
*	\>\>	Append standard output
*	\>\>\&	Append standard output and standard error

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

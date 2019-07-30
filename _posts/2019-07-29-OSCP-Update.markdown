---
layout: splash
title: "OSCP Advice"
classes:
  - landing
  - dark-theme
  - wide
---

# 7-29-2019 9:48 PM

My exam is fewer than three weeks from today. It is so scary because I don't feel prepared, but hopefully, that will change soon. To give me some hope and some relaxing time, I'm writing about I've learned from the labs:

- OneNote is a great reporting template.
- Just because a process is running on port 80 doesn't mean it's a website (hello, `nc -nlvp 80 -e evil.sh`). Be skeptical of nmap results: they may not capture everything. If that's so, WireShark/file enumeration are your friends.
- If you are sure all vulnerabilities discovered don't lead anywhere, you're in a rabbit hole, you missed combining vulnerabilities, you're cracking a machine with a dependency, or you missed some vulnerabilities.
- Modifying exploits is a pain and a good majority always break/were patched, so don't be mad if an exploit that *should work* doesn't.
- The powerful shell enablers are RFI/LFI/RCE/correct credentials. If you can't find any, there may be a vulnerability which will allow you to enumerate more information which will lead to them. Also I always ignore Denial of Service exploits.
- Some must-know PWK topics are knowing how to file upload in web/shell environments, searching for exploits in Google, GitHub, and searchsploit, privilege escalation techniques, and fixing/understanding exploit compilation errors.
- Gather all possible exploits and rank their probability of success before trying to execute them.

May everyone trying to get OSCP certified find success. :)
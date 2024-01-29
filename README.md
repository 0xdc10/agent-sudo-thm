# AGENT-SUDO

### IP: 10.10.209.235

### PORTS
- 21
- 22
- 80

## 21
- anon not allowed

## 80
```
Dear agents,

Use your own codename as user-agent to access the site.

From,
Agent R 
```

- from the message above, we got `Agent R`
- used `R` as user-agent via firefox, got this:
```
What are you doing! Are you one of the 25 employees? If not, I going to report this incident

Dear agents,

Use your own codename as user-agent to access the site.

From,
Agent R 
```

- from the above message, I can guess that there are 26 agents.
- 25 employees + Agent R
- so trying A-Z
- A and B : nothing
- C:
```
Attention chris,

Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!

From,
Agent R 
```
- so we also get `Agent J`, but not getting any response on webpage.
- same for other letters

### At this Stage
- Agent Chris
- his password is weak
- Agent J and Agent R have a deal

### Bruteforce ftp/chris
- tried ncrack and medusa : faced isssues/couldn't start while tee-ing
- used hydra : output in hydra-chris-ftp file
- password for ftp/chris: crystal

### FTP-chris
```
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png

```
- in the note for J, we know that the password for J is in these images : means `steg`
- steghide needs password
- online decoder gives gibberish

But, On THM, the question ask about the password for zip file , but we don't have that. so trying to `binwalk` on the files downloaded.

### Binwalk
- cute-alien
```

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01


```
- cutie
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive, footer length: 22


```

so we have a zip archive, 

### ZIP
- encrypted zip
- trying JOHN
- password: alien
- contents:
```
Agent C,

We need to send the picture to 'QXJlYTUx' as soon as possible!

By,
Agent R

```

- decoding QXJlYTUx
- base64: Area51

### Other image
on the THM portal, following the questions, this must be password for other file. 
- trying steghide again
- messsage:
```
Hi james,

Glad you find this message. Your login password is hackerrules!

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris

```

### Agent J
- name: James
- password: hackerrules
- creds type: ssh (from previous messages)

### SSH James
- user-flag: b03d975e8c92a7c04146cfa7a5a313c7
- Alien_autospy.jpg
- `sudo -l`
```
james@agent-sudo:~$ sudo -l
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash

```

- exploitDB
- CVE-2019-14287
- scp the file to james@IP
- got root access
- /root/root.txt
```
To Mr.hacker,

Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine. 

Your flag is 
b53a02f55b57d4439e3341834d70c062

By,
DesKel a.k.a Agent R

```

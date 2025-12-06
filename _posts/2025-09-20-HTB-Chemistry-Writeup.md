---
title: "Writeup: HTB Chemistry"
date: 2025-09-20
categories: Software
tags: [ctf, linux, htb]
---
 
Chemistry is an easy linux box that begins with us finding a website allowing us to upload `.cif` files. We gain RCE through file upload, giving us low privilege access as the `app` user. From there, we find a database with users and hashes. We find that one of the users who has a hash also owns a user on the machine. We SSH into the user, port forward an internal webpage that is susceptible to file inclusion. We get an SSH key for root and the box is compromised! 

## Enumeration
### `nmap` scan
We begin with an initial port scan to find ports 22 and 5000 open:
```bash
sudo nmap -p- 10.10.10.10
```
![](/assets/images/htb-chemistry/initial.png)

Full scan:
```bash
sudo nmap -sCV -Pn -T4 -p22,5000 -oN nmap-initial.sh 10.10.10.10
```
_(I save it as an `.sh` for syntax highlighting)_
![](/assets/images/htb-chemistry/fullscan.png)

There seems to be a webpage at port 5000. Let's check it out by navigating to `http://TARGET:5000/`:
![](/assets/images/htb-chemistry/index.png)

And we are greeted with some sort of file upload.

After registering an account, we are allowed to upload files.
![](/assets/images/htb-chemistry/register.png)

**What is a `.cif` file?**
> A .cif file typically refers to ==a Crystallographic Information File, a standardized, human- and machine-readable format for storing and exchanging crystallographic data, including single-crystal and powder structures==. Sponsored by the International Union of Crystallography (IUCr), **this format is essential for reporting crystal structure determinations to scientific journals and databases.** The file contains experimental and structural data such as atomic coordinates, bond lengths, and other crystallographic parameters.

A `.cif` file seems to be some sort of chemistry-related crystal structure documentation (hence the box name).

### Finding a vulnerability in the `.cif` files able to upload

Before testing a conventional reverse shell, I searched for `.cif` file CVEs/Vulns.
Upon further enumeration, we find a CVE ([CVE-2023-48031](https://nvd.nist.gov/vuln/detail/CVE-2023-48031)):
![](/assets/images/htb-chemistry/cve.png)

We can then find a PoC script that will get us a shell:
https://github.com/ex-cal1bur/CIF_Reverse_shell/blob/main/CIF_example.cif

After updating the file with our IP and Port, we can
![](/assets/images/htb-chemistry/revshell.png)

Start a `nc` listener:
```bash
nc -lvnp 7777
```

NB: Press the `VIEW` button on the webpage to trigger your shell (by requesting it)
![](/assets/images/htb-chemistry/triggershell.png)

And, we get a shell 🎉

![](/assets/images/htb-chemistry/shell.png)

## Initial Access as `app@chemistry`

Let's upgrade it:
```bash
python3 -c "import pty;pty.spawn('/bin/bash')"
CTRL Z
stty -raw echo;fg
ENTER x2
export TERM=xterm
```

resulting in a much nicer shell.
![](/assets/images/htb-chemistry/upgraded-shell.png)

### Finding a database
After navigating to `/home`, I discovered another user called `rosa`, whose home directory we are unable to access. So, I decided to continue enumerating. I went back to webroot, and found that there was a database with usernames and hashes (`/instance/database.db`).

![](/assets/images/htb-chemistry/database.png)

Most users don't stand out to us, apart from the same `rosa` user that exists on this box.

```sql
INSERT INTO user VALUES(3,'rosa','63ed86ee9f624c7b14f1d4f43dc251a5');
```

We can decrypt the hash with [CrackStation](https://www.crackstation.net/)

![](/assets/images/htb-chemistry/crackstation.png)

And we get the user password 🎉 

In our `nmap` scan, we saw port 22 open for SSH. Let's attempt to establish connection with `rosa@chemistry`:

![](/assets/images/htb-chemistry/rosassh.png)

We can now get the user flag:
```bash
cat user.txt
```

## Privilege Escalation

After trying:
- `sudo` permissions
- Searching for SUID binaries
- Looking for `cronjobs`
- `linpeas.sh`

### Finding an internally ran webpage
We find that there is a locally run webpage on port 8080, which was found with the command:
```bash
ss -lnt
```

We can now port-forward this website to our machine:
```bash
ssh -L 9000:localhost:8080 rosa@10.10.11.38
```

Now, navigating to `http://localhost:9000` gives us some form of panel with business graphs, and options to Start, Stop, check attacks and more.

However, the interesting sections seem to be blocked:
![](/assets/images/htb-chemistry/arc.png)

Opening burp, we can see that the popup is javascript as we don't get a response for it.

However, we do get a request for the "list services" page, with a HTTP header of

![](/assets/images/htb-chemistry/burprequest.png)

One of the headers is interesting - 
```http
Server: Python/3.9 aiohttp/3.9.1
```


Let's research and look for vulnerabilities now.

### Path Traversal CVE

We find that there are a lot of CVEs associated with this version, however path traversal sticks out like a sore thumb:
![](/assets/images/htb-chemistry/fileinclusion.png)

If we go to the `target` section on Burp, we can see many requests to `/assets`, so let's try file inclusion from there.
(We could have dirbusted/fuzzed for it otherwise)

![](/assets/images/htb-chemistry/burp.png)

After amending and adjusting the `../` characters for file inclusion, we get file inclusion:
![](/assets/images/htb-chemistry/works.png)
```http
GET /assets/../../../etc/passwd HTTP/1.1
```

### SSH Key for `root` user

Let's try to get an `ssh` key now for the `root` user:
```http
GET /assets/../../../root/.ssh/id_rsa HTTP/1.1
```
![](/assets/images/htb-chemistry/ssh.png)

We end up with the `root` key.

## Access as root

We can now login to root through `ssh`:
```bash
chmod 400 id_rsa
ssh -i id_rsa root@MACHINE
cat root.txt
```

And our box is compromised.
![](/assets/images/htb-chemistry/rootaccess.png)

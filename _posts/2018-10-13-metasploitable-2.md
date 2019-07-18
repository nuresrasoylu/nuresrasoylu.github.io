---
layout: post
title:  "Metasploitable 2"
categories: "metasploitable"
description: "Metasploitable 2"
---

## Metasploitable 2

Victim Machine’s IP Address: 192.168.207.133
Attacker Machine’s IP Address: 192.168.207.162


![Metasploitable 2](../assets/images/2018-10-13/01.png)

We do a classic nmap scan. You have seen a simple scan result without using any parameters. So now let’s do a detailed port scan !

![Metasploitable 2](../assets/images/2018-10-13/02.png)

As we have seen a much more detailed result. So we need to use the parameter.

Nmap Parameters:

**-sS:** This scaning is often used in penetration tests. There are two reasons for this. Firstly, only one packet is sent in the SYN scan, so the scan takes place quickly and the second is not blocked by the structures such as firewall / IPS because the 3-hands handshake is not complete.

**-sV:** Learns machine’s version information. The most important point I have to mention here is to complete 3 hands handshaking while performing a nmap version scan. That’s why we are more likely to be blocked by firewalls and logos are seen as we connect.

**-O:** Makes the operating system discovery.

### FTP

![Metasploitable 2](../assets/images/2018-10-13/ftp-03.png)

I’m connecting to the IP address using the Ftp client. If the ftp servers are not disabled, everyone has anonymous: anonymous users will do the installation.

![Metasploitable 2](../assets/images/2018-10-13/ftp-04.png)

I’ll try to get the link from the computer to the directory by installing the shell.php. I’m trying to upload shell.php in my local directory with the put command immediately, but I get 553 error that means that the configuration settings of the ftp server to close to the home directory.

![Metasploitable 2](../assets/images/2018-10-13/ftp-05.png)

I’m trying to change the directory with cd command, but still not, I can not leave the home directory, bye command to end the session.

![Metasploitable 2](../assets/images/2018-10-13/ftp-06.png)

I’m trying to exploit the vulnerabilities in the service and exploit the msfconsole.

![Metasploitable 2](../assets/images/2018-10-13/ftp-07.png)

And I’m ROOT! It’s OK 🙂

### SSH

What is SSH? SSH, or Secure Shell is a remote management protocol that allows users to control and edit their servers over the Internet.
I’m going to first attack the SSH service.
I’m searching to use the ssh_login module from msfconsol. I came up with 2 modules, we need the user: pass list for the first module, and the other we need rsa key. Since I did not have the rsa key so I continue with the first module.

![Metasploitable 2](../assets/images/2018-10-13/ssh-08.png)

![Metasploitable 2](../assets/images/2018-10-13/ssh-09.png)

After a series of attempts, there are 2 users, user: user and msfadmin:msfadmin, no low user root rights, after entering the machine, we will privilege escalation.

![Metasploitable 2](../assets/images/2018-10-13/ssh-10.png)

I enter the system with the user ssh client. I see that I am the standard user who is looking at the group rights again with the id command. I’m trying to be root with Sudo water but I get the error because the user configuration file (/etc/sudoers) is not included.

![Metasploitable 2](../assets/images/2018-10-13/ssh-11.png)

We entered the machine. Now if you want to privilege escalation you can do that steps:

![Metasploitable 2](../assets/images/2018-10-13/ssh-12.png)

Firstly, we learned System Information.

![Metasploitable 2](../assets/images/2018-10-13/ssh-13.png)

Second, we investigated the vulnerabilities in the system.

![Metasploitable 2](../assets/images/2018-10-13/ssh-14.png)

![Metasploitable 2](../assets/images/2018-10-13/ssh-15.png)

the exploit we found, we have downloaded our own machines as server.

![Metasploitable 2](../assets/images/2018-10-13/ssh-16.png)

![Metasploitable 2](../assets/images/2018-10-13/ssh-16_2.png)

![Metasploitable 2](../assets/images/2018-10-13/ssh-17.png)

Than, I try to access /etc/sudoers file. I can give root privileges to user “user”, I can increase the privilege. But It’s not work.

### TELNET

Telnet service is running on port 23. Telnet briefly; It is as TCP / IP type protocol. Developed to connect to a machine with an Internet network from another machine. Since Nmap cannot find any service version number in the service scan, I will try to determine the version using the auxiliary modules in metasploit.

![Metasploitable 2](../assets/images/2018-10-13/ssh-18.png)

When I review the incoming output, it goes through the text in “login with as msfadmin / msfadmin ”.

![Metasploitable 2](../assets/images/2018-10-13/ssh-19.png)

Yesss, Login is successful!
Now I need to know what authority we have in the system. 

![Metasploitable 2](../assets/images/2018-10-13/ssh-20.png)

Okey.. Then I need to privilege escalation. It says the operating system can cover versions from 2.6.9 to 2.6.33(from nmap scan). When I search from Exploitdb, I see that there are exploit written in c, covering version 2.6.x.

![Metasploitable 2](../assets/images/2018-10-13/ssh-21.png)

I’m recording the exploit with the wget command. I’m doing the file name test.c. Then I edit the exploit with the help of the leafpad, normally I need to create a file named run / tmp into the file, and write the bash script into it, but I’m writing the payload myself, thinking that the user doesn’t have netcat installed.

![Metasploitable 2](../assets/images/2018-10-13/ssh-22.png)

Then in my current directory I’m standing up with my http service on my IP address with python.

![Metasploitable 2](../assets/images/2018-10-13/ssh-23.png)

![Metasploitable 2](../assets/images/2018-10-13/ssh-24.png)

I compile my c file with gcc, then I find the operation number of the linux task manager and the udev service (2718), then carry the payload with the mv command to the tmp directory.

### TOMCAT

I see that the apache server is installed in the system. I need to gather information on the system.

![Metasploitable 2](../assets/images/2018-10-13/tomcat-25.png)

We need to use exploits that give us the best results. (“excellent”).

![Metasploitable 2](../assets/images/2018-10-13/tomcat-26.png)

I use the auxiliary module first because I need to collect information about the system to use the exploit.

![Metasploitable 2](../assets/images/2018-10-13/tomcat-27.png)

After entering the necessary information I’m running the exploit.

![Metasploitable 2](../assets/images/2018-10-13/tomcat-28.png)

Now I’m on the shell screen of the machine. But I’m not root. So I want to be root.

![Metasploitable 2](../assets/images/2018-10-13/tomcat-29.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-30.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-31.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-32.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-33.png)

I’m processing it after finding the appropriate exploit to privilege escalation.

![Metasploitable 2](../assets/images/2018-10-13/tomcat-34.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-35.png)

![Metasploitable 2](../assets/images/2018-10-13/tomcat-36.png)

Yesss, And we are ROOT !
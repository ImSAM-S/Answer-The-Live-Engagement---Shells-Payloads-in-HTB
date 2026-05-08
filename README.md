# Answer-The-Live-Engagement---Shells-Payloads-in-HTB
## Introduction
So! Today i wanna show you guy: "how can i exploit all of this stuff". It took me like 1 day to solve all the question. So let's begin!
## Quick throught
If you guy just want to know answers so look this img

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/14b7a0443916b66c43dc34e2e49f4df894a006a8/pic0_quess_and_answer.png)

## Scenario
CAT5's team has secured a foothold into Inlanefrieght's network for us so our responsibility is to find the results from the recon that was done, validate any info that we seem necessary, research what can be seen, and choose which exploit, payloads, and shells will be used to control the targets

## Objectives
. Demonstrate your knowledge of exploiting and receiving an interactive shell from a Windows host or server.
. Demonstrate your knowledge of exploiting and receiving an interactive shell from a Linux host or server.
. Demonstrate your knowledge of exploiting and receiving an interactive shell from a Web application.
. Demonstrate your ability to identify the shell environment you have access to as a user on the victim host.
## Credentials and Other Needed Info
Foothold:
. IP: (maked by yoursefl)
. Credentials: htb-student / HTB_@cademy_stdnt! Can be used by RDP.
## Target Hosts

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic1_target_hosts.png)

Host-01: 172.16.1.11 

Host-02: blog.inlanefreight.local 

Host-03: 172.16.1.13
## Hints
Attempt to complete the challenges on your own. If you get stuck then view the helpful hints below and next to each challenge question:

Host-1 hint:
This host has two upload vulnerabilities. If you look at status.inlanefreight.local or browse to the IP on port 8080, you will see the vector. When messing with one of them, the creds " tomcat | Tomcatadm " may come in handy.

Host-2 hint:
Have you taken the time to validate the scan results? Did you browse to the webpage being hosted? blog.inlanefreight.local looks like a nice space for team members to chat. If you need the credentials for the blog, " admin:admin123!@# " have been given out to all members to edit their posts. At least, that's what our recon showed.

Host-3 hint:
This host is vulnerable to a very common exploit released in 2017. It has been known to make many a sysadmin feel Blue.

## Exploit
So first of all, we need to connect the Foothold by using xfreerdp

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic2_connect.png)

After establishing initial access xfreerdp, i used nmap to conduct port scan on Host-01 (**172.16.1.11**), revealing several open ports.

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic3_scan_port.png)

Setence 1: What is the hostname of Host-1? look at the picture that we can see it is | **shells-winsvr**

Searched: "status.inlanefreight.local" like what we scaned. Bonus, how to open firefox too.

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic4_open_firefox.png)

Like what we was scaning, i admin that system is Windows Server 2019. After that, i used whatweb to determine the specific server software in order to generate a payload using msfvenom


![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic5_who_write_payload.png)

Based on findings, seem that it used ASP.NET so now we can generate a payload to attack.

Let's establish a shell. I used the Laudanum webshell available at the following link:
[Github](https://github.com/jbarcia/Web-Shells/tree/master/laudanum)

After you dowloaded (asp file), you need to push your own **ip** in your computer to the asp file. Look image to see how to find it! It is ens224:

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic6_more_ip.png)

So when you done with asp file. Now you go back to "status.inlanefreight.local" to upload that file. And if you done of upload, so now you can search: "status.inlanefreight.local/files/shell.aspx" to connect webshell - shell.aspx is your file's name uploaded.

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic7_answer_N1.png)

Now you have successfully obtained a shell and can now access the folder located at C:\Shares. 'Exploit the target and gain a shell session. Submit the name of the folder located in C:\Shares\ (Format: all lower case)' | **dev-share**

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic8_answer_N2.png)

Now next is host-2: **blog.inlanefreight.local**
Check what is it ip:

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic9_find_ip2.png)

Next is scanning:

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic10_scan_ip2.png)

'What distribution of Linux is running on Host-2? (Format: distro name, all lower case)' | **Ubuntu**
Two ports are open, 22 and 80. But this attack requires authentication, so i conducted directory enumeration to gather additional information.

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic11_enum.png)

At the line 1 of results, we saw a folder is data. Upon accessing it, I enterned a file named 'config.ini' and saw this information - a username: **admin** and it password: **admin123!@#**. You can scroll down to see it.

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/main/pic12_username.png)

So, the hardest i have faces in here you dont know what payloads i can use now! I look at the stences4 and realized the '50064' vulnerabbility and upon accessing port 80, a post directing to an exploit named 50064.
Now i will find '50064' at Foothold and hope it has, i didnt want to dowload it files. And lucky me it had

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic13_50064rb.png)

Answer the question:'What language is the shell written in that gets uploaded when using the 50064.rb exploit?' It **php**
So next i opend metasploit and searched it and used it:

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic14_msf.png)

Find and read flag.txt:'Exploit the blog site and establish a shell session with the target OS. Submit the contents of /customscripts/flag.txt' **B1nD_Shells_r_cool**

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/30eefafd7d4ad001f879cda54dcdb17ba5a71a3e/pic15_read_flag.png)

Now, the last challenge, let's exploit third target: **172.16.1.13**
Scan for sure

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/0b200356e15ac18a861d7f0cad190129bfa01979/pic16_scan_ip3.png)

I discovered that the server operates on Windows Server 2016 and the SMB protocol (445/tcp) is open.Now i tried to figure it out what vulnerability can uses to exploit it. So look at the hint: 'This host is vulnerable to a very common exploit released in 2017. It has been known to make many a sysadmin feel Blue.'

It seem like Eternalblu (ms17_010) so i choose metasploit to check it

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/0b200356e15ac18a861d7f0cad190129bfa01979/pic17_check_ip3_host.png)

So yea! It like what i thought, it is MS17-010!
Now let's exploit it!

![image alt](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/0b200356e15ac18a861d7f0cad190129bfa01979/pic18_exploit_WannaCry.png)

Finally! We can access to the flag and answer the question

![image](https://github.com/ImSAM-S/Answer-The-Live-Engagement---Shells-Payloads-in-HTB/blob/0b200356e15ac18a861d7f0cad190129bfa01979/pic19_finally.png)

What is the hostname of Host-3? | **SHELLS-WINBLUE**
Exploit and gain a shell session with Host-3. Then submit the contents of C:\Users\Administrator\Desktop\Skills-flag.txt | **One-H0st-Down!**

Thanks for taking the time to real all of this writeup. Have you a good day

## Project Files

```tree
Answer-The-Live-Engagement---Shells-Payloads-in-HTB
├── pic0_quess_and_answer.png
├── pic1_target_hosts.png
├── pic2_connect.png
├── pic3_scan_port.png
├── pic4_open_firefox.png
├── pic5_who_write_payload.png
├── pic6_more_ip.png
├── pic7_answer_N1.png
├── pic8_answer_N2.png
├── pic9_find_ip2.png
├── pic10_scan_ip2.png
├── pic11_enum.png
├── pic12_username.png
├── pic13_50064rb.png
├── pic14_msf.png
├── pic15_read_flag.png
├── pic16_scan_ip3.png
├── pic17_check_ip3_host.png
├── pic18_exploit_WannaCry.png
├── pic19_finally.png
└──README.md
```
## Author
ImSAM-S

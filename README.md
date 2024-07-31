# Pickle-Rick-CTF

## Background
This <a href="https://tryhackme.com/r/room/picklerick" target="_blank">Pickle Rick CTF</a> is part of the Web Hacking Fundementals section in the Complete Beginner Learning Path from TryHackMe

The challenge is designed as a culmination of all the skills learned so far in the learning path, including; researching skills, linux, networking, understanding the HTTP protocol, and knowlage of the OWASP Top 10.

## The Challenge 
![Screenshot 2024-07-30 204904](https://github.com/user-attachments/assets/04e97124-acda-4b3f-b2c7-e65b63e3afbb)

The instructions let us know that there are three flags we should be looking for to complete this challenge

## Enumeration
I started this challeneg by taking a look at the general website.

![Screenshot 2024-07-30 210958](https://github.com/user-attachments/assets/01bb4acd-9982-4920-b8ac-c7f92c347fe7)

The home page reveals that there is a password that we need to find somewhere on the site. 

After making note of this, I ran an nmap scan to try to discover any open ports running on the server. The command executed to run the scan was `nmap -sS -v 10.10.240.224`.

```
-sS: TCP SYN (Stealth Scan) 
-v: Verbosity Level 1 
10.10.240.224: Target IP
```

![Screenshot 2024-07-30 210836](https://github.com/user-attachments/assets/d8424b6a-d8bf-4ed9-a9c9-14085d99fc29)

This scan revealed two open ports, `port 22` which runs an ssh service and `port 80` which runs an http service

I then subsequently ran a gobuster directory by running the following command in the terminal `gobuster dir -u http://10.10.240.224/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,pdf,php` 

```
dir: Directory scan mode
-u http://10.10.240.224/: Target URL
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt: Path to the wordlist
-x txt,pdf,php: File extensions to search for
```

![Screenshot 2024-07-30 214202](https://github.com/user-attachments/assets/7e158f02-a5dc-4c46-80fc-0acd1e60aa38)

This revealed seven directiories. The `login.php` directory stood out to me so I visited it by adding the extension to the url

![Screenshot 2024-07-30 214430](https://github.com/user-attachments/assets/f279a614-ee80-4943-b629-3e10473b971b)

Perfect! Except I dident have any credentials to input so I tried something else. I went back to the home page and looked at the source code for the main page to see if there was anything hidden in the HTML. Luckily there was and this is what I found.  

![Screenshot 2024-07-30 212048](https://github.com/user-attachments/assets/4d3c01e4-c314-426a-8305-3800dd0c31ec)

`Username: R1ckRul3s`

I then went back to the login page to try the username and see if I could get some type of error that might provide some more infromation

![Screenshot 2024-07-30 214709](https://github.com/user-attachments/assets/90c8c09b-65f8-4954-b7f4-c72ae47415ef)

Although this dident work. I then went back to look through the other directories. `robot.txt` was accessible and had only one word, `Wubbalubbadubdub`. 

![Screenshot 2024-07-30 214822](https://github.com/user-attachments/assets/842dbe86-1670-4d48-aff4-56a33bd502f6)

I tried to input this into the login page as the password and it worked. 

![Screenshot 2024-07-30 215116](https://github.com/user-attachments/assets/fdf4313d-9344-48f8-b4ec-73889ad1c7d4)

I now had Rick's credentials 
```
Username: R1ckRul3s
Password: Wubbalubbadubdub
```

The home page of the 

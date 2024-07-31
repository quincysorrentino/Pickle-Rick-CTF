# Pickle-Rick-CTF

## Background
This [Pickle Rick CTF](https://tryhackme.com/r/room/picklerick) is part of the Web Hacking Fundamentals section in the Complete Beginner Learning Path from TryHackMe.

The challenge is designed as a culmination of all the skills learned so far in the learning path, including researching skills, Linux, networking, understanding the HTTP protocol, and knowledge of the OWASP Top 10.

## The Challenge 
![Screenshot 2024-07-30 204904](https://github.com/user-attachments/assets/04e97124-acda-4b3f-b2c7-e65b63e3afbb)

The instructions let us know that there are three flags we should be looking for to complete this challenge.

## Enumeration
I started this challenge by taking a look at the general website.

![Screenshot 2024-07-30 210958](https://github.com/user-attachments/assets/01bb4acd-9982-4920-b8ac-c7f92c347fe7)

The home page reveals that there is a password that we need to find somewhere on the site. 

After making note of this, I ran an nmap scan to try to discover any open ports running on the server. The command I executed to run the scan was `nmap -sS -v 10.10.240.224`.

```
-sS: TCP SYN (Stealth Scan) 
-v: Verbosity Level 1 
10.10.240.224: Target IP
```

![Screenshot 2024-07-30 210836](https://github.com/user-attachments/assets/d8424b6a-d8bf-4ed9-a9c9-14085d99fc29)

This scan revealed two open ports: `port 22`, which runs an SSH service, and `port 80`, which runs an HTTP service.

I then subsequently ran a gobuster directory scan by running the following command in the terminal: `gobuster dir -u http://10.10.240.224/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,pdf,php`.

```
dir: Directory scan mode
-u http://10.10.240.224/: Target URL
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt: Path to the wordlist
-x txt,pdf,php: File extensions to search for
```


![Screenshot 2024-07-30 214202](https://github.com/user-attachments/assets/7e158f02-a5dc-4c46-80fc-0acd1e60aa38)

This revealed seven directories. The `login.php` directory stood out to me, so I visited it by adding the extension to the URL.

![Screenshot 2024-07-30 214430](https://github.com/user-attachments/assets/f279a614-ee80-4943-b629-3e10473b971b)

Perfect! Except I didn't have any credentials to input, so I tried something else. I went back to the home page and looked at the source code for the main page to see if there was anything hidden in the HTML. Luckily, there was, and this is what I found:

![Screenshot 2024-07-30 212048](https://github.com/user-attachments/assets/4d3c01e4-c314-426a-8305-3800dd0c31ec)

`Username: R1ckRul3s`

I then went back to the login page to try the username and see if I could get some type of error that might provide some more information.

![Screenshot 2024-07-30 214709](https://github.com/user-attachments/assets/90c8c09b-65f8-4954-b7f4-c72ae47415ef)

Although this didn't work, I then went back to look through the other directories. `robots.txt` was accessible and had only one word: `Wubbalubbadubdub`.

![Screenshot 2024-07-30 214822](https://github.com/user-attachments/assets/842dbe86-1670-4d48-aff4-56a33bd502f6)

I then went back to the login page and tried these credentials:

```
Username: R1ckRul3s
Password: Wubbalubbadubdub
```

![Screenshot 2024-07-30 215116](https://github.com/user-attachments/assets/fdf4313d-9344-48f8-b4ec-73889ad1c7d4)

Success! I now had access to the page
After looking through the tabs and being blocked from accessing the all other pages besides `Commands` I tried a basic linux command `whoami`

![Screenshot 2024-07-30 215659](https://github.com/user-attachments/assets/210d43b9-606f-43c8-8a9f-37f5c38655c8)

This command gave me the return `www-data` so I tried to execute other linux commands, starting with `ls` to try to access the directory

![Screenshot 2024-07-30 215748](https://github.com/user-attachments/assets/c243a679-a7b1-46ab-8263-403fc0e74714)

After reading through the files inside the directory I tried to use the `cat` command on  `Sup3rS3cretPickl3Ingred.txt` but was shown this error message. 

![Screenshot 2024-07-30 220105](https://github.com/user-attachments/assets/f3b0bf3d-22b8-48d1-b754-aef673c84379)

So instead I tried to access the file through the URL `10.10.240.224/Sup3rS3cretPickl3Ingred.txt` which worked, revealing the first ingrediant, `mr. meeseek hair`

![Screenshot 2024-07-30 220630](https://github.com/user-attachments/assets/0379793f-5308-4d09-a111-acc72e68f378)

![Screenshot 2024-07-30 220725](https://github.com/user-attachments/assets/ead57467-0326-4bf3-a954-8c2b254fcae9)

First flag found!

I then tried to access the other files in the directory through the URL. Opening the `clue.txt` file revealed this clue:

![Screenshot 2024-07-30 220807](https://github.com/user-attachments/assets/2d086dab-fdcd-4ac0-87ef-df1a2acb6400)

I then went back to the command line and tried `ls /home` which revelaed these two directories:

![Screenshot 2024-07-30 221058](https://github.com/user-attachments/assets/0d709c22-6996-4a9e-b456-4ebe8c3e7861)

Going further into the directory using `ls /home/rick` revealed this:

![Screenshot 2024-07-30 221141](https://github.com/user-attachments/assets/06eb847c-8f47-4569-b891-d467eb35d952)

When I tried to access this file the same way through the URL, it didn't work. At this point, I got stuck and had to do some outside research. While searching for other commands to use instead of `cat`, I found an old Reddit post that suggested using `less` or `bat`.

![Screenshot 2024-07-30 221549](https://github.com/user-attachments/assets/f80d3ac8-1a73-44a2-bdb0-e754cd9ef2d6)

Trying `less /home/rick/second ingredients` in the command panel revealed this:

![Screenshot 2024-07-30 221748](https://github.com/user-attachments/assets/add6636e-bd9a-41c1-adf2-ad701fbbd682)

Second flag found!

![Screenshot 2024-07-30 221829](https://github.com/user-attachments/assets/91a9033c-c6e0-48f9-a528-87437e36e072)

After finding the second flag, I headed back to the command panel to try to look around the directories further. I tried accessing `root` but to no avail. I looked through the manual page for the `sudo` command and found this:

![Screenshot 2024-07-30 222838](https://github.com/user-attachments/assets/9cf46e51-bacd-4654-9bc2-53f6dfa7446a)

The -l tag lists the allowed commands for the specified user, so I entered `sudo -l` into the command panel and was shown this: 

![Screenshot 2024-07-30 223018](https://github.com/user-attachments/assets/df88c6dd-561a-4c3f-ab87-572288a80b3f)

This message indicates that I can use all sudo commands with no password, so I tried `sudo ls /root` and was shown another list of direcotires: 

![Screenshot 2024-07-30 223159](https://github.com/user-attachments/assets/51600591-c8df-4c40-afe0-a8c2c8d638c5)

I then tried `sudo less /root/3rd.txt` and got the third and final flag!

![Screenshot 2024-07-30 223323](https://github.com/user-attachments/assets/7ad6664f-54e7-43f4-b438-adc77168a907)

![Screenshot 2024-07-30 223607](https://github.com/user-attachments/assets/8eb0d3f8-7ea8-47b7-b528-b177929fd7aa)

## Conclusion
This CTF challenge was a great way to apply various skills in a practical environment. It required a combination of enumeration, command line proficiency, and creative problem-solving to find all three flags. The challenge highlighted the importance of thorough exploration and persistence when facing obstacles.

One of the key takeaways from this exercise was the necessity of understanding and using different tools effectively. Running nmap scans provided critical information about open ports and services, while gobuster helped discover hidden directories and files. Additionally, the use of basic Linux commands and familiarity with different file manipulation tools were essential for navigating and extracting the necessary information.

Another important aspect was the value of researching and leveraging external resources. When I encountered difficulties accessing certain files, consulting online forums and documentation provided alternative methods to achieve my goals. This emphasized the importance of continuous learning and adaptability in the field of cybersecurity.

Overall, this CTF challenge reinforced my understanding of the fundamentals of web hacking and network security. It also demonstrated how different elements of cybersecurity knowledge come together in a real-world scenario. I look forward to tackling more challenges like this in the future, as they provide invaluable hands-on experience and contribute to my growth as a cybersecurity professional.


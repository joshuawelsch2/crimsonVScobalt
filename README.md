# crimsonVScobalt
Red vs. Blue team project for the Vanderbilt Cybersecurity Bootcamp
Used nmap and discovered that 192.168.1.105 had an http port open. Concluded that this must be the capstone box which housed the web server. 

![Full real nmap logged IP addresses.jpg]

Used the dirb tool to locate the hidden directory on the target site. 

![Hidden directory on the web server.jpg]


Going to 192.168.1.105/server-status revealed three employees (Ryan, Hannah, and ashton) and their roles within the company. As I continued to search the site, it was suggested that 192.168.1.105/company_folders/secret_folder/ is potentially accessible. However, when I tried to access it, I realized I needed username and password authentication. Using the names of the three employees as usernames, I ran Hydra in my Kali Linux box against this directory.

Command: hydra -l ashton -P /root/Downloads/rockyou.txt -s 80 -f 192.168.1.105/company_folders/secret_folder

![found the real password.jpg]

This gave me access to the /secret files/ directory, which led me to this note:

![the inside of the website.jpg]


Cracked the password hash using Crack Station:

![cracked hash.jpg]

Now I have access to the WebDav file share. This allows me to upload a PHP reverse shell onto the 192.168.1.105 server using MSVenom.

![hopefully the payload that actually works.jpg]


Exporting that payload as a PHP script, I pulled the script into the WebDav fileshare, attempted to connect, and was stumped as to why it wasn’t working. 

![code injection.jpg]

I repeated the same steps, trying different exploits, and was unable to get a shell into the system. 

![dead reverse php sessions.jpg]

The solution was staring me in the face. I realized I had to actually run the script for the session to open. Using the access that the web dav had, I put it on a public facing part of the website and click on it, after I open the session.

![ACTUALLY GOT IN!!.jpg]

Success!

I monitored the exploit and attempts from alerts that I set in Kibana. Those alerts, metrics, and recommended mitigations can be found in the second half of the following powerpoint, which as a whole is a soft copy which could be presented to a client. 

<embed src="/Desktop/cobaltVScrimson repository/Copy of Red vs Blue Team Project slides FINAL JW.pdf" type=application/pdf>

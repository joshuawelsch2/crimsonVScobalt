# crimsonVScobalt
Red vs. Blue team project for the Vanderbilt Cybersecurity Bootcamp

Used nmap and discovered that 192.168.1.105 had an http port open. Concluded that this must be the capstone box which housed the web server. 

![Full real nmap logged IP addresses](https://user-images.githubusercontent.com/82197485/145508374-d4d86b25-1fb6-4fb2-92a0-5743594a5749.jpg)

Used the dirb tool to locate the hidden directory on the target site. 

![Hidden directory on the web server](https://user-images.githubusercontent.com/82197485/145508464-9bbfcb70-4aca-4a84-a1cd-9ec45e3a99d8.jpg)

Going to 192.168.1.105/server-status revealed three employees (Ryan, Hannah, and ashton) and their roles within the company. As I continued to search the site, it was suggested that 192.168.1.105/company_folders/secret_folder/ is potentially accessible. However, when I tried to access it, I realized I needed username and password authentication. Using the names of the three employees as usernames, I ran Hydra in my Kali Linux box against this directory.

Command: hydra -l ashton -P /root/Downloads/rockyou.txt -s 80 -f 192.168.1.105/company_folders/secret_folder

![found the real password](https://user-images.githubusercontent.com/82197485/145508493-602c17cb-7121-418a-b528-800c8d119c30.jpg)

This gave me access to the /secret files/ directory, which led me to this note:

![the inside of the website](https://user-images.githubusercontent.com/82197485/145508515-a3ea1eab-2589-4bf6-abee-281b45248333.jpg)

Cracked the password hash using Crack Station:

![cracked hash](https://user-images.githubusercontent.com/82197485/145508577-fe38e895-02cc-4598-8fea-8ff5b112ad4d.jpg)


Now I have access to the WebDav file share. This allows me to upload a PHP reverse shell onto the 192.168.1.105 server using MSVenom.

![hopefully the payload that actually works](https://user-images.githubusercontent.com/82197485/145508606-15501206-d588-4b85-acbb-855079aacad5.jpg)


Exporting that payload as a PHP script, I pulled the script into the WebDav fileshare, attempted to connect, and was stumped as to why it wasn’t working. 

![code injection](https://user-images.githubusercontent.com/82197485/145508717-6d72b18a-a3ce-442d-98d2-bd1b7716952a.jpg)


I repeated the same steps, trying different exploits, and was unable to get a shell into the system. 

![dead reverse php sessions](https://user-images.githubusercontent.com/82197485/145508736-ed113725-9231-4cec-8a24-b6e2baa2620d.jpg)


The solution was staring me in the face. I realized I had to actually run the script for the session to open. Using the access that the web dav had, I put it on a public facing part of the website and click on it, after I open the session.

![ACTUALLY GOT IN!!](https://user-images.githubusercontent.com/82197485/145508753-e69f872a-5a7d-478a-a209-22dfee3d5938.jpg)


Success!

I monitored the exploit and attempts from alerts that I set in Kibana. Those alerts, metrics, and recommended mitigations can be found in the second half of the following powerpoint, which as a whole is a soft copy which could be presented to a client. 

[Copy of Red vs Blue Team Project slides FINAL JW.pdf](https://github.com/joshuawelsch2/crimsonVScobalt/files/7689761/Copy.of.Red.vs.Blue.Team.Project.slides.FINAL.JW.pdf)

<embed src="https://github.com/joshuawelsch2/crimsonVScobalt/files/7689761/Copy.of.Red.vs.Blue.Team.Project.slides.FINAL.JW.pdf" type="application/pdf">

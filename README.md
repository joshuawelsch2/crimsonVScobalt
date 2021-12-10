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

![1](https://user-images.githubusercontent.com/82197485/145510649-1d016ee9-3a4e-448f-8ad9-ad8e3d9e5180.jpg)
![2](https://user-images.githubusercontent.com/82197485/145510733-442ce0f6-23b5-425f-90bd-55e6c718c9fe.jpg)
![3](https://user-images.githubusercontent.com/82197485/145510664-a5d29238-fbb3-4cb5-851f-57b64e125f79.jpg)
![4](https://user-images.githubusercontent.com/82197485/145510665-c35dd2c8-7c7d-4b03-9374-40964ec0b85d.jpg)
![5](https://user-images.githubusercontent.com/82197485/145510666-f811233a-99e8-4f01-8f05-aaca82ada115.jpg)
![6](https://user-images.githubusercontent.com/82197485/145510667-269a9926-df0d-4879-8d2b-761ba10480bf.jpg)
![7](https://user-images.githubusercontent.com/82197485/145510668-4294955b-d155-40eb-bde2-3b61fc80951d.jpg)
![8](https://user-images.githubusercontent.com/82197485/145510670-512d3d5f-78d7-4dea-b77f-8303a6161f7e.jpg)
![9](https://user-images.githubusercontent.com/82197485/145510671-917ff9af-d6f0-4a1f-8c74-b7b858462449.jpg)
![10](https://user-images.githubusercontent.com/82197485/145510673-4cb4b029-a130-45e0-94ff-b40cc4a70e45.jpg)
![11](https://user-images.githubusercontent.com/82197485/145510674-f0a3803f-885e-4079-81aa-66ff518e6fcc.jpg)
![12](https://user-images.githubusercontent.com/82197485/145510675-13f3ba5c-c345-44ed-8f5f-c972020a01b6.jpg)
![13](https://user-images.githubusercontent.com/82197485/145510676-7e4a5ccf-2738-41dd-a549-098ff20efdb9.jpg)
![14](https://user-images.githubusercontent.com/82197485/145510677-174bfb43-f737-4892-bdb0-cc4401921c4c.jpg)
![15](https://user-images.githubusercontent.com/82197485/145510678-2967c75a-0354-4a66-8908-9bb1d2ce04ea.jpg)
![16](https://user-images.githubusercontent.com/82197485/145510679-51870dd7-52bd-4c19-8faf-af4c89b91450.jpg)
![17](https://user-images.githubusercontent.com/82197485/145510680-b584a1aa-f128-4a34-a2b4-9aadb2b1781d.jpg)
![18](https://user-images.githubusercontent.com/82197485/145510681-be59c7f0-d1d4-41dc-829a-288bc89ae109.jpg)
![19](https://user-images.githubusercontent.com/82197485/145510682-3b3d632d-361c-4ac2-bea6-cdb10406dcd0.jpg)
![20](https://user-images.githubusercontent.com/82197485/145510683-5552e30b-2c27-41bb-b707-14fbcdeb86dd.jpg)
![21](https://user-images.githubusercontent.com/82197485/145510684-488624a2-f73f-428c-8a0c-8e4a59553975.jpg)
![22](https://user-images.githubusercontent.com/82197485/145510685-ed259d75-87e3-4569-afcd-70af87896112.jpg)
![23](https://user-images.githubusercontent.com/82197485/145510686-08333056-4296-475f-a684-095939ae70d4.jpg)



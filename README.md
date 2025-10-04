# CWES File Inclusion - Skill assessment
# Hey, in this Readme you will find write up for Skill assessment lab in File Inclusion module in Hack The Box platform. Main idea of writing this write up is that on first try i couldn't do it and eventually gave up but on second try I decided to think outside of the box )
## Let's Start

# The First thing I did was to enumerate website and get information about endpoints here are two of them that were intereseting to me first
![LFI PoC](images/1.png)
## image.php that was returning image
![SQLi PoC](images/2.png)
## apply.php from which we could upload a file

# First thing we need to do is to get LFI so let's look what we can get from image php file
![SQLi PoC](images/3.png)
## from ffuf's result i have got plenty of things but the working payload is ....//....//....//....//etc/passwd
![SQLi PoC](images/4.png)
# Let's see the source code of the files that we are dealing with
![SQLi PoC](images/5.png)
## image php has got a function file_get_contents which will be a problem for us because we can only read files with this lfi (this was the main thing that i was struggling with, i was trying to get rce with this file which is impossible)
## Let's try to upload file and see what's happening in backend
![SQLi PoC](images/6.png)
## interesting request to /api/application.php let's read it
![SQLi PoC](images/7.png)
## as we see from here names of the files that we upload are hashed and then are added to /uploads directory in web
# From this moment i have copied this source code to host on my machine to get precise name of the file that im uploading becuase i was having problems with requesting it with LFI
![SQLi PoC](images/9.png)
![SQLi PoC](images/8.png)
## this is the name of shell.php that i would have been uploaded to target machine
# Then i decided to get rce requesting uploaded file from image.php endpoint and it was unsuccessful
![SQLi PoC](images/11.png)
## If you are wondering log poisoning would have worked with this method also no :)
# From this moment i changed my attention to contact.php endpoint because from the very beginning i found there region attribute but didnt care about that after finding LFI in image.php
![SQLi PoC](images/12.png)
# Getting LFI from contact.php endpoint
![SQLi PoC](images/14.png)
## url encoding didnt help
![SQLi PoC](images/15.png)
## but url encoding twice helped :)
# Let's read what is inside of contact.php from image.php LFI endpoint
![SQLi PoC](images/16.png)
## as we see the script automatically appends .php in the end of the file and it is becoming executed
# Trying to prove execution
![SQLi PoC](images/17.png)
## here I have written in query ../apply.php in url encoded format
# Getting RCE
## in the locally hosted application i will write another script to automatically execute php code upon requesting it 
![SQLi PoC](images/18.png)
## this is the payload
![SQLi PoC](images/19.png)
## Name of the file in the system
![SQLi PoC](images/20.png)
## Double encoded form
![SQLi PoC](images/21.png)
## and Finally RCE
![SQLi PoC](images/22.png)
# Thanks for attention, Happy hacking

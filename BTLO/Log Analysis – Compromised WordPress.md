<h1 align="center">Log Analysis – Compromised WordPress</h1>

## Scenario

One of our WordPress sites has been compromised but we're currently unsure how. The primary hypothesis is that an installed plugin was vulnerable to a remote code execution vulnerability which gave an attacker access to the underlying operating system of the server.

## Tools

- [cloudvyzor.com/logpad](https://cloudvyzor.com/logpad)
- grep
- sort
- uniq
- awk
  
## File

- [File.zip](https://blueteamlabs.online/storage/files/SjuVmfhhCn5PEcmj5FMESd2FpEq9MT.zip)
  
## Challenge Submission

[time stamp ](https://cloudvyzor.com/logpad/?query&database=sandbox-12825c345f1c16217a6e45112e9237f7) solved

Q1. Identify the URI of the admin login panel that the attacker gained access to (include the token) (3 points)

- `wp-login.php?*token=` 
- We know from the scenario hypothesis that attack got into our system we will first look at the wp-login.php
![[{09AC1DBD-3478-4542-8D33-8B5FC04C1DCD}.png]]


Q2. Can you find two tools the attacker used? (3 points)

- `wp-login.php -wp-incl -css -admin -js` sqlmap
![[{1A4ADB04-29BF-40CC-BD45-900D4A525038}.png]]
- `` grep -E 'GET|POST' access.log | grep -v 'Mozilla' | awk -F'"' '{print $6}' | sort | uniq`
![[{D83B2669-6F0F-46FA-9D66-C4C7D4EF2B6B}.png]]

Q3. The attacker tried to exploit a vulnerability in ‘Contact Form 7’. What CVE was the plugin vulnerable to? (Do some research!) (4 points)

- [https://blog.wpsec.com/contact-form-7-vulnerability/](https://blog.wpsec.com/contact-form-7-vulnerability/)

Q4. What plugin was exploited to get access? (4 points)

- CVE-2020-35489: Unrestricted File Upload Vulnerability
- Now we know the vulnerability is in the upload we will search for upload

Q5. What is the name of the PHP web shell file? (3 points)

- `wp-content/uploads/*/*.php`
![[{8F5186E8-EB6F-45AC-BF56-0981D58698C4}.png]]
- Now we know the file name is : fr34k.php
  
Q6. What was the HTTP response code provided when the web shell was accessed for the final time? (3 points)

- 404
- `fr34k.php`
![[{74AF68E5-D42B-48C6-BCD9-4D44F2A72A06}.png]]

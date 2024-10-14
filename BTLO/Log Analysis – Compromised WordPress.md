<h1 align="center">Log Analysis – Compromised WordPress</h1>

## Scenario
One of our WordPress sites has been compromised but we're currently unsure how. The primary hypothesis is that an installed plugin was vulnerable to a remote code execution vulnerability which gave an attacker access to the underlying operating system of the server.

## Tools
https://cloudvyzor.com/logpad


## Challenge Submission
- Identify the URI of the admin login panel that the attacker gained access to (include the token) (3 points)
  wp-login.php?*token=*
  
- Can you find two tools the attacker used? (3 points)
  wp-login.php  -wp-incl -css -admin -js
  
- The attacker tried to exploit a vulnerability in ‘Contact Form 7’. What CVE was the plugin vulnerable to? (Do some research!) (4 points)
  https://blog.wpsec.com/contact-form-7-vulnerability/
  
- What plugin was exploited to get access? (4 points)
  
- What is the name of the PHP web shell file? (3 points)
  wp-content/uploads/
- What was the HTTP response code provided when the web shell was accessed for the final time? (3 points)

**Cyber Security Project Report** 

Bug Hunt 101 – Recon to Report 



1\. Introduction 



The goal of this assignment is to gain knowledge of every detail of the bug bounty process from 

reconnaissance to finally submitting a formal report of the vulnerability. The basic idea of this 

assignment is quite simple and straightforward. The primary idea of this assignment is to use 

knowledge of common vulnerabilities of web applications in a lab environment. 

For this purpose, OWASP Juice Shop was selected as the target application on which the tests 

will be performed. Juice Shop is a vulnerable web application developed specifically for creating 

a learning environment for web application security tests. Tests in this project are performed in a 

purely ethical way in the authorized lab. 



2\. Environment Setup 



A local test environment was created using Docker to eliminate problems with dependencies and 

configuration. 

Technologies/Tools Used: 

By offering a 

OWASP Juice Shop 

Docker Desktop (WSL2 backend) -bash 

Web Browser (Chrome) 

Windows 11 

Deployment Method: 

OWASP Juice Shop was deployed using the official Docker image. 

docker run -d -p 3000:3000 bkimminich/juice-shop 

The application was accessed via: 

http://localhost:3000



3\. Reconnaissance

&nbsp;

Reconnaissance was done to identify open ports and understand how the application is exposed. 

3.1 Target Identification 

Target Application: OWASP Juice Shop 

Host: localhost 

Port: 3000 

3.2 Port Scanning Using Nmap 

The following Nmap command was used: 

nmap -p 3000 localhost 

Many of us move to what we will call ' The Field' to get away from this very thing, or at least 

repress the knowledge of it. 

TCP port 3000 was found open, which indicated that the Juice Shop service was running and 

accessible. 

That meant the application had successfully deployed and was ready for further testing.

&nbsp;

4\. Vulnerability Findings 



Testing revealed several vulnerabilities, each with straightforward steps and impact that clearly 

describe the vulnerability.



4.1 Cross-Site Scripting (XSS) 

Description 

A type of reflected Cross-Site Scripting (XSS) vulnerability was found in the search function in 

the application. This is because the application does not sanitize the user input before it is 

rendered on the browser. 

Affected Functionality 

Search feature 

Payload Used 

<script>alert(1)</script>

Steps to reproduce: 

1\. Open OWASP Juice Shop homepage 

2\. Find the search bar 

3\. Input XSS payload into the search box 

4\. Submit the search 

5\. A JavaScript alert box is executed in the browser 

Effect 

“An attacker may leverage the identified vulnerability to inject malicious JavaScript in a victim's 

browser. This may result in session hijacking, stealing cookies, and/or malicious redirection.” 

Severity 

Medium



4.2 Insecure Direct Object Reference (IDOR) 

Insecure Direct Object Reference (IDOR) in Product Reviews Endpoint 

Affected URL: 

GET /rest/reviews?productId=20 

Payload: 

productId=20 

Steps to Reproduce: 

1\. Log in as a normal user. 

2\. Open the browser developer tools (F12) and navigate to the Network tab. 

3\. Visit any product page and observe the request fetching product reviews. 

4\. Capture the request: 

5\. GET /rest/reviews?productId=<original\_id> 

6\. Modify the productId parameter to another valid value (e.g., 20). 

7\. Send the modified request. 

8\. Observe that reviews written by other users are returned. 

Impact: 

The code contains a problem where it enables unauthorized access to reviews posted by the end

users via direct references to internal objects using the identifiers provided by the end-users. The 

code is prone to the IDOR attack, where the attackers will be able to view information for 

different end-users.




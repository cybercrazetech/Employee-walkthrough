# [Employee-Walkthrough]

## Introduction

[This box was made to show the importance of enumeration, critical thinking and the usual mess and error while dealing with public exploit scripts in real life. This is done via mimicking a company hiring for employees, and some manual site development that leads to code execution. Exploits used are CVE-2017-7494 and a little command injection for foothold, while CVE-2021-3156 for privilege escalation.]

### Access

Passwords:

| User  | Password                            |
| ----- | ----------------------------------- |
| root | [youshallnotcrackthis] |
| cybercraze | [youshallnotcrackthis] |
| guest (samba user)  | [guest] |

### Key Processes

[Services: SSH, HTTP, SMB
HTTP:
domain: designer.htb
subdomain: underdevelopment.designer.htb

SMB version: 4.5.9 --> vulnerable to CVE-2017-7494
].

### Docker

[docker run -d -p 137-139:137-139 -p 445:445 -p 6699:6699 -v /home/cybercraze/candidates:/candidates -v /root/backups:/var/www --restart=always --name=samba vulnerables/cve-2017-7494

*hosting vulnerable samba on docker to prevent direct access as root
*/candidates is the path to samba share "candidates"
*backup html files stored in /var/www
]

### Notes

[
1. Emails ending with @designer.htb scatter around the hosted website to indicate the domain name
2. credentials for samba share "guest:guest" is provided on the website for candidates to fill and submit the application form
3. the samba share "candidates" contains a job-application-form.docx and a submission directory to submit the application form
4. the samba share is only accessible via the creds "guest:guest"
5. all the exploit scripts and fixes are included in this zip file:
https://drive.google.com/file/d/1b0Pbc8xdLGaVjGe8nNJqP2MityPsKfJD/view?usp=sharing
for any errors resulted from using the exploits, refer to the Writeup section to fix
players are expected to search and fix the error themselves
]

## Writeup

[]

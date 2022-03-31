# Employee-Walkthrough

## Introduction

This box was made to show the importance of enumeration, critical thinking and the usual mess and error while dealing with public exploit scripts in real life. This is done via mimicking a company hiring for employees, and some manual site development that leads to code execution. Exploits used are CVE-2017-7494 and a little command injection for foothold, while CVE-2021-3156 for privilege escalation.

### Access

Passwords:

| User  | Password                            |
| ----- | ----------------------------------- |
| root | youshallnotcrackthis |
| cybercraze | youshallnotcrackthis |
| guest (samba user)  | guest |

### Key Processes

Services: SSH, HTTP, SMB
HTTP:
domain: designer.htb
subdomain: underdevelopment.designer.htb

SMB version: 4.5.9 --> vulnerable to CVE-2017-7494

### Docker

docker run -d -p 137-139:137-139 -p 445:445 -p 6699:6699 -v /home/cybercraze/candidates:/candidates -v /root/backups:/var/www --restart=always --name=samba vulnerables/cve-2017-7494

*hosting vulnerable samba on docker to prevent direct access as root

*/candidates is the path to samba share "candidates"

*backup html files stored in /var/www

### Notes

1. Emails ending with @designer.htb scatter around the hosted website to indicate the domain name
2. All inputs in http://designer.htb are sanitised and there would not be any injection attack such as ssti or xss
3. credentials for samba share "guest:guest" is provided on the website for candidates to fill and submit the application form
4. the samba share "candidates" contains a job-application-form.docx and a submission directory to submit the application form
5. the samba share is only accessible via the creds "guest:guest"
6. all the exploit scripts and fixes are included in this zip file:

https://drive.google.com/file/d/1b0Pbc8xdLGaVjGe8nNJqP2MityPsKfJD/view?usp=sharing

for any errors resulted for using the exploits, refer to the Writeup section to fix

#### *players are expected to search and fix the errors themselves*

## Writeup

1. Nmap
2. enumerate the website. found emails ending with @designer.htb. (info@designer.htb, contact@designer.htb, etc) edit /etc/hosts to set designer.htb as the domain name
3. button "contact us" redirects us to contact.html. reveal creds "guest:guest" for samba share to those interested in job application
4. Checking Samba Share "candidates"

$ smbclient -L designer.htb -U guest  

Enter WORKGROUP\guest's password: guest                                                               
                                                                                                 
        Sharename       Type      Comment                                                        
        ---------       ----      -------                                                        
        candidates      Disk      SambaShare for Job Application                                 
        IPC$            IPC       IPC Service (Designer Company)              
        
        
$ smbclient \\\\designer.htb\\candidates -U guest

Enter WORKGROUP\guest's password: guest

Try "help" to get a list of possible commands.

smb: \> ls
  .                                   D        0  Thu Mar 31 11:00:04 2022
  ..                                  D        0  Wed Mar 30 17:15:27 2022
  job-application-form.docx           A    19975  Sat Mar 26 00:44:53 2022
  submission                          D        0  Fri Mar 25 21:48:03 2022

                9336140 blocks of size 1024. 1836012 blocks available
                

5. 
  
<img src=""/>

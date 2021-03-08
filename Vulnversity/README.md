# Vulnversity

TryHackMe Live Room

## Info
**Title**  
VulnUniversity

**Created By**  
[TryHackMe](https://tryhackme.com/p/tryhackme)  

**Link**  
[here](https://tryhackme.com/room/vulnversity)  


***NOTE***  
 I've used this room as a challenge... My solution might be different from TryHackMe's.

## Scanning
I found a web server running on port 3333. The `index` page was a simple `HTML` template without any helpful information or links.

```
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X 
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu 
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 
```

To get better knowledge about the webserver, I performed an advanced scan. Nessus mentioned a browsable directory vulnerability which led me to a `/internal` directory. The `php` index page had an upload file form.

## Exploiting

Trying to exploit the upload form, the webserver wasn't accepting any file extension. To solve this issue, I intercepted the `POST` request and brute-forced multiple extensions. The plan was to try every helpful extension, though I used a much smaller wordlist from TryHackMe to save time.

The 5th `POST` request I sent was successful. The server accepted a `.phtml` file and sent a success message. I used the vulnerability to upload and execute a PHP reverse shell, renamed to `.phtml`. The user flag was under `/home/bill/user.txt`.

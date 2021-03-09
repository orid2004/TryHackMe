# Kenobi

TryHackMe Live Room

## Info

**Title**  
Kenobi
**Created By**  
[TryHackMe](https://tryhackme.com/p/tryhackme)

**Link**  
[here](https://tryhackme.com/room/kenobi)

***NOTE***  
I've used this room as a challenge... My solution might be different from TryHackMe's.


## Scanning
An advanced Nessus scan found two critical vulnerabilities.
1. `ProFTPD mod_copy Information Disclosure`. An unauthenticated client can run the `SITE CPFR` and the `SITE CPTO` commands to change files' location.
2. `NFS Exported Share Information Disclosure`. The directory `var` could be mounted.

Putting the vulnerabilities together, I was able to copy the `id_rsa` key from `.ssh` to `/var`. `showmounts \e <host>` confirmed the the `/var` directory can be indeed mounted, and I executed `sudo mount -t nfs <host>:/var var_host
`.


## Privilege Escalation
[TryHackMe](https://tryhackme.com/p/tryhackme) has the credit for this solution, as I missed it. The idea was to find a binary suid file, that executes another binary file as root. The `/usr/bin/menu` file matched the requirements. TryHackMe executed the `strings` command and found that `menu` executes `curl` as root. We could exploit this issue by creating a `curl` file (which executes the shell), adding it to the head of the `PATH` variable, and executing the `menu` binary again. 

## Credits
Ori David  
[TryHackMe](https://tryhackme.com/p/tryhackme) 

## Contact
orid2004@gmail.com

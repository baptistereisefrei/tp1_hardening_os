# TP1 partie 2
Baptiste Reis

# Part II : Files and permissions

**Dans un OS, en particulier Linux, on dit souvent que "tout est fichier".**

**C'est la première barrière de sécurité, (beaucoup) trop souvent négligée, alors qu'elle est extrêmement efficace et robuste.**

## 1. Listing POSIX permissions

🌞 **Déterminer les permissions des fichiers/dossiers...**
```
[Draskov@efrei-xmg4agau1 ~]$ ls -l /etc/passwd
-rw-r--r--. 1 root root 1057 Feb  3 12:41 /etc/passwd
[Draskov@efrei-xmg4agau1 ~]$ ls -l /etc/shadow
----------. 1 root root 992 Feb  3 12:44 /etc/shadow
[Draskov@efrei-xmg4agau1 ~]$ ls -l /etc/ssh/sshd_config
-rw-------. 1 root root 3667 Apr 18  2024 /etc/ssh/sshd_config
[Draskov@efrei-xmg4agau1 ~]$ ls -ld /root
dr-xr-x---. 3 root root 142 Feb  3 12:56 /root
[Draskov@efrei-xmg4agau1 ~]$ ls -ld /home/Draskov
drwx------. 3 Draskov Draskov 111 Feb  3 15:16 /home/Draskov
[Draskov@efrei-xmg4agau1 ~]$ ls -l /bin/ls
-rwxr-xr-x. 1 root root 140872 Apr 20  2024 /bin/ls
[Draskov@efrei-xmg4agau1 ~]$ ls -l /bin/systemctl
-rwxr-xr-x. 1 root root 305680 Apr  8  2024 /bin/systemctl
```
## 2. Extended attributes

🌞 **Lister tous les programmes qui ont le bit SUID activé**
```
[Draskov@efrei-xmg4agau1 ~]$ find / -perm -4000 -type f 2>/dev/null
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/umount
/usr/bin/su
/usr/bin/crontab
/usr/bin/passwd
/usr/bin/sudo
/usr/sbin/unix_chkpwd
/usr/sbin/pam_timestamp_check
/usr/sbin/grub2-set-bootflag
```

🌞 **Rendre le fichier de configuration du serveur OpenSSH immuable**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo chattr +i /etc/ssh/sshd_config

[Draskov@efrei-xmg4agau1 ~]$ sudo lsattr /etc/ssh/sshd_config
----i----------------- /etc/ssh/sshd_config

Dans nano de /etc/ssh/sshd_config :
[ File '/etc/ssh/sshd_config' is unwritable ]
```

## 3. Protect a file using permissions

🌞 **Restreindre l'accès à un fichier personnel**
```
[Draskov@efrei-xmg4agau1 ~]$ mkdir ~/public_folder
[Draskov@efrei-xmg4agau1 ~]$ chmod 777 ~/public_folder
[Draskov@efrei-xmg4agau1 ~]$ echo "Leo aime la biere" > ~/public_folder/dont_readme.txt
[Draskov@efrei-xmg4agau1 ~]$ chmod 600 ~/public_folder/dont_readme.txt
[Draskov@efrei-xmg4agau1 ~]$ ls -l ~/public_folder/dont_readme.txt
-rw-------. 1 Draskov Draskov 18 Feb  3 15:49 /home/Draskov/public_folder/dont_readme.txt

[Draskov@efrei-xmg4agau1 public_folder]$ echo " et les huitres" >> dont_readme.txt
[Draskov@efrei-xmg4agau1 public_folder]$ cat dont_readme.txt
Leo aime les ratons laveurs
 et les huitres

[meow@efrei-xmg4agau1 public_folder]$ cat dont_readme.txt
cat: dont_readme.txt: Permission denied
[meow@efrei-xmg4agau1 public_folder]$ echo "lol" >> dont_readme.txt
-bash: dont_readme.txt: Permission denied

[root@efrei-xmg4agau1 public_folder]# echo " et le vin" >> dont_readme.txt
[root@efrei-xmg4agau1 public_folder]# cat dont_readme.txt
Leo aime la biere
 et le vin
```
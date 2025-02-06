# TP1 partie 1

Baptiste Reis

# Part I : User management

**Hum, cette partie est censée être envoyée vite fait bien fait ! Prouvez-le moi :D**

note : j'ai ajouté mon utilisateur dans la liste de sudoers en faisant la commande :

note : certains outputs sont tronqués car trop longs.
```
usermod -aG wheel Draskov
```

## 1. Existing users

🌞 **Déterminer l'existant :**

Liste utilisateurs :
```
[Draskov@efrei-xmg4agau1 ~]$ cut -d: -f1 /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
dbus
tss
sssd
sshd
chrony
it4
tcpdump
Draskov
...
```
Liste groupes :
```
[Draskov@efrei-xmg4agau1 ~]$ cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:it4,Draskov
cdrom:x:11:
mail:x:12:
man:x:15:
dbus:x:81:
ssh_keys:x:101:
sgx:x:994:
it4:x:1000:
tcpdump:x:72:
Draskov:x:1001:
...
```
Liste de groupes auquel appartient notre user :
```
[Draskov@efrei-xmg4agau1 ~]$ groups
Draskov wheel
```
🌞 **Lister tous les processus qui sont actuellement en cours d'exécution, lancés par `root`**
```
[Draskov@efrei-xmg4agau1 ~]$ ps -ef -u root
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 10:12 ?        00:00:00 /usr/lib/systemd/systemd --switched-root --system --deserialize 31
root           2       0  0 10:12 ?        00:00:00 [kthreadd]
root           3       2  0 10:12 ?        00:00:00 [rcu_gp]
root           4       2  0 10:12 ?        00:00:00 [rcu_par_gp]
root           5       2  0 10:12 ?        00:00:00 [slub_flushwq]
root           6       2  0 10:12 ?        00:00:00 [netns]
root           8       2  0 10:12 ?        00:00:00 [kworker/0:0H-events_highpri]
root          10       2  0 10:12 ?        00:00:00 [mm_percpu_wq]
root          12       2  0 10:12 ?        00:00:00 [rcu_tasks_kthre]
root          24       2  0 10:12 ?        00:00:00 [inet_frag_wq]
root          25       2  0 10:12 ?        00:00:00 [kauditd]
root          26       2  0 10:12 ?        00:00:00 [khungtaskd]
root          27       2  0 10:12 ?        00:00:00 [oom_reaper]
root          29       2  0 10:12 ?        00:00:00 [writeback]
root          30       2  0 10:12 ?        00:00:00 [kcompactd0]
root          31       2  0 10:12 ?        00:00:00 [ksmd]
root          32       2  0 10:12 ?        00:00:00 [cryptd]
...
```
🌞 **Lister tous les processus qui sont actuellement en cours d'exécution, lancés par votre utilisateur**
```
[Draskov@efrei-xmg4agau1 ~]$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 10:12 ?        00:00:00 /usr/lib/systemd/systemd --switched-root --system --deserialize 31
root           2       0  0 10:12 ?        00:00:00 [kthreadd]
root           3       2  0 10:12 ?        00:00:00 [rcu_gp]
root           4       2  0 10:12 ?        00:00:00 [rcu_par_gp]
root           5       2  0 10:12 ?        00:00:00 [slub_flushwq]
root           6       2  0 10:12 ?        00:00:00 [netns]
root           8       2  0 10:12 ?        00:00:00 [kworker/0:0H-events_highpri]
root          10       2  0 10:12 ?        00:00:00 [mm_percpu_wq]
root          29       2  0 10:12 ?        00:00:00 [writeback]
root          30       2  0 10:12 ?        00:00:00 [kcompactd0]
...
```
🌞 **Déterminer le hash du mot de passe de `root`**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo cat /etc/shadow | grep root
[sudo] password for Draskov:
root:$6$.8fzl//9C0M819BS$Sw1mrG49Md8cyNUn0Ai0vlthhzuSZpJ/XVfersVmgXDSBrTVchneIWHYHnT3mC/NutmPS03TneWAHihO0NXrj1::0:99999:7:::
```

🌞 **Déterminer le hash du mot de passe de votre utilisateur**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo cat /etc/shadow | grep Draskov
Draskov:$6$.YL/az7sA7F6pJOH$7z2otIvlbGDBHj02j7LXNPkw8jsgiivjpoydFPSVdwtPCywbQjpyVDC1DOsPRKGIN/7JRilT1m05Kwya3tjbD.:20122:0:99999:7:::
```

🌞 **Déterminer la fonction de hachage qui a été utilisée**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo cat /etc/login.defs | grep -E "^ENCRYPT_METHOD"
ENCRYPT_METHOD SHA512
```

🌞 **Déterminer, pour l'utilisateur `root`** :
```
Shell :
[Draskov@efrei-xmg4agau1 ~]$ getent passwd root
root:x:0:0:root:/root:/bin/bash

Repertoire perso :
[Draskov@efrei-xmg4agau1 ~]$ echo ~root
/root
```

🌞 **Déterminer, pour votre utilisateur** :
```
Shell :
[Draskov@efrei-xmg4agau1 ~]$ getent passwd Draskov
Draskov:x:1001:1001::/home/Draskov:/bin/bash

Repertoire perso:
[Draskov@efrei-xmg4agau1 ~]$ echo ~Draskov
/home/Draskov
```

🌞 **Afficher la ligne de configuration du fichier `sudoers` qui permet à votre utilisateur d'utiliser `sudo`**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo cat /etc/sudoers |grep -E "^%wheel"
%wheel  ALL=(ALL)       ALL
```

## 2. User creation and configuration

🌞 **Créer un utilisateur :**

```
[Draskov@efrei-xmg4agau1 ~]$ [Draskov@efrei-xmg4agau1 ~]$ sudo groupadd admins
[Draskov@efrei-xmg4agau1 ~]$ sudo useradd -g admins -M -s /usr/sbin/nologin meow

[Draskov@efrei-xmg4agau1 ~]$ getent passwd meow
meow:x:1002:1002::/home/meow:/usr/sbin/nologin
[Draskov@efrei-xmg4agau1 ~]$ groups meow
meow : admins

[Draskov@efrei-xmg4agau1 ~]$ su - meow
Password:
Last failed login: Mon Feb  3 12:43:30 CET 2025 on pts/0
There were 2 failed login attempts since the last successful login.
su: warning: cannot change directory to /home/meow: No such file or directory
This account is currently not available.

[Draskov@efrei-xmg4agau1 ~]$ ssh meow@192.168.56.102
The authenticity of host '192.168.56.102 (192.168.56.102)' can't be established.
ED25519 key fingerprint is SHA256:XD2rc6EQlKwzb/fQHUjuQzbOHMl/5nW2ANUv7owyTkc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.56.102' (ED25519) to the list of known hosts.
meow@192.168.56.102's password:
Last login: Mon Feb  3 12:45:26 2025
Could not chdir to home directory /home/meow: No such file or directory
This account is currently not available.
Connection to 192.168.56.102 closed.
```
🌞 **Configuration `sudoers`**
```
[Draskov@efrei-xmg4agau1 ~]$ echo "meow ALL=(Draskov) NOPASSWD: /bin/ls, /bin/cat, /bin/less, /bin/more" | sudo tee -a /
etc/sudoers
meow ALL=(Draskov) NOPASSWD: /bin/ls, /bin/cat, /bin/less, /bin/more

[Draskov@efrei-xmg4agau1 ~]$ echo "%admins ALL=(ALL) NOPASSWD: /usr/bin/apt" | sudo tee -a /etc/sudoers
%admins ALL=(ALL) NOPASSWD: /usr/bin/apt

[Draskov@efrei-xmg4agau1 ~]$ echo "Draskov ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers
Draskov ALL=(ALL) NOPASSWD: ALL

[meow@efrei-xmg4agau1 Draskov]$ sudo -l
Matching Defaults entries for meow on efrei-xmg4agau1:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY
    HOSTNAME HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC
    LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User meow may run the following commands on efrei-xmg4agau1:
    (Draskov) NOPASSWD: /bin/ls, /bin/cat, /bin/less, /bin/more
    (ALL) NOPASSWD: /usr/bin/dnf

[meow@efrei-xmg4agau1 Draskov]$ sudo -u root dnf
usage: dnf [options] COMMAND

List of Main Commands:
...
```

## 3. Hackers gonna hack

🌞 **Déjà une configuration faible ?**

En faisant sur meow :

```
sudo -u Draskov less /etc/passwd
```
puis en exploitant un problème de sécurité lié à "less" on peut ecrire !/bin/bash pour faire spawn un shell en tant que Draskov, puis comme Draskov peut switch en root sans mot de passe on fait :

```
sudo su
```
Cette escalade de privilège montre un risque de sécurité important.
Depuis meow je suis devenu root.


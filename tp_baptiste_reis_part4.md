# TP1 partie 4
Baptiste Reis

# Part IV : Storage and partitions

Les fichiers, c'est bien beau, mais faut bien des partitions pour les stocker !

**Cette partie est dédiée aux partitions**, quelques subtilités autour, et on en profite pour parler des **options de montage**.

## 1. Existing partitions

🌞 **Déterminer la liste des partitions du système**
```
[root@efrei-xmg4agau1 Draskov]# lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT
NAME        FSTYPE       SIZE MOUNTPOINT
sda                        8G
├─sda1      xfs            1G /boot
└─sda2      LVM2_member    7G
  ├─rl-root xfs          6.2G /
  └─rl-swap swap         820M [SWAP]
```

🌞 **Identifier la partition qui est montée sur `/`**
```
[root@efrei-xmg4agau1 Draskov]# df -h /
Filesystem           Size  Used Avail Use% Mounted on
/dev/mapper/rl-root  6.2G  1.5G  4.8G  24% /
```

## 2. Mount options

🌞 **Déterminer les options de montage de la partition `/`**
```
[root@efrei-xmg4agau1 Draskov]# mount | grep "on / "
/dev/mapper/rl-root on / type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
rw : permet la lecture et l'écriture.

relatime : optimise les mises à jour des timestamps d’accès pour limiter les écritures inutiles.
seclabel : indique que SELinux est activé pour gérer la sécurité des fichiers.
attr2 : active des optimisations pour les attributs étendus de XFS.
inode64 : permet l'utilisation d'inodes 64 bits, utile pour les grands disques.
logbufs=8 et logbsize=32k : améliorent la performance des écritures journalières du système de fichiers.
noquota : désactive la gestion des quotas utilisateurs.
```

🌞 **Monter une partition de type `tmpfs` sur le dossier `/tmp`**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo mount -t tmpfs -o noexec,nosuid,nodev tmpfs /tmp

[Draskov@efrei-xmg4agau1 ~]$ mount | grep "on /tmp"
tmpfs on /tmp type tmpfs (rw,nosuid,nodev,noexec,relatime,seclabel,inode64)

Le test :
[Draskov@efrei-xmg4agau1 ~]$ cp /bin/ls /tmp/
[Draskov@efrei-xmg4agau1 ~]$ chmod 777 /tmp/ls
[Draskov@efrei-xmg4agau1 ~]$ /tmp/ls
-bash: /tmp/ls: Permission denied
```
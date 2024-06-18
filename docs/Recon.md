# Scan Réseau
## PING
### TTL
| TTL | Systeme |
| ---- | ---- |
| 64 | Linux |
| 128 | Windows |
| 255 | Cisco IOS |

## NMAP

| Fonction                                                                           | commande                     |
| ---------------------------------------------------------------------------------- | ---------------------------- |
| Scan all (utilise des script pour déterminé pour obtenir le maximum d'information) | `nmap -A <plage IP>`         |
| Scan réseau                                                                        | `nmap -sP <ip>`              |
| Scan syn                                                                           | `nmap -sS< ip>`              |
| Scan TCP                                                                           | `nmap -sT <ip>`              |
| Scan TCP range                                                                     | `nmap -sT -p- <ip>`          |
| Scan Version du service                                                            | `nmap -sV <ip>`              |
| Scan de l'OS et des services                                                       | `nmap -sO <ip>`              |
| Scan en verbose avec un script                                                     | `nmap -v --script vuln <ip>` |
| Scan Xmas                                                                          | `nmap -sX <ip>`              |
| Scan NULL                                                                          | `nmap -sN <ip>`              |
| Scan FIN                                                                           | `nmap -sF <ip>`              |
| Scan UDP                                                                           | `nmap -sU <ip>`              |

## Net Discover
On fait un scan ARP. Un scan ARP ne peux pas être bloqué
``` sudo netdiscover -i <carte reseau> -r <plage IP> ```

## Partage réseau
- Énumère les partages: `nmblookup -A <ip>`
- Map les partages: `smbmap -H <ip>`
- Client samba: `smbclient -L ///<ip> -W <workgroup> -U <user>`
- Client RPC: `rpclcient -U "" <ip>`
## Data Base
### Redis
port : 6379/tcp
- se connecter à Redis 
```bash
 redis-cli -h <hostname/IP>
```

```SQL
SELECT <index> #choisir une base
INFO #detail du serveur redis
DBSIZE #nb de keys
KEYS * #lister les keys
get "flag" #lire la value de la key "flag"
```
## Web
- Télécharger les fichier un-à-un pour les analyser : `wget <ip serveur>/<emplacement du fichier>`
- Lister arborescence d'un serveur web : `dirb <ip serveur>`
- Lister l'arborescence d'un serveur web : `gobuster dir -u http://10.10.133.151 -w /usr/share/seclists/Discovery/Web-Content/directory-list-1.0.txt`
- Lister l'arborescence d'un serveur web en cherchant les .php: `gobuster dir -u http://10.10.133.151 -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php`
- trier un fichier par ordre alphabet et supp. les doublon `cat <file> | sort | uniq > <out_file>`
- lister les sous-domaine présent sur une page `curl <site> | grep "href=" |cut -d "/" -f 3 | grep "\." | cut -d '"' -f 1 | sort -u`
- `curl <site> | grep "href=" | grep -o 'https://[^"]*' | cut -d "/" -f 3 | sort -u`
- `for url in $(cat liste.txt); do host$url; done | grep "has adress" | cut -d " " -f 4 | sort -u`

- `curl <site> | grep "href=" | grep "\.megacorpone" | grep -v "www*\.megacorpone\.com" | awk -F "http://" '{print $2}' | cut -d '/' -f 1 |cut -d '"' -f 1 | uniq`
- `for url in $(cat lien.txt); do host $url;done | grep "has address" | cut -d " " -f 4 > ip.txt`

## DNS

- 
- Transfert de Zone DNS : `dig -t axfr @dns.bigbusiness.loc bigbusiness.loc` 

## Windows
### Powershell
- ping :
```powershell
tnc 172.31.24.20
```
- ping d'une plage d'adresse :
```powershell
9..11 | % {tnc 172.31.24.$_}
```
- tester le service RDP avec résultat dans un fichier :
```powershell
tnc 172.31.24.20 rdp | Out-File c:\users\pslearner\challenge1.txt
```
- scan de ports avec liste :
```powershell
$ports = 22,53,80,445,3389
$ports | ForEach-Object {$port = $_; if (tnc 172.31.24.20 -Port $port ) {"$port is open" } else {"$port is closed"} }
```
- scan de tout les ports :
```powershell
1..65535 | % {tnc 172.31.24.10 -Port $_}
```
- créer une nouvelle connexion par WinRM, se connecter et la clôture :
```powershell
New-PSSession 172.31.24.20

Enter-PSSession 172.31.24.20

Exit-PSSession
```
- lister les processus sur Windows : 
```powershell
Get-Process | Select-Object -Property ProcessName,Id,Path
```
- chercher un fichier spécifique : 
```powershell
gci c:\users\public\desktop\LAB_FILES\ -Include *.txt,*.ini -File -Recurse -EA SilentlyContinue | Select-String -Pattern "Pwd"
```
-  lister toutes les informations sur les ressources partagées
```powershell
net share  | Out-File c:\users\pslearner\datacollection.txt -Append
```
-  lister toutes les données de connexion de session réseau
```powershell
net session | Out-File c:\users\pslearner\datacollection.txt -Append
```
-  lister toutes les informations utilisateur
```powershell
Get-LocalUser | Select * | Out-File c:\users\pslearner\datacollection.txt -Append
```
-  lister toutes les informations sur les services
```powershell
get-service | Out-File c:\users\pslearner\datacollection.txt -Append
```
-  lister toutes les informations de la carte réseau
```powershell
get-netadapter | Out-File c:\users\pslearner\datacollection.txt -Append
```
-  lister toutes les commandes disponibles pour un utilisateur sur cette machine:
```powershell
get-command | Out-File c:\users\pslearner\availablecommands.txt
```


## Wireshark 

Wireshark en CLI : `tshark -i <interface_name>`

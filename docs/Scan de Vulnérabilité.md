## Nikto
Scanning a website with Nikto can be done using the following command in your terminal or command prompt: 

```bash
nikto -p <target_website>
```
Replace `<target_website>` with the URL of the website you want to scan. For example, if you want to scan `www.example.com`, the command would be:

```bash
nikto -p www.example.com
``` 
This will start a Nikto scan on the target website and display any vulnerabilities that are found.

## Wapiti
Pour scanner un site Web avec Wapiti, vous pouvez utiliser la suite d'outils Nessus. Nessus est l'un des outils les plus populaires pour le scan de sites web et il supporte une grande variété de plugins pour tester les fonctionnalités de ce type.

Voici un exemple de commande que vous pouvez utiliser pour scanner un site Web avec Nessus :
```css
nessus -p 80,443 --script http-vuln --host www.example.com
```
Dans cet exemple, `-p 80,443` spécifie les ports à scanner, `--script http-vuln` indique le script à utiliser pour tester les vulnérabilités HTTP, et `--host www.example.com` spécifie l'hôte à scanner.

Vous pouvez remplacer les paramètres de cette commande par d'autres valeurs en fonction de votre besoin. Par exemple, vous pouvez ajouter des scripts supplémentaires pour tester d'autres types de vulnérabilités, spécifier un autre hôte à scanner, ou modifier les options du script HTTP-VULN.

Pour plus d'informations sur la suite Nessus et les outils qu'elle contient, vous pouvez visiter le site Web de Rapid7, le créateur de Nessus : <https://www.rapid7.com/products/nessus/>

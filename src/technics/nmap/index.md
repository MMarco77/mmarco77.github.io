#  [NMAP](https://highon.coffee/blog/nmap-cheat-sheet/)

- Ping scans the network, listing machines that respond to ping

> nmap -sP \<IP\>[/\<MASK\>]
    
> nmap -sP xx.xx.xx.\<RANGE1\>-xx.xx.xx.\<RANGE1\>
    
> nmap -sP xx.xx.xx.*

- Scan UDP (BIEN LIMITER LES PORTS)

> nmap -sU 

- Utilisation de ligne de commande

> nmap -iL file.txt

file.txt
```txt
1.1.1.1/16
1.1.1.1-1.1.1.100
1.1.1.*
```
#### Command plus discret

1. Port `WellKnown`(1050 ports) => port officiel.
2. Reponses possible:
    1. `Open` le port est accessible et une application est à l'ecoute
    2. `filtered` nmap est pas capable de savoir si le port est ouvert. Paquet filtre, machine est derriere un pare-feu 
    3. `unfiltered` Port est accessibles mais nmap ne sait pas si il est ferme ou ouvert

- `TCP SYN scan` INTERDIT à cause du SYN/ACK/RST
le RST diminu le temps de requete ce qui est flagrant.

> nmap -Ss xx.xx.xx.xx

#### Recommandation

- TCP
`-n/-R` pas de DNS
`-P0, -Pn, -PD` pas de `Keeplive`. 
`-sV` scan versions (Banner Grabbling - bannière de service)
`-p 80,22,33` Scan un ou plusierus ports
`10 -> 1044` Scan un range de pors
`-p-` Scan les 65000 ports (pas que les `WellKnown`

- UDP
`-sU` All ports en UDP (depricated)
`-sU -p 0->100` certains ports en UDP (best practice)

- Output
`-O` OSF
`-vvv` Verbose
`-o [N|G|X] file_name` Sortie au format `N`ormal, `G`reppable ou `X`ML
`-o A file_name` Sortie pour toues les formats.

`-A` Scan Agressif [DEPRICATED]
`-sC` Scan sCript [DEPRICATED] script NSE
`-n` Pas de resolution DNS. Pas de demande d'enregistrement
`-T4` Temporisation => de 0(Paranoid) à 5 (Speed)
`-F` Fast Scan
`--top-ports` Les ports les plus conventionnels.

**note** `keeplive` est accompagné de l'adresse IP du demandeur => DEPRICATED.

**note** Comment être discret:
- Usurpe la MAC address
- Modifier l'IP
- Temporiser les rêquetes
- Splitter les packets

- Example: recherche de l'OS

> nmap -sV -n 192.168.94.1 -p- -oX pcl.xml

- Utilisation du port d'allocation dynamic (0)

> nmap 192.168.94.1 -p 0


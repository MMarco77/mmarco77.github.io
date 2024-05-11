# Bash tips

## touch file into sub unexisting folder

```shell
$ file="./nested/folder/deep/more.txt"
$ mkdir -p "${file%/*}" && touch "$file"
```

Where

```shell
$ file="./nested/folder/deep/more.txt"
$ echo "${file%/*}" # dirname ${file}
./nested/folder/deep
$ echo "$file"
```

Possible function: 

```shell
$ mktouch() {mkdir -p $(dirname $1) && touch $1;}
```

## Linux

- Root console
> sudo -s

- IP address
> ip a

- Route

> ip r

- History des commande
> history
> !<history_id>

- Process 
> ps -ef
> ps -faux

- Process sort by mem 
> ps -faux --sort %mem

- App setup
> apt list

- App available
> apt-cache search nessus

- List des repo
> sudo nano /etc/apt/sources.list

- Server HTTP de partage 
> python2.7 -m SimpleHTTPServer 8080
> python3 -m http.server 8080

- Shared data
> qr http://0.0.0.0:8080/



- Afficher/Comparer les fichiers

| commande | action |
| ---|---|
| `wc fichier`            | compte le nombre de lignes, de mots, d'octets de fichier
| `cat fichiers`          | concatène les fichiers
| `more fichier`          | affiche fichier page après page 'Espace'=page suivante, 'Entrée'=ligne suivante, 'u'=remonter
| `less fichier`          | affiche *fichier* avec une navigation au clavier
| `head -n x fichier`     | affiche les x premières lignes de fichier
| `tail -n x fichier`     | affiche les x dernières lignes de fichier
| `tail -f fichier`       | affiche la dernière ligne de fichier en temps réel
| `diff file1 file2`      | affiche les différences entre deux fichiers texte
| `diff -u file1 file2`   | affiche les différences au format patch
| `comp file1 file2`      | compare deux fichiers binaires
| `comp file1 file2 n N`  | compare deux fichiers, file1 à partir du nième octet, et *file2* à partir du **N**ième

- Réseau

| commande | action
|---|---|
| `hostname`                      | affiche le nom d'hôte de la machine
| `ping 'machine`'                | envoie un ping à une 'machine'
| `traceroute 'machine`'          | fait un traceroute vers 'machine'
| `netstat`                       | liste les processus utilisant le réseau
| `netstat -a`                    | netstat + affichage des processus serveurs
| `lsof`                          | liste détaillée de l'usage des fichiers et du réseau
| `ifconfig`                      | affiche la config des interfaces réseaux
| `ifconfig interface IP masque`  | configure une interface réseau
| `route`                         | affiche la table de routage
| `curl ifconfig.me`              | IP publique

- Recherche

| commande/option | action
|---|---|
| `locate motif`| recherche sur un nom correspond au motif
| `updatedb`| mettre à jour la base de données de locate
| `find chemin options`| recherche les fichiers dans chemin avec option
| `find -name motif`| recherche sur le nom du fichier
| `find -type f/d/l`| recherche par type où f=fichier,d=répertoire,l=lien
| `find -exec cmd`| exécute la commande cmd à tous les fichiers trouvés

**Exemple** : trouver toutes les images avec l'extension png dans le dossier 'Images' de l'utilisateur et les copier dans le dossier tmp ( '{}' représente les fichiers trouvés).

> find $HOME/Images -name "*.png" -exec cp {} $HOME/tmp/ \;

- Kernel

Version du noyau Linux utilisé, son nom, la version du compilateur utilisé :

```shell
# System info
$ cat /proc/version
# Version du kernel
$ uname -r
# liste les noyaux installés sur votre machine
$ dpkg -l | egrep "linux-(header|image)"
```

## Windows

- Carte reseau
> ncpa.cpl

- Gestion des users (Local User Manager)
> lusrmgr.msc

- Certificat manager 
> certmgr.msc

- Gestionnaire de FireWall
> WF.msc

- Gestionnaire de la sourie
> main.cpl

- Gestionnaire des taches
> tasklist.exe (console)
> taskmgr.exe (GUI)

- Applications
> appwiz.cpl

- List des IP
> ipconfig

- Bypass AV
> #bypass ASMI
> powershell -ep bypass

- History
=> Press F7
    
- Screen catpure
> Win+Shift+s
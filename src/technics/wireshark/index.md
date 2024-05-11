```
▌ ▌▗          ▌        ▌  
▌▖▌▄ ▙▀▖▞▀▖▞▀▘▛▀▖▝▀▖▙▀▖▌▗▘
▙▚▌▐ ▌  ▛▀ ▝▀▖▌ ▌▞▀▌▌  ▛▚ 
▘ ▘▀▘▘  ▝▀▘▀▀ ▘ ▘▝▀▘▘  ▘ ▘
```

## Filter

- DNS

| Protocole | Filter
|---|---|
| DNS     |dns|
| DNS   |dns.query.response == 0|
| DNS   |dns.query.response == 1|
| DNS   |dns.flags.rcode == 2 [Server Failure]|

-FTP

| Protocole | Filter | Descriptions
|---|---|---|
| FTP|"ftp.request.command == ""USER"" "|Ce filtre est utilisé pour filtrer les données en fonction d'une commande FTP spécifique(autres commandes FTP possibles)
| FTP |"ftp.request.arg == ""anonymous"" "|Affine les arguments passés aux commandes FTP
| FTP |ftp.response.code |Filtrage pour les codes spécifiques de réponse FTP, peut nous aider à identifier des problèmes spécifiques sur le réseau. Ex : si beaucoup de codes 530 dans le trafic FTP, il y a une forte chance pour que quelqu'un tente de casser les mots de passe.
| FTP |"ftp || ftp-data (command control and data transfer) "|Permet de voir le trafic FTP complet sur le câble, y compris les commandes et les données transférées

- HTTP

| Protocole | Filter
|---|---|
| HTTP|http|
| HTTP |http2|
| HTTP |http.set_cookie|
| HTTP |http.cookie|
| HTTP |http.request.method|
| HTTP |http.response.code >=300 and http.response.code <400 [Redirections]|
| HTTP |http.response.code >=400 and http.response.code <500 [Client-Side Errors]|
| HTTP |http.response.code >500 [Server-Side Errors]|
| HTTP |http.user_agent |

- Filtres basés sur des signatures et expressions uniques

| Filter | Descriptions
|---|---|
|"frame contains ""\x50\x4B\x03\x04"""|Signature unique pur les fichiers ZIP
|"frame matches ""(?i)(password|confidential|secret)"""|Localise des mots clés dans la trace
|"http matches ""[a-zA-Z0-9\-\.]+\.(?i)(com)"" "|Filtre sur tous les domaines .com du trafic HTTP
|"smtp matches ""[a-zA-Z0-9._%+-]+@[a-zA-Z0-9._%+-]"""|Permet de trouver un e-mail dans du trafic SMTP

- Filtres pour identifier énumération SMTP||

| Filter | Descriptions
|---|---|
|"smtp.req.command == ""VRFY"" || smtp.req.command == ""EXPN"""|
|"   smtp.req.command == ""RCPT"""|
|   smtp.response.code == 550|Indique Action demandée non effectuée: boîte aux lettres indisponible
|"   smtp.req.command == ""RSET"""|

-Filtres pour identifier les attaques SMTP||

| Filter | Descriptions
|---|---|
|smtp.response.code == 554 |La transaction a échoué
|smtp.response.code == 553 |Destinataire non valide
|"smtp matches ""[a-zA-Z0-9._%+-]+@nmap.scanme.org"" "|Filtre la signture Nmap tout en testant l'open relay

- Filtres permettant de trouver une erreur/problème dans les communications mail||

| Filter | Descriptions
|---|---|
|smtp.response.code >= 400|
|"   pop.response.indicator == ""-ERR"""|

- Filtres permettant de montrer les identifiants de connexion e-mail||

| Filter | Descriptions
|---|---|
|" pop.request.command == ""USER"" || pop.request.command == ""PASS"""|
|"   imap.request contains ""login"""|
|"   smtp.req.command == ""AUTH"""|

- Filtres importants||

| Filter | Descriptions
|---|---|
|http.host|
|http.request |
|bootp.option.hostname |Hôte via DHCP
|dns.qry.name |Hôte via DNS
|irc.request |Commande pour joindre IRC
|"irc && tcp matches ""(?i) join"" "|Commande de requête IRC

- Autres filtres||

| Filter | Descriptions
|---|---|
|wlan.wep.iv |Filtre sur le trafic chiffré WEP
|llc |Pour se débarrasser du trafic 802.11
|imf|Filtre les paquets Internet Message Format 
|http.host |
|http.request.uri |
|dns.count.answers>5 |
|http.response.code > 399|Erreurs HTTP
|dns.flags.rcode > 0|Erreurs DNS
|ftp.response.code > 399|Erreur FTP
|wlan.fr.retry == 1|Tentatives WLAN 
|ip.host == 10.1.0.20|Filtre sur une IP en particulier

- Filtres sur TCP||

| Filter | Descriptions
|---|---|
|tcp.analysis.lost_segment|Paquet précédent non capturé
|tcp.analysis.fast_retransmission|Retransmissions rapides TCP
|tcp.analysis.duplicate_ack|ACKs dupliqués
|tcp.analysis.retransmission|Retransmissions TCP
|tcp.analysis.out_of_order|Segments out-of-orders
|tcp.analysis.zero_window|Fenêtre à 0
|tcp.window_size_value == 0 |
|tcp.time_delta |Delta time TCP
|tcp.flags.syn==1 && tcp.flags.ack==0|Attaque SYN flood
# OSINT

[OSINT Framework](https://osintframework.com/)

## Tools

<style>
.badge_username {
  background-color: pink;
  color: black;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
  text: bold;
}
.badge_email {
  background-color: #3a87ad;
  color: black;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
}
.badge_ip_domain {
  background-color: purple;
  color: white;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
}
.badge_photo {
  background-color: yellow;
  color: black;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
}
.badge_pseudo {
  background-color: orange;
  color: black;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
}
.bold-red {
    color: red;
    font-weight: bold;
}
</style>
<table>
    <thead>
        <tr>
            <th>Tools</th>
            <th>Tags</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://github.com/sherlock-project/sherlock">Sherlock</a></td>
            <td>
                <span class="badge_username">Username</span>
            </td>
            <td>Scan les reseaux sociaux.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://github.com/megadose/holehe">Holehe</a></td>
            <td>
                <span class="badge_email">Email</span>
            </td>
            <td>Email check.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://www.shodan.io/">Shodan</a></td>
            <td>
                <span class="badge_ip_domain">Ip/Domain</span>
            </td>
            <td>Traque les objets connectes et leurs valeurs par defaut.</td>
        </tr>
        <!-- ITEM -->
        <tr>
            <td><a ref="https://www.whois.com/whois/osint.net">Whois</a></td>
            <td>
                <span class="badge_ip_domain">Ip/Domain</span>
            </td>
            <td>Carto. IP et Domaine.</td>
        </tr>
        <!-- ITEM -->
        <tr>
            <td><a ref="https://github.com/laramies/theHarvester">the Harverter</a></td>
            <td>
                <span class="badge_ip_domain">Ip/Domain</span>
                <span class="badge_username">Unsername</span>
            </td>
            <td>The harvester is another OSINT tool for reconnaissance.</td>
        </tr>
        <!-- ITEM -->
        <tr>
            <td><a ref="https://pimeyes.com/en">PimEyes</a></td>
            <td>
                <span class="badge_photo">Photo</span>
            </td>
            <td>Outil de recherche pour la reconnaissance faciale.</td>
        </tr>
        <!-- ITEM -->
        <tr>
            <td><a ref="https://betaface.com/demo.html">Betaface</a></td>
            <td>
                <span class="badge_photo">Photo</span>
            </td>
            <td>Outil de recherche pour la reconnaissance faciale.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://www.google.com">Google</a></br><a ref="https://yandex.com/">Yandex</a> <span class="bold-red">[UK]</span></br><a ref="https://www.bing.com/">Bings</a></td>
            <td>
                <span class="badge_username">Username</span>
                <span class="badge_email">Email</span>
                <span class="badge_ip_domain">Ip/Domain</span>
                <span class="badge_photo">Photo</span>
                <span class="badge_pseudo">Pseudo</span>
            </td>
            <td>Most populaire search gear.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://whatsmyname.app/">WhatsMyName</a></td>
            <td>
                <span class="badge_username">Username</span>
                <span class="badge_pseudo">Pseudo</span>
            </td>
            <td>Enumerate username.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://github.com/snooppr/snoop">Snoop <span class="bold-red">[UK]</span></a></td>
            <td>
                <span class="badge_username">Username</span>
                <span class="badge_pseudo">Pseudo</span>
            </td>
            <td>Check username and pseudo.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://github.com/smicallef/spiderfoot">Spiderfoot <span class="bold-red">[UK]</span></a></td>
            <td>
                <span class="badge_username">Username</span>
                <span class="badge_pseudo">Pseudo</span>
            </td>
            <td>Check username and pseudo.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://github.com/mxrch/GHunt">GHunt</a></td>
            <td>
                <span class="badge_email">Email</span>
            </td>
            <td>Extraire les infos. d'un compte Google.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://epieos.com/">Epieos</a></td>
            <td>
                <span class="badge_email">Email</span>
            </td>
            <td>Extraire les infos. d'un compte Google.</td>
        </tr>
        <!-- ITEM --> 
        <tr>
            <td><a ref="https://epieos.com/">Epieos</a></td>
            <td>
                <span class="badge_email">Email</span>
            </td>
            <td>Extraire les infos. d'un compte Google.</td>
        </tr>
    </tbody>
</table>


## Reconnaissance

### Site de recherche

#### People Search


- https://generated.photos
- https://fr.fakenamegenerator.com
- https://fr.fakepersongenerator.com
- https://this-person-does-not-exist.com/fr

#### Google Dorks

Pour les `time:`, elle doit etre la valeur dans le calendrier Julien OU contenir explicitement la chaine de caractere.
Tous les operandes fonctionnent: &, | <, > 
`index of ...` => "index of "

[GHDB (Google Hacking DataBase)](https://www.exploit-db.com/google-hacking-database) 
[Google Social Search](https://www.social-searcher.com/google-social-search/)

CSE exploité par Google (Google Custom Search Engine (CSE))

[CSE Utopia](https://start.me/p/EL84Km/cse-utopia?locale=fr) => list de CSE pre-generé

Generateur de DORKS => https://www.google.com/advanced_search

#### Recherche d'information par images

[yandex](https://yandex.com/images)

[OSINT Framework](https://osintfr.com/fr/accueil/)
[lampire](https://lampyre.io/)

[Address IP](https://whoer.net/fr)

## Training

[OSINT FR](https://osintfr.com)
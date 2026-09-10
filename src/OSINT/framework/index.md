# Guide de Navigation : OSINT Framework

L'**OSINT Framework** est une ressource indispensable qui cartographie des centaines d'outils de recherche d'informations en sources ouvertes. Plutôt qu'une simple liste, il s'agit d'un arbre de décision visuel.

**Lien vers l'outil officiel :** [https://osintframework.com](https://osintframework.com)

---

## 🧠 Carte Mentale de Recherche (Par Intention)

Le framework est organisé par types de données. Voici comment orienter vos recherches selon votre objectif :

### 1. Identité & Profils Sociaux (People)
*Si vous avez un nom, un pseudo ou une photo de profil.*
- **Social Media :** Pour trouver des comptes sur les plateformes (Facebook, Twitter, LinkedIn, etc.).
- **Username :** Pour voir si un pseudo est utilisé sur différents sites.
- **Public Records :** Pour les registres civils, judiciaires ou de propriété.

### 2. Infrastructure & Réseau (Technical/Network)
*Si vous avez une adresse IP, un domaine ou un nom de serveur.*
- **IP Address :** Géolocalisation, ASN, et corrélation de serveurs.
- **Domain/Hostname :** WHOIS, sous-domaines, enregistrements DNS.
- **Email Address :** Vérification de l'existence et corrélation avec des fuites de données (Data Breaches).

### 3. Contenu de Fichiers & Métadonnées (Files/Metadata)
*Si vous analysez un document (image, PDF, etc.).*
- **Images :** Recherche inversée, analyse des tags EXIF (coordonnées GPS, modèle d'appareil).
- **Documents :** Extraction des métadonnées cachées (auteur, logiciel utilisé, date de création).

### 4. Géolocalisation (Maps/Locations)
*Si vous avez une image de rue ou une coordonnée.*
- **Maps/Satellite :** Corrélation entre visuels et coordonnées réelles.

---

## 🛠️ Méthodologie de travail recommandée

Pour tirer le meilleur parti du framework, suivez ce cycle :

1. **Pivotement (Pivoting) :** Ne restez pas sur un seul outil. Si un outil vous donne un nom d'utilisateur, utilisez-le comme point de départ dans la branche "Username".
2. **Corrélation :** Croisez toujours les informations provenant de deux sources différentes pour valer une hypothèse (ex: une adresse email trouvée sur un leak et un profil social).
3. **Vérification de l'intégrité :** Certains outils de l'arbre peuvent être obsolètes ou payants. Utilisez le framework comme une boussole, pas comme une vérité absolue.

## 💡 Astuce de Pro
Le framework est une structure de **recherche de branches**. Gardez toujours l'onglet de l'OSINT Framework ouvert avec la branche que vous explorez pour naviguer rapidement entre les outils de même catégorie.
[/index.md]
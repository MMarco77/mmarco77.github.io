# Maîtriser Frida : Écrire votre premier script

Le "hooking" est l'art d'intercepter l'exécution d'une fonction pour observer son comportement ou le modifier en temps réel. Ce guide vous apprendra à passer de la simple lecture de snippets à la rédaction de votre propre logique.

## Le concept central : Le moteur JavaScript

Frida fonctionne en injectant un moteur JavaScript dans le processus cible. Lorsque vous exécutez un script, Frida :
1. Injecte le moteur.
2. Fournit un pont entre le moteur JS et le runtime natif/Java.
3. Vous permet de "hooker" des points spécifiques d'exécution.

## 1. Le wrapper fondamental : `Java.perform`

Lorsque vous travaillez avec Android, tout doit se dérouler à l'intérieur du bloc `Java.perform`. Cela garantit que votre script s'exécute lorsque le moteur JavaScript est pleinement intégré au runtime Android et que la sécurité des threads est respectée.

```javascript
Java.perform(function() {
    console.log("[*] Script injecté avec succès !");
    // Votre logique va ici
});
```

## 2. L'anatomie d'un hook

Pour hooker une méthode, vous suivez un schéma en trois étapes : **Identifier -> Intercepter -> Reprendre**.

### Étape A : Identifier la classe et la méthode
Vous avez besoin du nom complet et qualifié de la classe (ex: `com.example.app.UserSession`) et de la signature exacte de la méthode.

### Étape B : Intercepter la méthode
Nous utilisons `Java.use()` pour obtenir un proxy de la classe, puis `.implementation` pour remplacer le code original.

### Étape C : Reprendre/Modifier
À l'intérieur de l'implémentation, vous pouvez accéder aux arguments, appeler la méthode originale, ou retourner une valeur personnalisée.

---

## 3. Exemple pratique : Intercepter une fonction de Login

Imaginons que nous voulons intercepter une méthode qui valide un mot de passe.

**Méthode cible :**
- **Classe :** `com.example.app.AuthManager`
- **Méthode :** `boolean checkPassword(String password)`

### Le Script

```javascript
Java.perform(function() {
    // 1. Récupérer un proxy de la classe cible
    var AuthManager = Java.use("com.example.app.AuthManager");

    // 2. Remplacer l'implémentation
    AuthManager.checkPassword.implementation = function(password) {
        console.log("[!] Interception de checkPassword !");
        console.log("[+] Mot de passe fourni : " + password);

        // 3. Logique : Tentons de contourner la vérification en retournant toujours 'true'
        var result = true; 
        
        // Alternativement, vous pouvez appeler la méthode originale pour garder un comportement normal :
        // var result = this.check-password(password);

        console.log("[+] Retourne : " + result);
        return result;
    };
});
```

## 4. Gérer les surcharges (Crucial)

En Java, une classe peut avoir plusieurs méthodes portant le même nom mais avec des paramètres différents. C'est ce qu'on appelle la **surcharge** (overloading). Si vous essayez de hooker une méthode sans préciser sa signature exacte, Frida renverra une erreur.

Pour gérer cela, utilisez `.overload()` :

```javascript
// Si checkPassword possède :
// 1. checkPassword(String password)
// 2. checkPassword(String password, int attempts)

// Utilisez ceci pour la première version :
AuthManager.checkPassword.overload('java.lang.String').implementation = function(password) {
    console.log("Version String capturée");
    this.checkPassword(password); // Appel de l'originale
};

// Utilisez ceci pour la deuxième version :
AuthManager.checkPassword.overload('java.lang.String', 'int').implementation = function(password, attempts) {
    console.log("Version String + int capturée");
    this.checkPassword(password, attempts); // Appel de l'originale
};
```

*Note : Utilisez toujours le nom complet du type Java, ex: `java.lang.String`, `int`, `boolean`, `java.lang.Integer`.*

## 5. Checklist de synthèse

| Tâche | Commande / Syntaxe Frida |
| :--- | :--- |
| Encapsuler dans le contexte Android | `Java.perform(function() { ... });` |
| Récupérer un proxy de classe | `var MyClass = Java.use("package.Name");` |
| Remplacer une méthode | `MyClass.methodName.implementation = function(...) { ... };` |
| Appeler la méthode originale | `this.methodName(...args);` |
| Gérer les surcharges | `.overload('type1', 'type2')` |
[/index.md]
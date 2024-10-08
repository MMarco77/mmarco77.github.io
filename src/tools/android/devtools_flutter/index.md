# Devtool FLUTTER

Dans le cadre d'un APK compilé **DEBUG**, il est possible d'acceder au code source en ligne : 

1- installation de l’apk sur l’appareil android :

```shell
$ adb install path/to/apk
```

2- lancement de l’apk sur l’appareil

3- affichez les logs pour trouver l’observatory :
 
```shell
$ adb logcat | grep flutter
```

exemple :

    07-16 18:24:28.887 30621 30691 I flutter : Observatory listening on http://127.0.0.1:37405/XXXXXXX=/

4- copiez le port de l’obsevatory, dans l’exemple c’est 37405

5- configurez la redirection de port sur la machine :

```shell
$ adb forward tcp:37405 tcp:37405
```

(remplacez 37405 par votre port)

5- lancez le devtool avec :

```shell
$ dart devtools
```

et ajoutez le lien de l’observatory dans l’interface.

Il est possible d’acceder au code source depuis cette interface.
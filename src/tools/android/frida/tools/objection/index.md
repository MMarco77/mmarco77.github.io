```
                                                                      
   ▄▄▄▄    ▄▄           ██                                      ██    
  ██▀▀██   ██           ▀▀                           ██         ▀▀    
 ██    ██  ██▄███▄    ████      ▄████▄    ▄█████▄  ███████    ████      ▄████▄   ██▄████▄ 
 ██    ██  ██▀  ▀██     ██     ██▄▄▄▄██  ██▀    ▀    ██         ██     ██▀  ▀██  ██▀   ██ 
 ██    ██  ██    ██     ██     ██▀▀▀▀▀▀  ██          ██         ██     ██    ██  ██    ██ 
  ██▄▄██   ███▄▄██▀     ██     ▀██▄▄▄▄█  ▀██▄▄▄▄█    ██▄▄▄   ▄▄▄██▄▄▄  ▀██▄▄██▀  ██    ██ 
   ▀▀▀▀    ▀▀ ▀▀▀       ██       ▀▀▀▀▀     ▀▀▀▀▀      ▀▀▀▀   ▀▀▀▀▀▀▀▀    ▀▀▀▀    ▀▀    ▀▀ 
                     ████▀                                            
                                                                      

 ▞   ▌   ▖      ▐ ▝▖  ▗     ▖      ▐    ▞▗       ▝▖ 
▐ ▞▀▖▛▀▖▗▖▞▀▖▞▀▖▜▀ ▐  ▄ ▛▀▖▗▖▞▀▖▞▀▖▜▀  ▐ ▄ ▞▀▖▛▀▖ ▐ 
▝▖▌ ▌▌ ▌ ▌▛▀ ▌ ▖▐ ▖▞  ▐ ▌ ▌ ▌▛▀ ▌ ▖▐ ▖ ▝▖▐ ▌ ▌▌ ▌ ▞ 
 ▝▝▀ ▀▀ ▄▘▝▀▘▝▀  ▀▝   ▀▘▘ ▘▄▘▝▀▘▝▀  ▀   ▝▀▘▝▀ ▘ ▘▝  
```

```shell
objection -g owasp.sat.agoat explore
Checking for a newer version of objection...
Using USB device `Galaxy S10`
Agent injected and responds ok!

     _   _         _   _
 ___| |_|_|___ ___| |_|_|___ ___
| . | . | | -_|  _|  _| | . |   |
|___|___| |___|___|_| |_|___|_|_|
      |___|(object)inject(ion) v1.11.0

     Runtime Mobile Exploration
        by: @leonjza from @sensepost

[tab] for command suggestions
owasp.sat.agoat on (unknown: 13) [usb] # $env
Unknown or ambiguous command: `$env`. Try `help $env`.
owasp.sat.agoat on (unknown: 13) [usb] # env

Name                    Path
----------------------  --------------------------------------------------------------------------------------
cacheDirectory          /data/user/0/owasp.sat.agoat/cache
codeCacheDirectory      /data/user/0/owasp.sat.agoat/code_cache
externalCacheDirectory  /storage/emulated/0/Android/data/owasp.sat.agoat/cache
filesDirectory          /data/user/0/owasp.sat.agoat/files
obbDir                  /storage/emulated/0/Android/obb/owasp.sat.agoat
packageCodePath         /data/app/~~wSbNTKhW9lO7xzLnpx6vnw==/owasp.sat.agoat-zHmm--jVcgRWJT5jsMT_WQ==/base.apk

owasp.sat.agoat on (unknown: 13) [usb] # android hooking set return_value owasp.sat.agoat.RootDetectionActivity.isRooted false
(agent) Attempting to modify return value for class owasp.sat.agoat.RootDetectionActivity and method isRooted.
```

Petite commande intérresante:

Recherche de methodes de classe pour un module specifique (owasp.sat.agoat) et activité précisee (EmulatorDetectionActivity)

```shell
owasp.sat.agoat on (unknown: 13) [usb] # android hooking list activities
owasp.sat.agoat on (unknown: 13) [usb] # android hooking list classes owasp.sat.agoat.EmulatorDetectionActivity
owasp.sat.agoat on (unknown: 13) [usb] # android hooking list class_methods owasp.sat.agoat.EmulatorDetectionActivity
```
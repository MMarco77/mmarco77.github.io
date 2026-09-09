## Driver NVidia

EN cas de problème de chargement avec le kernel linux et les drivers NVidia, voici comment forcer l'installation des dirvers

- Recherche du system

```shell
$ uname -r # Linux kernel version
$ sudo dkms status # Check if driver in installed with `uname -r`
````

- Si par installé

```shell
$ sudo apt install nvidia-driver
$ sudo apt install linux-headers-$(uname -r)
$ sudo apt dkms autoinstall -k $(uname -r)
$ sudo update -initramfs -u -k $(uname -r)
``` 

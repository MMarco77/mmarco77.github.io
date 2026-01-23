# Crypti

## Encryption avec eCryptfs

### Mount

```shell
$> sudo mount -t ecryptfs /home/htotof/.Private /home/htotof/Private
```

```text
Key type: passphrase 
Cypher: aes [1]
Key bytes: 32 [2] 
Enable plaintext passthrough (y/n) [n]: n
Enable filename encryption (y/n) [n]: y
Filename Encryption Key (FNEK) Signature [a0bff6831f049d9c]: y
```

```text
ecryptfs_unlink_sigs
ecryptfs_fnek_sig=a0bff6831f049d9c
ecryptfs_key_bytes=32
ecryptfs_cipher=aes
ecryptfs_sig=a0bff6831f049d9c
```

### Umount

```shell
$> sudo umount .Private
```

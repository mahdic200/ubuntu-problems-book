# Generate a SSH key

```shell
ssh-keygen -f $FILE_FULL_PATH -t [-t dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa]
```

Then you will be prompted for passphrase, enter the passphrase and then confirm it, now your ssh_key is ready ! .

# Examples

## Custom Name

```shell
$ ssh-keygen -f /home/user/myNewPassphrase -t ed25519
Generating public/private ed25519 key pair.
Enter passphrase for "myNewPassphrase" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/myNewPassphrase
Your public key has been saved in /home/user/.ssh/myNewPassphrase.pub
The key fingerprint is:
SHA256:KqJFfR1QNInRU5TQvW+Aue9V2FHjix2nhiPsyugwdF0 user@hostname
The key's randomart image is:
+--[ED25519 256]--+
|      o*=*oo   ..|
|      ..+.o . . o|
|        ..Eo . +.|
|   .   o +o o.o+=|
|  . o o S o.o++oo|
| . . o . ... oo. |
|  o + .   .. ..  |
| o . + o .  ..   |
|.    .o o  ..    |
+----[SHA256]-----+
```

Now my passphrase is ready to use .

## Alternative Option for Naming

```shell
$ ssh-keygen -f /home/user/.ssh/id_ed25519_mypassphrase
Generating public/private ed25519 key pair.
Enter passphrase for "myNewPassphrase" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/myNewPassphrase
Your public key has been saved in /home/user/.ssh/myNewPassphrase.pub
The key fingerprint is:
SHA256:KqJFfR1QNInRU5TQvW+Aue9V2FHjix2nhiPsyugwdF0 user@hostname
The key's randomart image is:
+--[ED25519 256]--+
|      o*=*oo   ..|
|      ..+.o . . o|
|        ..Eo . +.|
|   .   o +o o.o+=|
|  . o o S o.o++oo|
| . . o . ... oo. |
|  o + .   .. ..  |
| o . + o .  ..   |
|.    .o o  ..    |
+----[SHA256]-----+
```

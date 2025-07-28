# Checking Passphrases

```shell
ssh-keygen -y -f $FILE_PATH
```

# Examples

```shell
$ ssh-keygen -y -f /home/user/.ssh/id_ed25519
Enter passphrase for "id_ed25519": 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIO8gpRR1/BnAEzILflHPjyAP+HvSpOcRyih9iWkkpJFu user@hostname
```
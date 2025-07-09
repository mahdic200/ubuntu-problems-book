# SSH Passphrase For Logging in

After you generated a new ssh key file (private and public key), now it's time to use it instead of password authentication on your remote server .

```shell
ssh-copy-id [-i identity_file] [-p port] [user@]hostname 
```

# Examples

For example, let's assume I've created a new ssh-key named `id_ed25519foo`, now I want to use it as password instead of password authentication .

```shell
$ ssh-copy-id -i /home/user/.ssh/id_ed25519 -p 3592 user@104.234.196.99
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
user@104.234.196.99's password: 

Number of key(s) added: 1

Now try logging into the machine, with: "ssh -i /home/mahdi/.ssh/id_ed25519 -p 3592 'user@104.234.196.99'"
and check to make sure that only the key(s) you wanted were added.
```

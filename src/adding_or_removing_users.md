# adding or removing users

```bash
sudo adduser <username>
```

## making it a sudoer

```bash
usermod -aG sudo <username>
```

verifying operation :

```bash
groups <username>
```

# removing a user

if you wanna completely remove that poor user :

```bash
sudo deluser --remove-home <username>
```

if you wanna keep files :

```bash
sudo deluser <username>
```

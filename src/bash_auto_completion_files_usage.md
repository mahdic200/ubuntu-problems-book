# bash auto completion usage

when you have an auto completion file and you want to use it , you have to put the file in the `/usr/share/bash-completion/completions/` folder .

then you have to edit the `~/.bashrc` file and add these line in the end of your file . for example I have a `mdbook` bash auto completion file and I want to use it , first I put the file in the `/usr/share/bash-completion/completions` folder and then I come and edit the `.bashrc` file .

```bashrc
// snip

# mdbook auto completion file loading
source /usr/share/bash-completion/completions/mdbook
```

then save file and exit terminal . now enter this command in the shell .

```bash
. ~/.profile
```


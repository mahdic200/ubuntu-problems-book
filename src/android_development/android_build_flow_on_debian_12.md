# Android build flow on debian 12 (bookworm)

# You need a healthy VPN

Make sure you have made a VPN that works and it's not banned from google services , you can achieve that by trying to download a file from `dl.google.com` or `maven repository` . this is super important .

after that you must build a vpn which I have provided necessary information for that in this book.

# Install qv2ray on debian

I also have noted necessary information for this topic in this book .

# CLI proxy in debian

When you start qv2ray , you must reopen all your terminal sessions and then verify that proxy is applied to your sessions with this command :

```bash
env | grep proxy
```

if something like this comes up , it means you are all set :

```bash
no_proxy=
ftp_proxy=http://127.0.0.1:8889/
https_proxy=http://127.0.0.1:8889/
http_proxy=http://127.0.0.1:8889/
all_proxy=socks://127.0.0.1:1089/
```

# Install Android Studio

This is MOST pain in the ass section of this topic for one who doesn't know how to do it .
first of all , verify that your vpn is working and it's connected . then open firefox or any web browser . search for `Android Studio` . navigate to the download section and download it for Linux .

After downloading the tarball , extract it in the `$HOME` directory , which will be `/home/$USER/` and you will have something like this `/home/$USER/AndroidStudio` . please don't change it , please .

> [!WARNING]
> Replace the $USER with the name you have in /home directory . for example /home/jack . if you don't it won't work and will be some waste of time and energy .

## Configurations After Extracting tarball

Now, for the very first time and only time , you must execute `studio.sh` in `/home/$USER/AndroidStudio/bin/studio.sh` . this will make necessary config files for you in your `~/.config` directory .

After you executed that IN THE TERMINAL SESSION WITH ACTIVE VPN , the AndroidStudio is able to download it's necessary files and configure your application .

> [!TIP]
> Program must fail on downloading Gradle , it's normal , close the program and do the rest , I'll tell you what to do .

The real pain in the ass is `Gradle` , gradle doesn't respect to your terminal proxy environment variables and you must tell it explicitly to use which network .

## Gradle Configurations

You have to navigate to the folder I tell you now , open the file manager which is `nautilus` because this instruction is dedicated to Debian 12 (bookworm) . press `Ctrl + L` this will focus on the top path `url` and you will be able to write your desired path instead of clicking on it . why ? am I dumb ? why just don't click the hell on which folder we want to navigate right ? no :) . I want to take you to `/home/$USER/.gradle` folder which is a hidden folder .

So press `Ctrl + L` and type `/home/$USER/.gradle` . you will navigate to that folder , there must be a file named `gradle.properties` . open it with the desired text editor, I will use `gedit`, it's light and coazy . and add these in the end of the file :

```
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=8889
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=8889
```

Why `127.0.0.1` ? because I'm using proxy on my machine and not an external proxy . I'm using qv2ray as you MUST :) .

> [!WARNING]
> Change the 8889 with your actual port, you can check the port you are using by going to the settings > network > Network Proxy . else it won't work and again waste of time  and energy .

> [!TIP]
> Save and exit .


## Continue of Android Studio Configurations after Gradle Proxy settings

Now it's time to get real job done . MAKE SURE VPN IS ON , open a terminal and type, it shouldn't have `.sh` it must be `studio` , it's ok :

```bash
~/AndroidStudio/bin/studio
```

Now continue the android studio's setup . now everything is OK . and we are all set .
## .bashrc / environment configuration

Open the `~/.bashrc` file and add the following directives :

```bash
export JAVA_HOME=$HOME/AndroidStudio/jbr
export ANDROID_HOME=$HOME/Android/Sdk
# this line is for when you are using just one ndk and not multiple ndk versions
#export ANDROID_NDK_HOME="$ANDROID_HOME/ndk/$(ls -1 $ANDROID_HOME/ndk)"
export ANDROID_NDK_HOME="$ANDROID_HOME/ndk/27.0.12077973"
export PATH=$PATH:$ANDROID_HOME
export PATH=$PATH:$ANDROID_NDK_HOME
export PATH=$HOME/AndroidStudio/bin:$PATH
```

> [!WARNING]
> This is for the case you have multiple ndk files which is very common . if you are using one ndk file and you are confident about that (which I'm confident you will not :) ) you may comment this line `export ANDROID_NDK_HOME="$ANDROID_HOME/ndk/27.0.12077973"` and uncomment the upper line which is `export ANDROID_NDK_HOME="$ANDROID_HOME/ndk/$(ls -1 $ANDROID_HOME/ndk)"` .
>
> Else Change the 27.0.12077973 with the version which is in ~/Android/Sdk/ndk/ , don't panic, there will be just folders with some odd names (they all are numbers) which they are the names of different ndk versions . so just pick one 😂 .

> [!TIP]
> Save and exit

# Trouble Shooting In Making Android Projects Using Android Studio

Now it's time to make a new project and test it to see if it works . make a new project with your desired layout and activity bar . select minimum SDK version (it should be better 24) and select kotlin instead of groovy . wait until android studio downloads all necessary dependencies and then try to build :) . with the Gradle proxy setup there mustn't be any issues . just make sure VPN works .

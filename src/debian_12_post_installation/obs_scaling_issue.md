# OBS Studio scaling issue

UI is too small and everything is tiny .
Find OBS Studio's `.desktop` file in `/usr/share/applications/` and then open it with some editor , then add this env var for scaling :

you may want to execute this command :
```shell
find /usr/share/applications/ -name *obs*
```

```shell
code /usr/share/applications/com.obsproject.Studio.desktop
```

and then change this line `Exec=obs` to this line `Exec=QT_SCALE_FACTOR=1.5 obs`

Your welcome !

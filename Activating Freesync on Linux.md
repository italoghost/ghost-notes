## EDIT 1: 

Apparently, the Xorg reads the default config on the folder `/usr/share/X11/xorg.conf.d`, meanwhile the folder `/etc/X11/xorg.conf.d` is used only for user customization. Since the configuration inside the `etc` folder is not working for me, I changed it directly in `/usr/share/X11/xorg.conf.d`. Anyway, the tutorial below works, you just need to change the folder.

Useful links:

- [/etc/X11/xorg.conf.d missing?](https://unix.stackexchange.com/questions/176870/etc-x11-xorg-conf-d-missing)
- [/usr/share/X11/xorg.conf.d/ instead of /etc/X11/xorg.conf.d/](https://bbs.archlinux.org/viewtopic.php?id=198719)
- [/etc/X11/xorg.conf doesn't exist? [duplicate]](https://askubuntu.com/questions/25746/etc-x11-xorg-conf-doesnt-exist)
- [There is no xorg.conf. How does ubuntu store user configurations of screen resolution?](https://askubuntu.com/questions/1122374/there-is-no-xorg-conf-how-does-ubuntu-store-user-configurations-of-screen-resol)

---

# Activating Freesync

To enable Freesync on Pop!_OS with Xorg you must first configure the following file:

`/etc/X11/xorg.conf.d/20-amdgpu.conf`

It probably won't exist, so it will be necessary to create it:

`sudo nano etc/X11/xorg.conf.d/20-amdgpu.conf`

Once created, you need to enter the following parameters:

```
Section "Device"
     Identifier "AMD"
     Driver "amdgpu"
EndSection`
```

Below these parameters we can enter extra settings, such as Freesync:

```
Section "Device"
	Identifier "AMD"
	Driver "amdgpu"
	Option "TearFree" "false" #adds triple buffering, so set it to false
	Option "DRI" "3" #DRI 3 is required for Xorg > 1.18
	Option "VariableRefresh" "true" #activate Freesync
EndSection
```

Finally, just save by clicking Ctrl + X and Y + Enter.

---

## Extras

 - To find out the version of Xorg:

`sudo X -version`

- View the settings of the monitors:

`xrandr --props`

- To disable TearFree (triple buffering) in the current session:

`xrandr --output DisplayPort-2 --set TearFree off`

The "DisplayPort-2" parameter is my monitor's connection. You need to change it for your specific monitor.

If you want to enable it again, or leave it on automatic, just change "off" to "on" or "auto".

- To test if Freesync is working:

There are a couple of options. We can check in the Xorg logs. By default, the logs are located in the `/var/log/Xorg.N.log` folder, `N` being a number, which can be 0, 1, etc. If Xorg is started as a user (startx), the logs are in the following folder: `~/.local/share/xorg/Xorg.N.log`

In this way, you can see using the following codes:

```
grep VariableRefresh /var/log/Xorg.N.log
```

or 

```
grep VariableRefresh /home/<your_home_directory>/.local/share/xorg/Xorg.1.log
```

*Remember to replace the `N` with the correct number*.

So if Freesync is enabled, the output will look like this:

```
[    17.462] (**) AMDGPU(0): Option "VariableRefresh" "true"
[    17.505] (**) AMDGPU(0): VariableRefresh: enabled
```

There is also a simple program that does this test on linux:

https://github.com/Nixola/VRRTest

Just download the AppImage, enable it to run on properties, and test. Full information is on the project's GitHub.

- Useful links:

1) https://wiki.archlinux.org/title/AMDGPU#Xorg_configuration
2) https://wiki.archlinux.org/title/Variable_refresh_rate
3) https://forums.linuxmint.com/viewtopic.php?t=347244
4) https://linuxreviews.org/HOWTO_enable_Adaptive_Vertical_Sync_(Freesync)_on_AMD_GPUs

# How to play the Epic Games version on Linux without the Epic Games Launcher

## Linking accounts

The Epic Games version of this game is not installed or downloaded from Epic itself. Instead, it is linked to Origin, where you have to install and download it. 

On Linux, linking your Epic account with your Origin account can be easely done through [*legendary*](https://github.com/derrod/legendary), with the `-O` argument. 

Another method is to have both the Epic Games launcher and the Origin launcher installed on the same prefix. Trying to install the game through Epic will auto-start Origin, where you will be able to download and play it.
I do not recommend this method because it creates unnecessary steps and requires both launchers to be opened at the same time. But if you want to use this method anyway, there is already a [Lutris script for it](https://lutris.net/games/install/27729/view).   

## Installing

Regardless if you are using *legendary*, Lutris or installing manually, you are going to have Origin installed on the prefix, as the game will be downloaded and launched through there. I recommend the [Lutris's Origin script](https://lutris.net/games/origin/), but you are free to install it manually. 

Open Origin and install the game.

## Playing

We are going to use the commands that are stated on the "Old Method" session of the [*legendary's Battlefront II Wiki page*](https://github.com/derrod/legendary/wiki/Star-Wars:-Battlefront-II).

1) If you have installed the game manually on a prefix, you can launch the game with the following code from the Terminal:

> WINEDLLOVERRIDES="nvapi, nvapi64=d" WINEPREFIX="<prefix_where_origin_is_installed>" wine64 "<prefix_where_origin_is_installed>/drive_c/Program Files (x86)/Origin/EALink.exe" "link2ea://launchgame/MtMassive?AUTH_PASSWORD=0&AUTH_TYPE=exchangecode&epicusername=myusername&epicuserid=myepicuserid&epiclocale=en&theme=sws&platform=epic&Hotfix=go"

Where: 

- The env `WINEDLLOVERRIDES="nvapi, nvapi64=d"` is useful if the game fails to launch with message "outdated nvidia drivers..." (in case you are using NVIDIA);
- `WINEPREFIX="<prefix_where_origin_is_installed>" wine64` specifies that the prefix should be 64 bits;
- `"<prefix_where_origin_is_installed>/drive_c/Program Files (x86)/Origin/EALink.exe"` specifies that the executable should be the "EALink.exe", and;
- `"link2ea://launchgame/MtMassive?AUTH_PASSWORD=0&AUTH_TYPE=exchangecode&epicusername=myusername&epicuserid=myepicuserid&epiclocale=en&theme=sws&platform=epic&Hotfix=go"` is the command from the *legendary*'s wiki responsable for launching the game directly from Origin.

2) If you are using Lutris, you should already have a prefix with both Origin and the game installed. 

Click on the plus icon ("+") on the top left corner, select "Add a localy installed game" and fill the game's name, year and runner (it must be Wine).

Go to the "Game options" tab and click on "Executable". Select the "EALink.exe" from the Origin folder (it should be on `"<prefix_where_origin_is_installed>/drive_c/Program Files (x86)/Origin/EALink.exe"`).
On "Arguments", copy and paste the following command:

> link2ea://launchgame/MtMassive?AUTH_PASSWORD=0&AUTH_TYPE=exchangecode&epicusername=myusername&epicuserid=myepicuserid&epiclocale=en&theme=sws&platform=epic&Hotfix=go

On the "Runner Options" tab, go to "DLL overrides" and add the following keys and value:

|Key | Value |
|----|-------|
|nvapi,nvapi64| disabled|

Finally, on the "System Options" tab, go to the "Environment variables" and add the following keys and values:

|Key | Value |
|----|-------|
|STAGING_SHARED_MEMORY| 0|
|__GL_SHADER_DISK_CACHE| 1|
|__GL_SHADER_DISK_CACHE_PATH| <prefix_where_origin_is_installed>|

Now the game should already be able to run without the need for installing Epic Games Launcher on your prefix!

---

Useful links:

- [Installing Star Wars: Battlefront II with Legendary](https://gist.github.com/derrod/333fb5218002347435b7f31d532cbd01) (special thanks to the @Hadrianneue user) 

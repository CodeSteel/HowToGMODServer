# How to Manage a Garry's Mod Server

Welcome to "How to Manage a Garry's Mod Server", here I will try to explain the process of managing a Garry's Mod server properly with the aim of productivity and scale over time. I am sick of seeing populated servers with horrible development practices and even more so, tired of working for them, so read this guide and fix your shit.

## The Explanation

### 1. Github
You need Github. Plain and simple. You will want to use Github for two main reasons, although there are many many more reasons why Github should be used.
1. Github allows you to store a backup for every single update and every single line of code you change. This means, if you fuck shit up, you can easily revert back.
2. Git's source conrol allows working with future developers, or any development you and your team contributes, much easier to manage and deploy as you can have mulitple people working on the same project at a time.

If you plan to hire a developer for any commission work, a Github repository is a MUST. **DO NOT** hire anyone for commission work without this step.

### 2. Local Dedicated Server
You will want to set up a local dedicated server, this will be your dev server. Any future work YOU do should be done through this server. **DO NOT** use FTP to update your code, that is very bad practice. 

Hosting the server yourself is free and allows you to develop much faster.

It is also what we will be using to store your local github repository branch so you can make updates to your live server without ever touching or modifying any of the files, only your local files.

### 3. DeployHQ
DeployHQ makes it easy to deploy your github repository to the live server. What this means is any updates you or your team makes can be pushed to the live server with a click of a button.
This allows us to take the changes you've made in your local server and send them to the live server to be updated. DeployHQ is free, but limited to 5 deploys a day. As long as you have a decent update routine, this won't be an issue.

## The Setup

### 1. Github
1. Start by creating a Github account if you don't have one already.
2. Create a new Github Organization with your ServerName/CompanyName.
3. Create a new Reposiory for the Gamemode you are developing under that organization.

It should look something like this **github.com/MoonNetwork/DarkRP**

Next we need to set up the local server before going any further with Github.

### 2. Local Dedicated Server
1. First you need to install SteamCMD. SteamCMD is used to download/update the Garry's Mod game. You can go [here](https://developer.valvesoftware.com/wiki/SteamCMD), scroll down to Download, and following the instructions for your OS.
2. Create a new directory for your server located in a safe location and name it whatever you want. I.e. "MoonNetworkDarkRP". Save the location of this directory as you will need it for the next step.
3. Go to where you installed SteamCMD and open it. The first time you open the program, it will download all of the required files for it to run. Once it has finished, type in these commands in order:
```
force_install_dir <path_to_your_server_directory>
login anonymous
app_update 4020 validate
```

This will install the Garry's Mod Dedicated Server in that directory, now we need to configure it.

4. Create a new file in the root directory of the installed garry's mod server, name it `start.bat`.
5. Open it and paste these contents, but make sure to replace the <values> with your own.
```
start "SRCDS" /B srcds.exe -game garrysmod -conlog -port 27015 -console -conclearlog -condebug -tvdisable -maxplayers 16 +gamemode <GAMEMODE_HERE> +r_hunkalloclightmaps 0 +map <MAP_HERE> -tickrate 26 +fps_max 26 +host_workshop_collection "<WORKSHOP_ID_HERE>" +sv_lan 0
```

Now if you run `start.bat`, a terminal should open and the server should start. If it doesn't run, then congratulations you failed.

Now that we have the server installed, we must set up our github repository with the `addons` folder.

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
3. Create a new Private Reposiory for the Gamemode you are developing under that organization. Make sure it is set to Private.

It should look something like this **github.com/MoonNetwork/DarkRP**
![image](https://user-images.githubusercontent.com/48765827/236049473-f81edb99-3612-4a4e-bfc7-102891d84040.png)

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

If you already have a live server with addons installed, now is the time to copy everything from the live server to your local server, only the `addons` folder.

6. Open a terminal in your local server's `addons` directory. You can do this by right cicking in the directory and clicking "Open in Terminal".
7. Type in these commands to initialize the github repository with the repository you made.
```
git init
git remote add origin <THE_URL_TO_YOUR_REPOSITORY>
git fetch
git checkout main
git add .
git commit -m "init"
git push origin main
```

This will initialize a new repository, set it up with the repository you made, fetch any files the repo may have, add the files we already had, and push it to git.

Congratulations, you created a repository and are now backing up your server files. Let's move on and set up DeployHQ.

### 3. DeployHQ

1. Go to https://deployhq.com/ and create a new account.
2. Sign into your deploy panel and you should be greeted with a "Create a Project" screen.
![image](https://user-images.githubusercontent.com/48765827/236048057-9feccc4b-5b6a-40ab-9896-f31c2ffdb043.png)
3. Fill out the server information and select Github, click Create Project.
![image](https://user-images.githubusercontent.com/48765827/236048127-42c75682-93bf-43f3-aa95-d4fe53795488.png)
4. You should be able to link your Github account to your DeployHQ account, allowing you to select the Server's Repository. 
![image](https://user-images.githubusercontent.com/48765827/236048298-4e0558ad-8cd0-47b2-b0e5-9fd46824b0e6.png)
5. Now we must link the repository to the live serve so updates can be pushed. Fill out the New Server form with your server's connection info. For Physgun servers, use SSH/SFTP.
![image](https://user-images.githubusercontent.com/48765827/236048546-0a308b64-6208-4bd1-ba5b-3511c8f16e83.png)
6. On the bottom of the page, you should see Deployment Path as an option. This will be the directory the repository is updating the files to. For Physgun servers, this should be `/garrysmod/addons`, though it could differ depending on the server's host. Now you can click Create Server.
![image](https://user-images.githubusercontent.com/48765827/236048822-14f0cbfa-2ee2-4caf-9854-db063c3aba60.png)

If you didn't get any errors then great, it worked. Otherwise, you're going to have to debug the problem. 

Now whenever we want, we can go to the deploy's overview page and click New Deployment. This will allow you to update the server with the latest commit, which should be "init". Go ahead and run the first deployment so DeployHQ is synced with what files we have on the server.

Check FTP and make sure all the files are in the correct place. Great, this is probably the last time you'll need to use FTP.

### 4. IDE (Visual Studio Code)

You will most likely want an IDE, in this case Visual Studio Code, to manage/update your files. Visual Studio Codes makes things easier by having a built-in Source Control window which shows every change you've made on that commit. It's helpful for those times you forget what you've changed and want to look back and verify everything is good. It can also be helpful for mass-finding a function or string that is messing up. Just click `Ctrl + Shift + F` to search through every file.
1. Download and install Visual Studio Code [here](https://code.visualstudio.com/)
2. Now you can open your addons directory in VSCode and boom, you have the power of FTP all in one editor! 😎

### Conclusion

Now that we have the Github Repository storing the server files, the DeployHQ account ready to push our changes to the live server, and the dedicated server to make the changes on, we just need to put it all together.

### Development Flow
1. Open your server's `addons` directory in VSCode. You can either open a terminal in the addon's directory and type `code .` or you can right click in the directory and click 'Open with Code'.
2. Make a change, or changes.
3. Push changes to Github:
```
git add .
git commit -m "feat: my changes"
git push origin main
```
4. Go to your DeployHQ overview and click `New Deployment`. Click Deploy to deploy your latest commit to the sever.

And that's it, you just updated the live server with your local server's files!

### Bonus
When commiting to github, you have to type 3 commands that are hard to remember every time. Make this process easier by installing my Github CLI, (Boom)[https://www.npmjs.com/package/@steelio/boom-cli].

This cuts down the pushing process from this:
```
git add .
git commit -m "feat: my changes"
git push origin main
```
To only this:
```
boom push "feat: my changes"
```

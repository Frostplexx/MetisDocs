## Prerequisites
* Nodejs
* npm
* A server or something where you can run the bot on.
* patience

## Setup

### Initial Setup
1. Clone the repo with `git clone https://github.com/Frostplexx/Symphone.git`.
2. run `npm i` to install the required packages. If you dont have python 2 install (which you should't) run `npm config set python /path/to/python3` otherwise sqlite 3 wont install.
3. go to https://discord.com/developers/applications and create a new Application.
4. Go to the bot tab and create a new bot.
5. Run the bot with `npm run start`. The bot will generate some files and ask you for some IDs like your bot token or the application id.
most of these IDs can be gathered at the discord developer portal.
4. The bot should run now.
### Deployment
For deployment we use pm2. Install it on your server and optionally configure the web dashboard. With `npm run prod` you can start Metis with pm2. Now you can open pm2.io in a webbrowser and monitor the bot! It even has custom metrics.
![image](https://user-images.githubusercontent.com/62436912/156926798-4f8cff67-be32-48c1-96d9-a7adecc67bfb.png)


## Customisation
### Permissions
You can add permissions in the permissions.json file under src/data/permissions.json. There are already 3 Roles preconfigured, but you can always add more. Other than Roles you can also add Users to this file. Every permission has this template: 
```
{
  "name": "exampleName",
  "id": "123456789012345678",
  "type": "ROLE",
  "permission": true
}
```
`name`can be whatever you want and is only used inside the code.\
`id` is the ID of the Role or the User. You can get it by right clicking the item inside discord and copying the id (with developer mode enabled)\
`type`can either be USER or ROLE, depending on if you want to add a user or a role\
`permission`dictates if this user or this role can user the command or cant.
The second part of adding a permission is it also inside the code. For this open the command file you want inside src/commandHandler/commands. Scroll to the bottom of the command until you see
```
//_____SlashCommand (do not change)_______
export const command: SlashCommand = {
	permissions: ["modRole", "devRole", "adminRole"],
	data: data,
	execute: execute,
};
```
Add the name you have just given your new permission to the permissions Array like this: 
```
//_____SlashCommand (do not change)_______
export const command: SlashCommand = {
	permissions: ["modRole", "devRole", "adminRole","exampleName"],
	data: data,
	execute: execute,
};
```
Lastly run `npm run deploy-commands` to update the commands. If you now restart your bot the permissions should be updated.
### Messages
Most messages can be found in src/data/messages.json where you can freely edit them to your hearts content.
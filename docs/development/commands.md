# Commands

In this section we will explore how commands are created and registered.

## Creating a command

### Writing the command

As seen [here](../../commands/commands) commands have the following basic layout: 
```
import { SlashCommandBuilder } from "@discordjs/builders";
import { CommandInteraction, Message } from "discord.js";
import { SlashCommand } from "../commands";


//______SLASH COMMAND OPTIONS_________
const data = new SlashCommandBuilder()
  .setName("name")
  .setDescription("description")
  //false = @everyone role cant use this command
  .setDefaultPermission(false);

//_________Command____________
async function execute(interaction: CommandInteraction) {
    // write your command here
}


//___________SlashCommand___________
export const command: SlashCommand = {
  permissions: ["devRole"],
  data: data,
  execute: execute,
};
```
After the intital imports a new `SlashCommandBuilder()` is created. This Builder __needs__ to have the following Parameters: 

- setName -- the name you want to give your command / the name with which the command is called in discord. It has to be all lower case without spaces
- setDescription -- description that shows in discord
- setDefaultPermission -- if this is set to true everyone has per default access to this command. For commands only intended for some roles set this option to false

Inside the `execute` function is where your code will get executed. Write your command here. The last part of the command should be left alone. Except the permissions Array. Add here the Role or Users who should have access to your command. For more information see [here](../../installation/customization/#permissions)

The .ts file of your command has to be inside the "src/commandHandler/commands" folder but can be in any subfolder you like. Inside the same folder you'll also find the same snippet as on this page in the "commandCodeSnippet.txt".

### Registering Commands 

You can register commands with `npm run deploy-commands`. This will first recursively read all the files inside "src/commandHandler/commands" and save the to an array. Afterwards a call to the Discord API is made where all the commands are registered with the discord bot token and their command data: 
```
(async () => {
	try {
		console.log('Started refreshing application (/) commands.');
		await rest.put(
			Routes.applicationGuildCommands(process.env.CLIENT_ID!, process.env.GUILD_ID!),
			{ body: commands },
		)
		console.log('Successfully reloaded application (/) commands.');
		
	} catch (_error) {
		console.error(_error);
	}
})();
```
While registering the commands the bot will start and after it started completely the permissions for all the commands will be set.

### Setting Command Permissions

After the bot started all the commands that are registered to it are fetched with `const comms = await guild.commands.fetch();`
Then the `roles` Array is read containing all the roles we defined in the permissions.json.
This is an example of such a Role:
```
{
	"name": "adminRole",
	"id": "940188878692835368",
	"type": "ROLE",
	"permission": true
}
```
Now we loop through all the commands inside `comms` and for each one we get the permission array defined here: 
```
//___________SlashCommand___________
export const command: SlashCommand = {
  permissions: ["devRole"],
  data: data,
  execute: execute,
};
```
and search for the name in the `roles` array. When we found the correct role we map its data to an interface called `DeployablePermissionData`, which looks like this, and return it: 
```
interface DeployablePermissionData{
	id: string;
	type: string;
	permission: boolean;

}
```

-  `id` is the id of the Role or the User
-  `type` can eiter be USER or ROLE
-  `permission` tue means the role or user can use the command. false means he cant.
  
The whole function looks like this:
```
function mapDeployablePermissions(permissionsNamesArray: string[], roles: Array<any>) {
	const permissionsArray: any = [];
	let deployablePermissions: DeployablePermissionData[] = [];
	permissionsNamesArray.forEach(permission =>{
		permissionsArray.push((roles.find(role => role.name === permission)) ?? "")
	})
	deployablePermissions = permissionsArray.map((role: any) => {
		return {
			id: role.id ?? "123456789012345678",
			type: role.type ?? "ROLE",
			permission: role.permission ?? false
		}
	}
	)
	return deployablePermissions;
	
}
```
Now that we have the permission data for every permission of this command we add it and the command id to the `fullPermissions` Array. After we do this for every remaining we have an Array
```
[
  {
    id: '950428001533845659',
    permissions: [
		{
			id: "123456789012345678",
			type: "ROLE",
			permission: true
		},
		{
			id: "123456789012345678",
			type: "ROLE",
			permission: true
		}
	]
  }
]
```
In the last step this Array gets passed into the `guild.commands.permissions.set();` function where all the perimissions are finally set.

## Editing or Deleting commands

### Editing Commands
You can edit the content of 
```
//_________Command____________
async function execute(interaction: CommandInteraction) {
    // write your command here
}
```
but if you update for example the perimissions you need to run `npm run deploy-commands` agian.

### Deleting Commands
If instead you want to delete a command you only need to delete its file from the "/commands" folder and then run `npm run deploy-commands`. 
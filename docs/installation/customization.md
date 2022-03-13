# Customisation
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
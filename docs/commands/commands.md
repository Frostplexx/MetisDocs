# Commands
All Commands are stored in src/commandHandler/commands and are divided into these catergories: 

* Utility
* Player
* Development
* Campaign

## Creating a new command
1. To create a new command please create a .ts file inside the commands directory. 
2. Add this boilerplate code: 
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
3. Replace "name" with the name for your command and "description" with the description. Attention! your name has to be with all lowercase letters and no spaces
4. write your command inside the `execute(interaction)` function
5. run `npm deploy-commands` to register your new command
6. start your bot. Your command should now be working!
7. If you want to edit the permissions please look [here](../../installation/customization/#permissions)

## 
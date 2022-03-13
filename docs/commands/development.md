# Buttons 
Some commands that are only used in development

### dev-edit-campaign
See edit-campaign under the Campaigns section


### nuke
Deletes all campaigns and database entries from the bot

| Argument   | Description                                                                             |
| ---------- | --------------------------------------------------------------------------------------- |
| nukemode   | security switch, true = you can nuke, false = it wont let you nuke the server           |
| passhprase | Additionaly to nukemode being set to true, you also need to know the secret passhphrase |

### purge 
This command deletes a given amount of messages. Messages cant be older than 14 days, otherwhise the bot returns an error. 

| Argument | Description                                                        |
| -------- | ------------------------------------------------------------------ |
| amount   | amount of messages you want to delete. Has to be between 1 and 100 |

### ping 
This command does nothing and is used for testing only


# Player

These commands help you to manage players

### add-player

Add a User to a campaign

| Argument    | Description                                             |
| ----------- | ------------------------------------------------------- |
| user        | type the name of the user (either Username or Nickname) |
| campaign_id | id of the campaign you want to add the user to          |

### status

Get your own status. This command returns all the campaigns you are in and some info about them.

### player-status

This is nearly the same as the status command. It gets all the campaign a user is in.

| Argument | Description                                             |
| -------- | ------------------------------------------------------- |
| user     | type the name of the user (either Username or Nickname) |

### kick-player

Kick a player from a campaign. Attention! the player can apply again to the same campaign, so it is not a ban.

| Argument    | Description                                             |
| ----------- | ------------------------------------------------------- |
| user        | type the name of the user (either Username or Nickname) |
| campaign_id | id of the campaign you want to add the user to          |

## Quotes

### add-quote

| Argument | Description                                                      |
| -------- | ---------------------------------------------------------------- |
| user     | type the name of the user (either Username or Nickname)          |
| quote    | quote you want to add                                            |
| tags     | list of relevant tags separated by a comma. e.g. funny,hello,lol |

Saves the quote to quotes.json where it can be called with other commands.

### delete-quote

| Argument | Description                                             |
| -------- | ------------------------------------------------------- |
| user     | type the name of the user (either Username or Nickname) |

Generates a dropdown where you can select a quote from the given user to delete.

### quotes

| Argument | Description                                             |
| -------- | ------------------------------------------------------- |
| user     | type the name of the user (either Username or Nickname) |
| content  | content to filter by                                    |
| tag      | tag to filter by                                        |

List all the quotes of a user. You can also filter them by content and tag.

### q

Get a random quote from yourselve.

### give-quote

| Argument | Description                                             |
| -------- | ------------------------------------------------------- |
| user     | type the name of the user (either Username or Nickname) |
| content  | content to filter by                                    |
| tag      | tag to filter by                                        |

Get a random quote from a user. These quotes can be filtered by tag and content.

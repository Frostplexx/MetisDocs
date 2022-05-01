# Buttons

Some commands that are only used in development

### dev-edit-campaign

See edit-campaign under the Campaigns section

### nuke

Deletes all the campaigns and users from the database and all campaign messages, channels and roles from the server. Basically starts the bot from new.
This is documented badly on purpose, so that nobody gets any funny ideas!

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

### gen-test-campaign

| Argument | Description                                                                |
| -------- | -------------------------------------------------------------------------- |
| dm       | optional argument to set the dm. If this is empty the dm is a default user |

Generates a campaign with some preset values. This can be used to quickly generate a campaign without having to fill out the form




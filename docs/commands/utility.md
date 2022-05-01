# Utilites

These are some utlity and qol commands

### feedback

Get the link to the feedback form.

### get-form

Get the link to the form to create a new campaign.

### help

Help command of the bot.

### roll

Rolls a die.

| Argument | Description                               |
| -------- | ----------------------------------------- |
| dice     | e.g. '2d8+3'. Use the normal die notation |
| check    | Skill in capital letters e.g. STEALTH     |

### campaign-info

Generates a dropdown where you can select a campaign from the server and see some info on it. In essence it shows the same info as the campaign message but also the associated channels and roles and their ids.

### check

This makes a D&D check.

| Argument | Description                                                           |
| -------- | --------------------------------------------------------------------- |
| check    | Skill in capital letters e.g. STEALTH. and also have modifier e.g. +5 |

### migrate

Used to migrate old campaigns to the new system. Generates a new campaign but no new channels, but uses existing ones. Also adds the players automatically

| Argument         | Description                                                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| campaign_name    | Set the name of the campaign                                                                                                  |
| description      | Set the descrption of the campaign                                                                                            |
| language         | set the language                                                                                                              |
| players_max      | set the maximum number of allowded players in the campaign                                                                    |
| location         | this is the text that will be displayed under the "🏠 Where" heading in the campaing                                           |
| time             | this is the text that will be displayed under the "🕣 When" heading in the campaign                                            |
| notes            | this is the text that wille be displayed underthe "📝 Notes" heading in the campaign                                           |
| difficutly       | this is the difficulty of the campaign. It can be one of the following string:  "Beginner/Easy", "Medium", "Expert/Difficult" |
| dm               | Dungeon Master -- Discord User which is used internally                                                                       |
| dm_namer         | Dungeons Master name -- Name that appears on the message                                                                      |
| role_id          | id of the role that lets people view the right channels for the campaign                                                      |
| text_channel_id  | id of the text channel for this campaign                                                                                      |
| voice_channel_id | id of the voice channel for this campaing                                                                                     |
| category_id      | id of the category that contains the voice and text channel                                                                   |
| player1-5        | add player to campaign                                                                                                        |

### pruge

Deletes a given number of messages, that can't be older than 14 days.

| Argument | Description                                         |
| -------- | --------------------------------------------------- |
| amount   | amount of messages you want to delete between 1-100 |

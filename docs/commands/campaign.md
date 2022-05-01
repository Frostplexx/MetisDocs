# Campaign

Here are the arguably most important commands for the bot - everything that has to do with campaigns

### add-database-entry

With this command you can add a new entry to the Campaign Database.

| Argument         | Description                                                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| campaign_name    | Set the name of the campaign                                                                                                  |
| description      | Set the descrption of the campaign                                                                                            |
| language         | set the language                                                                                                              |
| players_max      | set the maximum number of allowded players in the campaign                                                                    |
| players_current  | set the amount of current players in the campaign                                                                             |
| location         | this is the text that will be displayed under the "🏠 Where" heading in the campaing                                           |
| time             | this is the text that will be displayed under the "🕣 When" heading in the campaign                                            |
| notes            | this is the text that wille be displayed underthe "📝 Notes" heading in the campaign                                           |
| difficutly       | this is the difficulty of the campaign. It can be one of the following string:  "Beginner/Easy", "Medium", "Expert/Difficult" |
| message_id       | id of the message that contains the campaign embed                                                                            |
| dm_id            | user id of the dungeon master                                                                                                 |
| role_id          | id of the role that lets people view the right channels for the campaign                                                      |
| text_channel_id  | id of the text channel for this campaign                                                                                      |
| voice_channel_id | id of the voice channel for this campaing                                                                                     |
| category_id      | id of the category that contains the voice and text channel                                                                   |
| campaign_id      | id of the whole campaign. campaign ids always start with 8                                                                    |

### edit-campaign

edit any of the following parameters of any campaign

| Argument         | Description                                                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| campaign_id | edit the campaign with this id. Can be found in the  footer of the embed|
| campaign_name    | Set the name of the campaign                                                                                                  |
| description      | Set the descrption of the campaign                                                                                            |
| language         | set the language                                                                                                              |
| players_max      | set the maximum number of allowded players in the campaign                                                                    |
| players_current  | set the amount of current players in the campaign                                                                             |
| location         | this is the text that will be displayed under the "🏠 Where" heading in the campaing                                           |
| time             | this is the text that will be displayed under the "🕣 When" heading in the campaign                                            |
| notes            | this is the text that wille be displayed underthe "📝 Notes" heading in the campaign                                           |
| difficutly       | this is the difficulty of the campaign. It can be one of the following string:  "Beginner/Easy", "Medium", "Expert/Difficult" |

### gen-test-campaign

Generates a Campaign with predefined values. For development purposes only!

### delete-campaign

Generates a dropdown of campaigns where you are the dm. You can then select one of the campaigns and delete it. Campaigns are also saved to the trash where they can be restored up to 24h after deletion.

### dev-delete-campaign

| Argument    | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| campaign_id | id of the campaign which can be found in the footer of the embed. |

Deletes a campaign instantly without the option to restore it!. Only to be used by admins and devs.

### undo-camp-delete

Generates a dropdown of deleted campaigns where you can choose one to restore. Campaigns are saved in trash for 24h after that they are deleted forever. Attention! Content of e.g. Text channels is not saved

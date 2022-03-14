# Development

This is the Development section of the Bot. This section is intended to aid in the maintenance of the bot.

## Additional Info

Some Additional Info that helps in development

- Accent Color Hex code --- #ED2027
- Discord IDs are too big for int and should therefore always used as strings.
- If sql wont install run `npm config set python /path/to/python3`
- [How to create a request server to receive webhooks](https://stackoverflow.com/questions/5692960/node-js-create-a-web-hook)

## Setting up your Dev Environment

It is strongly recommended to use a Code Editor like VSCode.

### Prerequisites

- Nodejs
- Python
- A Discord Application from [Discord Dev Portal](https://discord.com/developers/applications)
- A Discord server you can use your Bot while developing

### Installation

These steps are the same as if you would install the bot on a server.

1. Clone the repo with `git clone https://github.com/Frostplexx/DnD_Helper.git`.
2. run `npm i` to install the required packages. If you don't have python 2 install (which you should't) run `npm config set python /path/to/python3` otherwise sqlite 3 wont install.
3. go to <https://discord.com/developers/applications> and create a new Application.
4. Go to the bot tab and create a new bot.
5. Run the bot with `npm run start`. The bot will generate some files and ask you for some IDs like your bot token or the application id.
most of these IDs can be gathered at the discord developer portal.
6. The bot should run now.

### Developing the Bot

You can now freely edit the bot as you please. Its is recommended that you run the bot with the `npm run dev` command as this allows the bot to automatically restart when you make changes to it. Additionally for debugging purposes in VSCode we include a launch.json in the .VSCode folder, which will allow you to use breakpoints. </br>
We also recommend install a linter like eslint, which allows you to write cleaner and more consistent looking code

### Using Ngrok

If you want to test out test the google form, but cant on the production bot it is recommended to use a tunneling service like ngrok.</br>
To get started go to <https://ngrok.com/download> and download and install ngrok for your operating system. Now open a terminal and type `ngrok http 3000` this will open a tunnel to <http://localhost:3000>. Now a ui will appear which looks similar to this:
![image](https://cdn.discordapp.com/attachments/950686852325711882/952857415597047818/Screenshot_2022-03-14_at_10.14.58.png)
Copy the URL which forwards to <http://localhost:300>, in this case it is "http://2f64-2a02-810d-d40-65a0-dde3-275-e5f2-f210.ngrok.io" and
paste it into the `POST_URL` in the google forms script. It should look something like this: 

```javascript
var POST_URL = "http://2f64-2a02-810d-d40-65a0-dde3-275-e5f2-f210.ngrok.io/form";
var TOKEN = "";
function onSubmit(e) {
    var form = FormApp.getActiveForm();
    var allResponses = form.getResponses();
    var latestResponse = allResponses[allResponses.length - 1];
    var response = latestResponse.getItemResponses();
    var items = new Map();

    for (var i = 0; i < response.length; i++) {
        var question = response[i].getItem().getTitle().toLowerCase()
          .replaceAll(" ", "_")
          .replaceAll("/", "_")
          .replaceAll("&", "")
          .replaceAll("__", "_")
          .replaceAll("(", "")
          .replaceAll(")", "");
        var answer = response[i].getResponse();
        try {
            var parts = answer.match(/[\s\S]{1,1024}/g) || [];
        } catch (e) {
            var parts = answer;
        }
        for (var j = 0; j < parts.length; j++) {
            items.set(question, parts[j] ?? "")
        }
    }
    var parsed ={
        "campaign_name" : items.get("campaign_name"),
        "dm_name" : items.get("dungeon_master_name"),
        "language" : items.get("language"),
        "description" : items.get("campaign_description"),
        "difficulty" : items.get("difficulty"),
        "players_max" : items.get("maximum_amount_of_players"),
        "location" : items.get("where_online_irl_location"),
        "time" : items.get("when_day_s_time"),
        "notes": items.get("additional_notes") ?? "No Notes",
        "icon": items.get("campaign_icon"),
        "username": items.get("discord-tag")
    }
    console.log(parsed)
    var options = {
        "method": "post",
        "headers": {
            'Authorization': 'Bearer ' + TOKEN,
            "Content-Type": "application/json",
        },
        "payload": JSON.stringify(parsed)
    };

    UrlFetchApp.fetch(POST_URL, options);
};
```

Don't forget to add the "/form" at the end of the URL. Now save the script and when you send a new campaign form it should now be received by your development bot. </br>
If you are finished with development don't forget to change the url back to the one of your production bot, or better yet have a completely separate form just for development.

# Creating the Google From

To create a campaign we use Google Forms, because it is the easiest way without hosting an additional website.

## Creating the google Form

First we need to create the google form. Our current form looks like this:
![image](https://cdn.discordapp.com/attachments/950686852325711882/952531731418857552/Screenshot_2022-03-13_at_12-39-38_New_Dungeons_and_Dragons_Campaign.png)
Go to [forms.google.com](https://forms.google.com) and create a new form. The only thing that you have to keep in mind is, that you'll have to change some settings in the next section if your decide to add or remove sections or if you change the titles.

## Google Forms Script

The second part of creating the Google Form is programming the part that send the the forms data to our bot. For this click the 3 dots in the top right corner of the google forms editing page and fill in the following script:

```javascript
var POST_URL = "";
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

For `POST_URL` you need to fill in the URL your bot is listening to, followed by "/form" (e.g. <https://example.com/form>). `TOKEN` on the other hand contains the bearer token with which the bot checks if its really coming from your google form. This should be a strong password and it hast to be the same as the `SECRET_KEY` variable that you set for your bot.
If you changed any of the section while creating the google form, you'll need to change the names of the keys in the "parsed" variable. As you can see the keys are the Titles of the questions but in a more machine readable form. They follow this notation:

- Spaces get turned into "_"
- / & and () are removed
- if double empty spaces should happen they get reduced to "_"
- everything is lowercase

Here are some examples how the titles will look:

- Campaign Name >> campaign_name
- Dungeon Master Name >> dungeon_master_name
- Where (online/irl & location) >> where_online_irl_location
- When (day/s & time) >> when_day_s_time

Because some of these names are hard to work with, everything gets parsed into the `parsed` variable, which is then sent in the body of the web request to our bot.

# Campaigns

This Document will explain the flow every campaign goes trough until it appears as a message in your discord server

## Sending a Campaign

The first step is that the user fills out the google form and sends it. The campaign will be converted from a google form to a json object which looks like this:

```json
{
 "campaign_name": "Test Campaign",
 "dm_name": "Hans",
 "language": "English",
 "description": "This is the description",
 "difficulty":"Medium",
 "players_max": 5,
 "location": "Some location",
 "time": "Every day, all day long",
 "notes": "here come the optional notes",
 "icon": "Shield",
 "username": "Hans#0002"
}
```

Important to note is, that difficulty can be either "Beginner/Easy", "Medium" or "Expert/Difficult" and icon can be
"Dice", "Swords" or "Shield" as seen in the interface for the google forms data:

```typescript
export interface GoogleFormsData {
 "campaign_name": string,
 "dm_name": string,
 "language": string,
 "description": string,
 "difficulty": "Beginner/Easy" | "Medium" | "Expert/Difficult",
 "players_max": number,
 "location": string,
 "time": string,
 "notes": string | null,
 "icon": "Dice" | "Swords" | "Shield",
 "username": string
}
```

If the express server now receives the google forms data it calls the `generateCampaign()` function in the next step.

## Generating the Campaign

### Generating channels and buttons and registering user

Before anything else the bot generates the correct channels and registers the buttons for the campaign. After that it adds the user which it gets via the username (=Discord tag).

```typescript
 //fetch user by username and get the id
 const user = await getMemberFromUsername(data.username, process.env.GUILD_ID!);
 
 //generates and gets corresponding channels
 const campaignChannels: CampaignChannels = await generateCampaignChannels(
  data.campaign_name,
  user?.id ?? "",
 );

 //add DM to User Database
 const userData = await userToUserDataObject(user as GuildMember, { isDM: true, isInCampaign: true });
 await addUserDataDBEntry(userData).then(async () => {
  await addCampaignToUser(user!, campaignId);
 });
 //generates and registers the "apply for campaign" Button
 const btnRow = new MessageActionRow().addComponents(
  new MessageButton()
   .setCustomId("beitreten")
   .setLabel("Apply for Campaign ")
   .setStyle("PRIMARY")
   .setEmoji("📝"),
 );
 registerCampaignEmbedButton(
  btnRow.components[0] as MessageButton,
  campaignId,
 );
```

Campaign Channels are made up of a text channel, a voice channel and a category as a parent. The the Category is named the same as the name of the campaign and the voice and text channels are called "Campaign Voice" and "Campaign Text" respectively.</br>
In addition to the channels the bot creates also roles in the same step. These roles have the permission to see and access the aforementioned channels and are names "[campaign name] -- Roll". Also mods and admins have access to the channels.</br>
Afterwards it generates the embed for the campaign by calling `formsDataToCampEmbed()` and passing the campaign data and the campaign id.

### Generating the Embed

The Embed is a big Javascript Object which looks like this:

```typescript
 {
  color: Number.parseInt(campData.color.replace("#", "0x")),
  title: `__${campData.title}__`,

  description: campData.description,
  fields: [
   { name: "**🏠 Where**", value: campData.location, inline: false },
   { name: "**🕣 When**", value: campData.time, inline: false },
   {
    name: "**📝 Notes**",
    value: campData.notes ?? "",
    inline: false,
   },
   { name: "\u200B", value: centerText(" Additional Info ", 30, "━"), inline: false },
   {
    name: "\u200B",
    value: `⚔️ ${campData.currentPlayers}/${campData.maxPlayers} Players\n 📖 DM Name: ${campData.dmName}`,
    inline: true,
   },
   {
    name: "\u200B",
    value: `🧮 Difficulty: ${campData.difficulty} \n 🌍 Language: ${campData.language}`,
    inline: true,
   },
  ],
  image: {
   url: "https://i.imgur.com/Tz2QZ5O.png",
  },
  thumbnail: {
   url: url,
  },
  timestamp: campData.timestamp?.getDate() ?? new Date().getTime(),
  footer: {
   text: campData.footerText + `\nCampaign-ID: ${campData.campId}`,
   icon_url: "https://i.imgur.com/mMW6WlB.png",
  }
 };
```

Where campData is the google forms data object except the icon which is passed separately and associated to a url with this code snippet

```typescript
 let url = "";
 switch (icon) {
 case "Dice":
  url = "https://i.imgur.com/snWt2hZ.png";
  break;
 case "Swords":
  url = "https://i.imgur.com/GJaCzBE.png";
  break;
 case "Shield":
  url = "https://i.imgur.com/GFAHDGo.png";
  break;
 default:
  url = "https://i.imgur.com/snWt2hZ.png";
  break;
 } 
 ```

### Finishing up

This embed is then sent with

```typescript
 await campaignOverviewChannel
  .send({ content: "\u200B", embeds: [campaignEmbed], components: [btnRow] })
  .then((message) => {
   msgId = message.id;
  });
```

and the Campaign is added to the database with

```typescript
 //Inserts the Data into the DB
 addCampaignDBEntry(campaignData);
 await updateMetrics();
```

Also the metrics for pm2 are updated.

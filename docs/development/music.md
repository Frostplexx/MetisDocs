# Music Player

## Usage

Metis, starting with v1.1, also posesses a music player. But instead of any other discord bot, this music player isn't controlled via commands but using a website, found [here](https://metisbot.xyz/music.html).
The basic user flow is as follows:

- The user has to join a voice channel in discord
- Afterwards he writes /new-session, which will generate a session code for him
- The user navigates to <https://metisbot.xyz/music.html> and enters the session code
- He has now access to the dashbaord where he can: create and manage playlists; play, pause, skip and change the volume.
- After 24h the session code is invalid

We chose to to keep the session code valid for 24h no matter what, because it can be used by other players in the same channel, as this code is not player, but channel specific.

## Behind the scenes

The Music player uses the WebSocket protocoll, which allows communicaiton in both dircetion.

### Authenticating

Firstly the website establishes a http connection with the bot/server over which it asks to switch to the WebSocket protocoll. The server will respond and the connection is established.
This first handshake is handled by this piece of code:

```typescript
 server.on('upgrade', (request: any, socket: any, head: any) => {
  wsServer.handleUpgrade(request, socket, head, (socket: any) => {
   wsServer.emit('connection', socket, request);
  });
 });
```

To keep the connection open, a heartbeat has to be established which happens clientside:

```javascript
function ping() {
 connection.send(JSON.stringify({
  "type": "__ping__"
 }));
 tm = setTimeout(function () {
  /// ---connection closed ///
  console.error('connection closed');
 }, 5000);
}

function pong() {
 clearTimeout(tm);
}
connection.onopen = function () {
 setInterval(ping, 30000);
}


connection.onmessage = function (evt) {
    var msg = evt.data;
    if (msg !== '__pong__') {
     checkresponse(msg);
     resolve(JSON.parse(msg));
    } else if(msg == '__pong__') {
     pong();
    }
   }
```

Communication between the client and the server happens with json parsed to a string, because this protocol only allows strings.
A websocket message looks like this:

```json
{
 "type":  //type of action you want to do
 "token":  //the token of the player
 "data":  //the data you want to send can in some cases be undefined
}
```

Where type can be any of the following:

```
 "__pong__",
 "__ping__",
 "playerCreate",
 "playerUpdate",
 "skipPlayer",
 "getPlayerState",
 "authenticate",
```

and token is always the authentication token that the user generated at the beginning. Data may contain extra information but can also be empty depending on which "type" you chose.

### Playing Music

To play music you have to first create the player, which you do like this:

```javascript
{
  "type": "playerCreate",
  "token": "mytoken",
  "data": {
   "songs": [],
   "volume": 10
  }
}
```

"volume" can either be a number between 0 and 100 or can be left out completely. In the latter case the volume will default to 100.
"songs" on the other hand is an array of all the songs you want to put in the queue. A single song object looks like this:

```javascript
{
    "song_id": "12321342,
    "url": "https://www.youtube.com/watch?v=",
    "start_time": "0",
   }
```

"song_id" is a random id which currently goes unused but still has to be passed. Similarly "start_time" goes also unused but will in the future be used to control the starting time of the video. "start_time" contrary to what the name says will be a percentage value that goes from 0% (start of video) to 100% (end of video). The music player currently only supports youtube videos and therfore will the "url" field only be filled with youtube video urls.
Support for youtube playlists needs to be implemented client side and is up to the developer. Lastly youtube lifestreams are also unsupported due to the yt-dl-core which doesn't support youtube life stream.

Most of the other Music Player function are accesed with the "playerUpdate" type:

```javascript
{
  "type": "playerUpdate",
  "token": "mytoken",
  "data": {}
}
```

In this case the "data" field is also important. In can be filled with these values:

```javascript
{
   "state": 
   "volume": 
}
```

"state" can be one of the following values: "paused", "playing", "stopped" and volume is a number between 0 and 100. Both of these are optional, so for example if you want to stop the player you dont need to pass the volume.
The state "stopped" is currently unused.

The only separate function is "skipPlayer" which as the name suggests skips to the next song and only takes the token (This may be update in the future to be included in "updatePlayer"):

```javascript
{
  "type": "skipPlayer",
  "token": "mytoken"
}
```

The last request you can make is "getPlayerState" which returns the current state of the player:

```javascript
{
  "type": "getPlayerState",
  "token": "mytoken",
 }
```

This includes if the player is playing or paused, the name of the song that is playing, its url and youtube thumbnail and the current volume.

### Server responses

As indicated in the previous chapter the server also send a response to every request you make. This reponse looks like this:

```javascript
{
 status: true,
 message: "",
 data: {}
}
```

"status" is true if the request suceeded and false if it failed. "message" is a human readable message intenden for the developer e.g. "Player skipped successfully".
"data" similarly to the "data" field in our request may include additional information which changes from request to request but most of the time includes the current state of the player or some information about the current song.

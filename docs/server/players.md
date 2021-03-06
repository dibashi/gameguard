# Players

When a player connects to the server, the client sends over the player's id and a new Player Object is created for the player. The player is then available to be interacted with through the `player-joined` event emitted by the players module.

Table of Contents

- [Properties](#properties)
- [Events](#events)
- [API](#api)

## **Properties**

### **id**

This is the id for the player, this id is generated by the client or retrieved from a saved cookie if the player is a returning player.

```js
const id = player.id;
```

### **ip**

This is the most probably ip for the player. The IP is retrieved from either the `x-forwarded-for` headers or from the remote address of the player.

```js
const ip = player.ip;
```

## **Events**

Since there is no exact timing with WebSockets, we rely on events to know when things have occurred. After getting the Player Object from the event you can use any method from the API section defined below on them. Each player has the following events that can be responded to:

### **player-joined**

This is emitted when the player has joined and their Player Object has been created by the server. Along with the event, the player's Player Object is also sent over.

```js
gameguard.players.on('player-joined', (player) => {

  console.log(player);

});
```

### **player-left**

This is emitted when the player has left the server through either being disconnected, kicked, or banned. This event also sends over the Player Object.

```js
gameguard.players.on('player-left', (player) => {

  console.log(player);

});
```

### **player-kicked**

This is emitted when the player has been kicked from the server. This event sends over the Player Object and the reason as to why they were kicked.

```js
gameguard.players.on('player-kicked', (player, reason) => {

  console.log(player, reason);

});
```

### **player-banned**

This is emitted when the player has been banned from the server. This event sends over the Player Object and the reason that they were banned.

```js
gameguard.players.on('player-banned', (player, reason) => {

  console.log(player, reason);

});
```

## **API**

### **message**

Sends a message to just this player.

| param   	| type   	| description                                                                  	| default 	|
|---------	|--------	|------------------------------------------------------------------------------	|---------	|
| type    	| string 	| The type of message this is. This helps you decipher messages on the client. 	|         	|
| message 	| string 	| The message to send to this player.                                          	|         	|

```js
gameguard.players.on('player-joined', (player) => player.message('info', 'Hello there!'));
```

### **kick**

Kick a player from the server. This ends their connection to the game server but they can join again by refreshing.

| param  	| type   	| description                                                                                                                                        	| default 	|
|--------	|--------	|----------------------------------------------------------------------------------------------------------------------------------------------------	|---------	|
| reason 	| string 	| The reason as to why this player was kicked from the server. This reason is sent to the client and from there you can use it to inform the player. 	|         	|

```js
gameguard.players.on('player-joined', (player) => player.kick('cheating'));
```

### **ban**

Bans the player from the server. This ends their connection to the game server and sets their id/IP as banned so that they can't join again.

| param  	| type    	| description                                                                                                                                        	| default 	|
|--------	|---------	|----------------------------------------------------------------------------------------------------------------------------------------------------	|---------	|
| reason 	| string  	| The reason as to why this player was kicked from the server. This reason is sent to the client and from there you can use it to inform the player. 	|         	|
| useIP  	| boolean 	| Indicates whether the Player's IP should be banned instead of just their individual id.                                                            	|         	|

```js
gameguard.players.on('player-joined', (player) => player.ban('cheating'));
```
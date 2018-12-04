# battle-cards

Welcome to the jungle... It's time to get your game on!

You have been tasked with building out the front-end of an intense card battling game. 
The server-side team just finished up with the backend game logic for how the game works. 
To comply with these settings you will need to follow a set of specific instructions and HTTP requests.

To get started you need to know about three important routes or API endpoints.

- The baseURL: https://battlecardz.herokuapp.com/cards
	- A Node Server is waiting for requests at /cards. You will need to use the HTTP methods with the above url to build the game.

- GET: /cards
	- This will give you a list of all of the card games created. Details about the game object are below
- POST: /cards
	- This is how you create a new game... A Game Config Object is optional see below
- GET: /cards/:gameId
	- Gets the game at this Id
- PUT: /cards/:gameId
	- Edits the game state. Only accepts an attack Object... See Below
- DELETE: /cards/:gameId
	- Removes a game by its Id
	


### Starting a new game

To start a new game you will need to send the following object to the post route:
```javascript
{
	"gameConfig":{
		"playerName": "", //The player's name
		"set": 2 // Card Images are generated base on game set 1-4
	}
}
```

The response will be a new game object
	
### The Game Object
```javascript
{
	"id": "cdff37e5-a5e4-4596-83cd-a286983aa503", // The Game Id
	"set": 2, // valid card set themes 1-4 
	"opponet": 1, //same as the player object
	"player": { 
			"id": "a97decee-de95-466b-be8c-610852bb06c5", // Your Player Id
			"name": "Jake", //The players Name
			"hand": [ // No more than 5 cards in your hand at a time
				{
					"attack": 4,
					"health": 3,
					"defense": 0,
					"id": "f8e88a52-b888-467d-8423-567ea8a94855", // The cards ID
					"img": "https://robohash.org/f8e88a52-b888-467d-8423-567ea8a94855?set=set2", // An auto generated picture based on the games card set
					"dead": false // The status of the card
				}
			],
			"remainingCards": 5, // The total remaining cards yet to be drawn
			"dead": false //player status
		}
	"dead": [], // Players that have exhasted all of their cards
	"over": false, // The state of the game victory condition
	"winner": false // 3 states for the winner:  The player Object that wins || "Cats Game" || false
}
```

### Playing the Game (the Attack Object)
```javascript
// The only way to play a game is to send the server an attack payload via PUT
// If the payload is valid the attack will be performed... 
{
	"playerCardId": "",
	"opponentCardId": ""
}
// You will recieve a ready response and an updated game object
```

Building this game would breakdown as follows:
 
 1. Create form for a player to provide their name and the game set, then submit to start
	- This should trigger a method to start the game posting the data to the server.
	- The response object can then be used to draw the data
 2. Write a function to handle drawing the cards that come back from the server (*remember: if an enemy card is not marked 'revealed' you should draw the back of a card not the front*)
 3. The player will click their card and one of the enemy cards then select 'play'
 	- This should submit the put request to the server which will return a message in response, this response will have the updated game
 4. When a card is dead, it is added to the 'dead' array on the object, while its not required, maybe render this 'discard pile' somewhere on the page
 	- You will most likely draw only the first index of the dead array
 5. When the game is over, draw the result to the screen and provide the option to start over.
 	- The process should repeat from the earlier new game methods
		

Write the game so the player choses their card, and an opponents card (the first round this will be a blind guess as the cards will only be turned over if they are marked 'revealed')


## Requirements

### Visualization
	- Players can see their hand at all times, and see the backs of the enemy cards
	- Once an enemy card has been attacked its stats are revealed until that card is destroyed

### Functionality
	- The player does not have to manually draw new cards, their hand will always be full until their deck is empty
	- The game is winnable/loseable and tieable

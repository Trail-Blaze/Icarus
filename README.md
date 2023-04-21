# Icarus
> ## TLDR - I haven't had much time to work on this but it's coming along rather well


The Blaze backend — An entire rewrite equipped with account management, 2FA authentication and more!

> _"Icarus laughed as he fell. Threw his head back and yelled into the winds, arms spread wide, teeth bared to the world."_

This backend is written in Typescript and C++. We use MongoDB for the database handlnig since it's simple to use.

### Features
- Robust account handling
  -  Resetting passwords, deleting and creating accounts are all supported
- Optional 2FA (TOTP)
- Built-in party support
- External API that 
  - Gives access to the XMPP server using POST requests
      -  Using either POST requests or the Websocket
      -  Websocket gives the options of using XMPP or SJM
  - Checks authority and gives owner access to upload new lobby themes
- Built in dashboard for managing users, blocking IP addresses
- Allows analytics to be enabled for `/data_router` to be used to collect user information (IP address, client_version, OS, etc.)
- Automatic compensation for different Fortnite versions. This allows any Fortnite version to be used with this backend as it automatically modifies responses depending on the user agent provided in the request.
- Your data is saved on the server once your account is created and your data can only be accessed by you and the person that owns the server. (Thought about encryption but that's a waste of time, only passwords are encrypted with blowfish... and maybe XMPP messages but I'll think about that)
- "Modules" which are basically an executable spawn-list are available along with a WIP Fortnite SDK (not the SDK that your Uncle uses to create things like [Milxnor/Reboot](https://github.com/Milxnor/Project-Reboot) (sorry, man) but something with an actual API that you can use to make cool stuff with bindings available for JavaScript and C/C++)

### The API

#### Backend API
Allows account creation, deletion, modication, authentication with 2 layers (password and 2FA (optional)) and connection with parties and the chat through 2 ways (Fortnite_Client (XMPP) and client (XMPP/SJM)).

#### Fortnite API/SDK (@trail-blaze/XField)

```js
import { FWorld } from "@trail-blaze/XField";

// Get world properties (state, game_mode, playlist, damage) 
// state could have the following values: "disengaged", "engaged", "inProgress"
// disengaged -> Inactive, engaged -> On spawn island, inProgress -> The game is in progress

FWorld.getProperty(); // No argument means get all properties. Providing a string will search through the properties and return the one you specified

// Disable fall damage
FWorld.setProperty("damage", "false");

// Move all pawns to (x, y)
World.getPawnList().forEach((pawn)=>{
   pawn.move(70, 70, 70); // x, y. z
});


// Move one pawn 
const myPawn = World.getPawnByUsername("Array0x");

myPawn.move(600, 600, 600); // x, y, z
myPawn.costume("CID_016_Athena_Commando_F"); // Set the skin (changes the value in the profile's directory as well)
myPawn.kill(); // Kick the pawn from the world

```

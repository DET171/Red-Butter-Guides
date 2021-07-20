# Part 2
If you want to skip the next part, click [here](#cmd).
# Code from Previous Post
As promised, I will be putting the code here for those who just want to grab and go:
Your project directory:
```bash
│   .env
│   index.js
│   package-lock.json
│   package.json
│
├───commands
│       owo.js
│
├───events
│       ready.js
│
└───node_modules
    │   ...
```
`./.env`
```
TOKEN=<your-token-here>
PREFIX=<your-bot-prefix>
```
`./index.js`
```js
const { Client } = require('yuuko');
const path = require('path');
const dotenv = require('dotenv');
var env = dotenv.config();
env = process.env;
 
const bot = new Client({
    token: env.TOKEN,
    prefix: env.PREFIX,
    ignoreBots: true,
});
 
bot.extendContext({
    variableOne: 'Variable number 1!',
});
bot.editStatus('dnd'); // edits bot status
 
bot.on('error', (err) => {
    console.error(err);
});
 
bot.globalCommandRequirements = {
    guildOnly: true,
};
 
bot
    .addDir(path.join(__dirname, 'commands'))
    .addDir(path.join(__dirname, 'events'))
    .connect();
```
`./package.json` + `./package-lock.json `   
I will not be showing this, but you should have `yuuko`, `eris`, and `dotenv` installed.
`./commands/owo.js`
```js
const { Command } = require('yuuko');
module.exports = new Command('owo', (message, args, context) => {
  message.channel.createMessage('OwO');
});
```
`./events/ready.js`
```js
const { EventListener } = require('yuuko');
module.exports = new EventListener('ready', ({client}) => {
  console.log(`Logged in as ${client.user.usename}`);
});
```
That should be all the code for now.

# The `Meme` Command <a name="cmd"></a>
Now, for the `Meme` command! For this, we will need to get the memes from reddit. For that, we will be using `got` to get the JSON from `https://www.reddit.com/r/memes/random/.json`.    

Here's the code (I will be explaining it later):
```js
const { Command } = require('yuuko');
const got = require('got');
module.exports = new Command('meme', (message) => {
	got('https://www.reddit.com/r/memes/random/.json')
		.then((response) => {
			const [list] = JSON.parse(response.body);
			const [post] = list.data.children;

			const permalink = post.data.permalink;
			const memeUrl = `https://reddit.com${permalink}`;
			const memeImage = post.data.url;
			const memeTitle = post.data.title;
			const memeUpvotes = post.data.ups;
			const memeNumComments = post.data.num_comments;
			message.channel.createMessage({
				embed: {
					title: memeTitle,
					url: memeUrl,
					image: {
						url: memeImage,
					},
					color: 15267908,
					footer: {
						text: `👍 ${memeUpvotes} 💬 ${memeNumComments}`,
					},
				},
			});
		})
		.catch(err => {
			console.error(err);
		});
});

```
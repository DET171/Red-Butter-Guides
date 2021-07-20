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
Install `got` first:
```bash
npm i got --save
```
Create a file in `./commands` and name it `meme.js`.
Put the following code inside (I will be explaining it later):
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
Now start the project by navigating to the root folder of the project and running
```bash
node index.js
```
or if you have `nodemon` installed
```bash
nodemon index.js
```
Let me break the code up into smaller pieces to explain it.
```js
const { Command } = require('yuuko');
const got = require('got');
module.exports = new Command('meme', (message) => {
  // code here
})
```
So, we first import the modules as usual, and create a command as we did before. Easy.

```js
got('https://www.reddit.com/r/memes/random/.json').then((response) => {
  // code here
}).catch(err => {
            console.error(err);
});
```
Now, we use `got` to get the JSON from reddit (the subreddit `r/memes` actually), and save the response as the `response` variable. Note that we are using Promises here, thus the `.then().catch()` in the code. You can, however, use the `async/await` in ES6.

Good?

```js
const [list] = JSON.parse(response.body);
const [post] = list.data.children;
```
Now, we parse the response body by using `JSON.parse` (Note: You will get an error if you just use `JSON.parse(response)`), and get the information about the reddit post which we saved inside the `post` variable. Understand? Excellent.
```js
const permalink = post.data.permalink;
const memeUrl = `https://reddit.com${permalink}`;
const memeImage = post.data.url;
const memeTitle = post.data.title;
const memeUpvotes = post.data.ups;
const memeNumComments = post.data.num_comments;
```
Now we save the post url as `memeUrl`, the meme image url as `memeImage`, the meme title as `memeTitle`, the number of meme upvotes as `memeUpvotes`, and the number of comments as `memeNumComments`.

```js
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
```
We then send the embed object. That's the end of it. Easy, right?


# Conclusion
In this post, we used a REST API, and learnt how to send an embed in Eris. For my next post, I will be writing a `whois` command. See you until next time!

# Introduction
Hello there, this sentence will mark the beginning of my first ever article released to the public. In this article, I will be writing how to build a Discord bot with [Eris](https://abal.moe/Eris/) and [Yuuko](https://eritbh.me/yuuko/).

In this tutorial, I will be using the following format for the code:
```js
+ // This line should be added to your code
- // This line should be removed from your code

```

# Prerequisites
- A basic knowledge of JavaScript
- Node.js (v12) and NPM (v7) installed on your machine
- A basic knowledge of the [Discord API](https://discord.com/developers/docs/intro)

# Background Info
So, what is Eris exactly?
> A lightweight NodeJS Discord Library.

What is Yuuko, then?
> A Discord command framework for Javascript and Typescript.

I assume that if you've ever wanted to make a Discord Bot, you would have at least googled it up. The first and most common answer you'd see is probably "How to build a Discord Bot with Discord.js". What exactly is the difference between Eris and Discord.js?

# Features
D.js covers 100% of the Discord API while Eris does not. However, covering 100% of the Discord API has its disadvantages.
D.js has a larger memory footprint, and when the bot is in many servers, it starts having performance issues. That is why many large bots, like [Dank Memer](https://dankmemer.lol/) (The 4th largest Discord Bot), are made using Eris.

However, there are some packages on NPM that can help with the functions that Eris lacks, for example, [Eris Additions](https://www.npmjs.com/package/eris-additions). There are even command handlers for Eris on NPM, like [Yuuko](https://www.npmjs.com/package/yuuko) and [Eris Boiler](https://www.npmjs.com/package/eris-boiler). For developers moving from D.js to Eris, there is [Chariot.js](https://www.npmjs.com/package/chariot.js).

# Getting started
Now, without further delay, let us dive into the magical world of Discord Bots.

# Creating a Discord Bot account
Now, the first thing you need to do is to create a Discord Bot account.
1. Head over to the [Discord Applications page](https://discord.com/developers/applications).
2. Click the `New Application` button on the top right corner.
3. Now name your application (You can change it afterwards). Enter the name and hit `Create`.
4. Now that you have created the application, now you need to create the bot account. Head over to `Bot` and click on `Add Bot`, and then click `Click on Yes, do it!`.
5. Under the `TOKEN` section, click `Copy`.
6. Awesome! Now you have your Bot Token!

Last but not least, do remember to invite your bot into your server in order to "talk" to it.

# Set up your project
1. Create your project folder and `package.json`.
```bash
mkdir <your-project-name>
cd <your-project-name>
npm init
```
Ensure that the `main` in your `package.json` is set to `index.js`.

2. Install the relevant dependencies now.
```bash
npm i eris yuuko dotenv
```
Should you be using a version of NPM below 4.5 *(you shouldn't)*, run the following instead:
```bash
npm i eris yuuko dotenv --save
```
4. Create a `.env` and `index.js` file, and a `commands` and `events` folder.

### Optional Steps
- Install `bufferutil`, `zlib-sync` or `abalabahaha/erlpack`
- Install a linter and create the config file
 ```bash
 npm i eslint -D
 # -D is short for --save-dev
 npx eslint --init
 # Just answer the prompts
 ```
That's about the end of setting up your project!
Your project tree should look something like this now:
```
|   .env
│   index.js
│   package-lock.json
│   package.json
│
├───commands
├───events
└───node_modules
    │   ...
```

# Now, let's start coding!
Note: The final code will be included at the end :)

First, open the project in you favourite text editor, and fill in the `.env` file with the following:
```
TOKEN=<your-token-here>
PREFIX=<your-bot-prefix>
```
Of course, replace `<your-token-here>` with the Bot token you obtained earlier, and `<your-bot-prefix>` with your bot prefix.
If you do not understand `dotenv` and `.env` files,
Now that we are no longer concerned with the bot configurations, let us write our basic bot code!
Head over to your `index.js` file, and insert the following at the top to require the packages.
```js
const { Client } = require('yuuko'); // Imports the Client constructor
const path = require('path'); // For joining paths
require('dotenv').config(); // Imports the variables in the `.env` file
```
As you can see, we have imported the `Client` constructor from `Yuuko` but not the `Command` constructor. Why? We will be putting the commands in `js` files in the `command` folder, so our `index.js` file will not be crowded with commands. Neat!

We can create a client with the following:
```js
const bot = new Client({
	token: process.env.TOKEN,
	prefix: process.env.PREFIX,
	ignoreBots: true,
});
```
The `ignoreBots: true` in the code tells our bot to ignore all messages sent by other bots.

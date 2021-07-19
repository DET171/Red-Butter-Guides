# Introduction
Hello there, this sentence will mark the beginning of my first ever article released to the public. In this article, I will be writing how to build a Discord bot with [Eris](https://abal.moe/Eris/) and [Yuuko](https://eritbh.me/yuuko/).

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

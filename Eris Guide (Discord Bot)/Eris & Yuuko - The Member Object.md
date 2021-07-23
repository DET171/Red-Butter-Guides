# Part 3
In this post, I will be teaching you how to write a `whois` command.

# The `whois` command

So, here's the code:
```js
const { Command } = require('yuuko');
const moment = require('moment');
const { today } = require('../../utils.js');
module.exports = new Command(['whois', 'member'], async (message, args, context) => { // eslint-disable-line no-unused-vars
	if (!args[0]) {
		return message.channel.createMessage(`${message.author.mention}, apologies! Please specify a particular member!`);
	}
	const user = message.mentions[0];
	const guild = message.channel.guild;
	const member = await guild.members.get(user.id);
	message.channel.createMessage({
		embed: {
			title: `User information for ${user.username}#${user.discriminator}`,
			thumbnail: {
				url: user.avatarURL,
			},
			color: 0x008000,
			fields: [
				{
					name: 'Account created at:',
					value: `${moment.utc(user.createdAt).format('MMMM, Do YYYY, h:mm:ss a')}`,
					inline: false,
				},
				{
					name: 'User ID:',
					value: `\`${user.id}\``,
					inline: false,
				},
				{
					name: 'Roles:',
					value: '<@&' + member.roles.map((r) => `${r}`).join('>, <@&') + '>',
					inline: false,
				},
				{
					name: 'Joined server at:',
					value: `${moment.utc(member.joinedAt).format('MMMM, Do YYYY, h:mm:ss a')}`,
					inline: false,
				},
			],
			footer: {
				text: today,
			},
		},
	});
});

```
Create a file in `./commands`, and name it `whois.js`. Proceed to dump the above code into `whois.js`.  You *MIGHT* have to run *`npm i moment --save`* to install the `moment` module.

Now, let me explain the code.
As usual, we require the packages, create the command, and export it:
```js
const { Command } = require('yuuko');
const moment = require('moment');
module.exports = new Command('whois', async (message, args, context) => {
  // code here
});
```
We will then check for arguments. If there are none, we stop the code (or it will return `undefined`):
```js
if (!args[0]) {
    return message.channel.createMessage(`${message.author.mention}, apologies! Please specify a particular member!`);
}
```
We use `message.author.mention` to mention the message author.

We get the first user that is mentioned in the message, get the guild the message was sent in, and get the `member` object from the `guild` object:
```js
const user = message.mentions[0];
const guild = message.channel.guild;
const member = await guild.members.get(user.id);
```
After that, we proceed to send the embed message with the `member` and `user` information:
```js
message.channel.createMessage({
        embed: {
            title: `User information for ${user.username}#${user.discriminator}`,
            thumbnail: {
                url: user.avatarURL,
            },
            color: 0x008000,
            fields: [
                {
                    name: 'Account created at:',
                    value: `${moment.utc(user.createdAt).format('MMMM, Do YYYY, h:mm:ss a')}`,
                    inline: false,
                },
                {
                    name: 'User ID:',
                    value: `\`${user.id}\``,
                    inline: false,
                },
                {
                    name: 'Roles:',
                    value: '<@&' + member.roles.map((r) => `${r}`).join('>, <@&') + '>',
                    inline: false,
                },
                {
                    name: 'Joined server at:',
                    value: `${moment.utc(member.joinedAt).format('MMMM, Do YYYY, h:mm:ss a')}`,
                    inline: false,
                },
            ],
        },
    });
```
However, what if you wanted this command the have two triggers (e.g. `whois` and `member`) instead of just one trigger(`whois`)?
That's quite easy. You just have to replace `module.exports = new Command('whois', async (message, args, context) =>` with `module.exports = new Command(['whois', 'member'], async (message, args, context) =>`

This are just some `user` and `member` properties, more of them can found at the following pages:
- [Member](https://abal.moe/Eris/docs/Member)
- [User](https://abal.moe/Eris/docs/User)

# Conclusion
In this article, we learnt how to send more advanced embed with fields, create command aliases, and fetch members from the guild objects. In my next post, I will be making a `guild` command that shows information about the guild the message was sent in.
Have a nice day!

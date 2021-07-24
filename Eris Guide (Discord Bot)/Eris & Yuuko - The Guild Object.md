# Part 3
In this post, I will be teaching you how to write a `guild` command. 

# The `guild` command
Here's the code As usual, here's the code (put it in `./commands/guild.js`):
```js
const { Command } = require('yuuko');
const moment = require('moment');
module.exports = new Command(['guild', 'server'], (message) => {
	const guild = message.channel.guild;
	const owner = guild.members.get(guild.ownerID);
	message.channel.createMessage({
		embed: {
			title: 'Guild Information',
			description: `Guild information for ${guild.name} (id: \`${guild.id}\`)`,
			color: 11272041,
			thumbnail: {
				url: guild.iconURL,
			},
			fields: [
				{
					name: 'Owner:',
					value: `${owner.username}#${owner.discriminator} (id: \`${guild.ownerID}\`)`,
					inline: false,
				},
				{
					name: 'Created at:',
					value: `${moment.utc(guild.createdAt).format('MMMM, Do YYYY, h:mm:ss a')}`,
					inline: false,
				},
				{
					name: 'Member count:',
					value: `${guild.memberCount} members`,
					inline: false,
				},
			],
		},
	});
});
```
Again, this is quite simple. This is mostly properties of the guild object. More can be found [here](https://abal.moe/Eris/docs/Guild). I will not be covering this in detail, as I will also be covering a `roll` command, which is more complex.

# The `roll` command
As usual, here's the code (put it in `./commands/roll.js`):
```js
const { Command } = require('yuuko');
require('dotenv').config();
module.exports = new Command(['roll', 'rolladie', 'rolladice'], (message, args) => {
	const arg = args.join(' ');
	if(!args.length) {
		message.channel.createMessage({
			embed: {
				title: `${message.author.username} rolled a die!`,
				description: `${message.author.mention} rolled a die and got **${Math.floor(Math.random() * 6) + 1}**!`,
				color: 12252021,
				fields: [
				],
			},
		});
	}
	else {
		try {
			const num = arg.trim().split('d');
			const times = parseInt(num[0]);
			const max = parseInt(num[1]) || 6;
			const nums = [];
			for(let i = 0; i < times; i++) {
				let result = Math.floor(Math.random() * max); // eslint-disable-line prefer-const
				result = result + 1;
				nums.push(result);
			}
			message.channel.createMessage({
				embed: {
					title: `${message.author.username} rolled a ${times} dice!`,
					description: `${message.author.mention} rolled a ${times} dice and got [ **${nums.join(' ')}** ]!`,
					color: 12252021,
				},
			});
		}
		catch(err) {
			console.warn(err);
			message.channel.createMessage(`${message.author.mention}, the correct usage would be \`${process.env.PREFIX} roll <number of dice to roll>d<highest number on the die>\``);
		}
	}
});
```
Now, let me explain. 
A user is supposed to type the command in the followoing format, assuming the prefix is `!`:
```bash
!roll
```
```bash
!roll 10d6
# or !roll 10 d 6
```

btw there are few aliases for this command:
```js
['roll', 'rolladie', 'rolladice']
```
We check for arguments, and if there are none, we roll a random number from 1 to 6 and send it in an embed:
```js
if(!args.length) {
        message.channel.createMessage({
            embed: {
                title: `${message.author.username} rolled a die!`,
                description: `${message.author.mention} rolled a die and got **${Math.floor(Math.random() * 6) + 1}**!`,
                color: 12252021,
                fields: [
                ],
            },
        });
    }
```
And if there are arguments, we join the arguments:
```js
const arg = args.join(' ');
```
and split it by `d`.
We first initialize an empty array`[]`, use the `for` loop to roll a random number between the number the user specified (if not, the default is `6`) and push the result to the array the number of times the user specified.    
After that has been completed, we join the array with spaces and send it in an embed:
```js
else {
        try {
            const num = arg.trim().split('d');
            const times = parseInt(num[0]);
            const max = parseInt(num[1]) || 6;
            const nums = [];
            for(let i = 0; i < times; i++) {
                let result = Math.floor(Math.random() * max); // eslint-disable-line prefer-const
                result = result + 1;
                nums.push(result);
            }
            message.channel.createMessage({
                embed: {
                    title: `${message.author.username} rolled a ${times} dice!`,
                    description: `${message.author.mention} rolled a ${times} dice and got [ **${nums.join(' ')}** ]!`,
                    color: 12252021,
                },
            });
        }
        catch(err) {
            console.warn(err);
            message.channel.createMessage(`${message.author.mention}, the correct usage would be \`${process.env.PREFIX} roll <number of dice to roll>d<highest number on the die>\``);
        }
    }
```
EZPZ.
PS tell me in the comments if you have any trouble.

# Conclusion
Woohoo! We made a `fun` command today, and also accessed the `guild` command. That's about the end of it for today.
Take care and goodbye!
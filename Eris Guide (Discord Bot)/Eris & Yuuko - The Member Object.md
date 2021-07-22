# Part 3
In this post, I will be teaching you how to write a `whois` command.

# The `whois` command

So, here's the code:
```js
const { Command } = require('yuuko');
const moment = require('moment');
module.exports = new Command(['whois', 'member'], (message, args, context) => { // eslint-disable-line no-unused-vars
	if (!args[0]) {
		return message.channel.createMessage(`${message.author.mention}, apologies! Please specify a particular member!`);
	}
	const user = message.mentions[0];
	const guild = message.channel.guild;
	const member = guild.members.get(user.id);
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
});
```
Create a file in `./commands`, and name it `whois.js`. Proceed to dump the above code into `whois.js`.
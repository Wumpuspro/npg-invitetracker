Track the invites in your servers to know who invited who and with which invite!

## Installation

```sh
npm install npg-discord-invitestracker
# or
yarn add npg-discord-invitestracker
```

## Example

```js
const Discord = require('discord.js');
const client = new Discord.Client();

const InvitesTracker = require('npg-tracker');
const tracker = InvitesTracker.init(client, {
    fetchGuilds: true,
    fetchVanity: true,
    fetchAuditLogs: true
});

tracker.on('guildMemberAdd', (member, type, invite) => {

    const welcomeChannel = member.guild.channels.cache.find((ch) => ch.name === 'welcome');

    if(type === 'normal'){
        welcomeChannel.send(`Welcome ${member}! You were invited by ${invite.inviter.username}!`);
    }

    else if(type === 'vanity'){
        welcomeChannel.send(`Welcome ${member}! You joined using a custom invite!`);
    }

    else if(type === 'permissions'){
        welcomeChannel.send(`Welcome ${member}! I can't figure out how you joined because I don't have the "Manage Server" permission!`);
    }

    else if(type === 'unknown'){
        welcomeChannel.send(`Welcome ${member}! I can't figure out how you joined the server...`);
    }

});

client.login(process.env.TOKEN);
```

**Different join types available:**

* `normal` - When a member joins using an invite and the package knows who invited the member (`invite` is available).
* `vanity` - When a member joins using an invite with a custom URL (for example https://discord.gg/discord-api).
* `permissions` - When a member joins but the bot doesn't have the `MANAGE_GUILD` permission.
* `unknown` - When a member joins but the bot doesn't know how they joined.

## Ignoring guilds

```js
const InvitesTracker = require('npg-invitetracker');
const tracker = InvitesTracker.init(client, {
    fetchGuilds: true,
    fetchVanity: true,
    fetchAuditLogs: true,

    // servers that contain "nude" in their name will not be processed
    exemptGuild: (guild) => guild.name.includes('nude')
});
```

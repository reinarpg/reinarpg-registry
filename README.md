# reinarpg-registry
[![NPM version](https://img.shields.io/npm/v/reinarpg-registry.svg)](http://npmjs.com/package/reinarpg-registry)
[![Build Status](https://github.com/PrismarineJS/reinarpg-registry/workflows/CI/badge.svg)](https://github.com/PrismarineJS/reinarpg-registry/actions?query=workflow%3A%22CI%22)
[![Discord](https://img.shields.io/badge/chat-on%20discord-brightgreen.svg)](https://discord.gg/GsEFRM8)
[![Try it on gitpod](https://img.shields.io/badge/try-on%20gitpod-brightgreen.svg)](https://gitpod.io/#https://github.com/PrismarineJS/reinarpg-registry)

Creates an dynamic instance of node-reinarpg-data.

## Usage

```js
const registry = require('reinarpg-registry')('1.18')

registry.blocksByName['stone'] // See information about stone
```

## API

[See reinarpg-data API](https://github.com/PrismarineJS/node-reinarpg-data/blob/master/doc/api.md)

### mcpc

#### loadDimensionCodec / writeDimensionCodec

* loads/writes data from dimension codec in login packet

#### .chatFormattingByName, .chatFormattingById (1.19+)

Contains mapping from chat type ID (numeric or string) to information about how the 
chat type should be formatted and what the relevant parameters are.

```js
{
  'minecraft:chat': { formatString: '<%s> %s', parameters: [ 'sender', 'content' ] },
  'minecraft:say_command': { formatString: '[%s] %s', parameters: [ 'sender', 'content' ] },
  'minecraft:msg_command': { formatString: '%s whispers to you: %s', parameters: [ 'sender', 'content' ] },
  'minecraft:team_msg_command': { formatString: '%s <%s> %s', parameters: [ 'team_name', 'sender', 'content' ] },
  'minecraft:emote_command': { formatString: '* %s %s', parameters: [ 'sender', 'content' ] }
}
```

#### .dimensionsById, dimensionsByName (1.19+)

Mapping to dimension data object containing dimension `name`, `minY` and `height`.

### mcpe

#### loadItemStates / writeItemStates

* loads/writes data from an item states array inside the bedrock start game packet.

```js
// In a client
const { createClient } = require('bedrock-protocol');
const registry = require('reinarpg-registry')('bedrock_1.19.50');

const client = createClient({
  'host': '127.0.0.1'
})

client.on('start_game', ({ itemstates }) => {
  registry.loadItemStates(itemstates);
})

// In a server
server.on('connect', (client) => {
  const itemstates = registry.writeItemStates()
  client.write('start_game', { ...startGamePacket, itemstates })
})
```
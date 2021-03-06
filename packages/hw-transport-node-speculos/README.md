<img src="https://user-images.githubusercontent.com/211411/34776833-6f1ef4da-f618-11e7-8b13-f0697901d6a8.png" height="100" />

## @ledgerhq/hw-transport-node-speculos

A transport for <https://github.com/LedgerHQ/speculos> Nano simulator.

[Github](https://github.com/LedgerHQ/ledgerjs/),
[Ledger Devs Slack](https://ledger-dev.slack.com/)

### Getting started

*   Install <https://github.com/LedgerHQ/speculos>
*   Make sure to have a speculos running with a APDU port and (optionally) a buttons port available.

```js
import SpeculosTransport from "@ledgerhq/hw-transport-node-speculos";

const apduPort = 40000;

async function exampleSimple() {
  const transport = await SpeculosTransport.open({ apduPort });
  const res = await transport.send(0xE0, 0x01, 0x00, 0x00);
}

async function exampleAdvanced() {
  const transport = await SpeculosTransport.open({ apduPort });
  setTimeout(() => {
    // in 1s i'll click on right button and release
    transport.button("Rr");
  }, 1000); // 1s is a tradeoff here. In future, we need to be able to "await & expect a text" but that will need a feature from speculos to notify us when text changes.
  // derivate btc address and ask for device verification
  const res = await transport.send(0xE0, 0x40, 0x01, 0x00, Buffer.from("058000002c8000000080000000000000000000000f"));
}
```

### With ledger-live CLI

It's working with SPECULOS_APDU_PORT and SPECULOS_HOST envs.

```sh
SPECULOS_APDU_PORT=40000 ledger-live sync -c btc

# starts an http proxy with speculos (http proxy that works with LLD and LLM)
SPECULOS_APDU_PORT=40000 ledger-live proxy
```

To make it work with Docker, I had to expose some port and do this:

```sh
docker run -it -p 40000:40000 -v "$(pwd)"/apps:/speculos/apps speculos /bin/bash

$ pipenv shell
$ ./speculos.py -m nanos ./apps/btc.elf --sdk 1.6 --seed "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about" --display headless --apdu-port 40000
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

*   [SpeculosTransportOpts](#speculostransportopts)
    *   [Properties](#properties)
*   [SpeculosTransport](#speculostransport)
    *   [Parameters](#parameters)
    *   [Examples](#examples)
    *   [button](#button)
        *   [Parameters](#parameters-1)
    *   [open](#open)
        *   [Parameters](#parameters-2)

### SpeculosTransportOpts

Type: {apduPort: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number), buttonPort: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?, automationPort: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?, host: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?}

#### Properties

*   `apduPort` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** 
*   `buttonPort` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** 
*   `automationPort` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** 
*   `host` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** 

### SpeculosTransport

**Extends Transport**

Speculos TCP transport implementation

#### Parameters

*   `apduSocket` **net.Socket** 
*   `opts` **[SpeculosTransportOpts](#speculostransportopts)** 

#### Examples

```javascript
import SpeculosTransport from "@ledgerhq/hw-transport-node-speculos";
const transport = await SpeculosTransport.open({ apduPort });
const res = await transport.send(0xE0, 0x01, 0, 0);
```

#### button

Send a speculos button command
typically "Ll" would press and release the left button
typically "Rr" would press and release the right button

##### Parameters

*   `command` **any** 

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)\<void>** 

#### open

##### Parameters

*   `opts` **[SpeculosTransportOpts](#speculostransportopts)** 

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[SpeculosTransport](#speculostransport)>** 

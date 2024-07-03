# deribit-v2-ws

Deribit V2 API WS RPC wrapper with added multiple subscription support, forked from [deribit-v2-ws](https://github.com/askmike/deribit-v2-ws) which was originally published by [github.com/askmike](https://github.com/askmike/).

[Docs are here](https://docs.deribit.com/v2/?javascript#deribit-api-v2-0-0).

This module is designed to be used with nodejs, it doesn't support browser usage.

## Features

- Connect to WS api
- Authenticate
- Reconnect on disconnect
- Resubscribe subscriptions on disconnect

## Example usage

See more examples in `example.js`.

    const Deribit = require('deribit-v2-ws');

    const key = 'x';
    const secret = 'y';

    const db = new Deribit({key, secret})
    await db.connect();
    const position = await db.request(
      'private/get_position',
      {instrument_name: 'BTC-PERPETUAL'}
    );

    console.log(position);

result:

    {
      requestedAt: 1552992953360, // added by this lib
      usIn: 1552992953479560,
      usOut: 1552992953482218,
      receivedAt: 1552992953601, // added by this lib
      jsonrpc: '2.0',
      id: 404,
      result:
       { total_profit_loss: 0,
         size_currency: 0,
         size: 0,
         settlement_price: 3959.67,
         realized_profit_loss: 0,
         open_orders_margin: 0,
         mark_price: 3971.85,
         maintenance_margin: 0,
         kind: 'future',
         instrument_name: 'BTC-PERPETUAL',
         initial_margin: 0,
         index_price: 3972.04,
         floating_profit_loss: 0,
         estimated_liquidation_price: 0,
         direction: 'zero',
         delta: 0,
         average_price: 0 },
      usDiff: 2658,
      testnet: false
    }

Custom multiple subscription support was added with this fork which will now allow passing an array of subscription strings:

    await db.subscribeMultiple(
      'public',
      ['book.BTC-PERPETUAL.none.10.100ms', 'trades.BTC-PERPETUAL.raw']
    );
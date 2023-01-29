# ravencoin-history-list

Creates a list of transactions for a wallet based on address deltas, in a human understandable way.

## What do we mean by human understandable?

Example.
Alice transfer one LEMONADE token to Bob.
Now Alice expects her history to show that she sent one LEMONADE token to Bob.

Technically, a transaction has been broadcasted on the network.
That transaction typically contains

- One or more unspent transaction outputs (UTXOs) to cover the miner fee, Alice needs to pay some RVN to send an asset.
- One or more UTXOs for the LEMONADE transfer, like input 20 LEMONADE, output one to BOB and 19 back to a change address.

## How to use

As a developer, you have to ask a Ravencoin node for the address deltas (getaddressdelta).
The address deltas represents everything that has come in and out from your addresses.

See example below.

## Install

`npm install @ravenrebels/ravencoin-history-list`

## How to use

```
 import {getHistory } from "@ravenrebels/ravencoin-history-list";


```

## Example how to use

Create a JavaScript project (npm init -y) and create an index.mjs file

Run the file `node index.mjs`
![image](https://user-images.githubusercontent.com/9694984/215101963-0fb8b973-3072-473d-817e-3104ecf28fd1.png)

```
import {getHistory} from "@ravenrebels/ravencoin-history-list";

function main() {
  const history = getHistory(getDeltas());

  const toPresent = []; //The result that we will console.table out

  history.map((item) => {
    //Note each item is a transaction but a transaction can have multiple inputs and outputs.
    const transactionId = item.transactionId;
    const blockHeight = item.blockHeight;
    item.assets.map((asset) => {
      toPresent.push({
        assetName: asset.assetName,
        value: asset.value,
        blockHeight,
        transactionId,
        type: item.isSent ? "Sent" : "Received",
      });
    });
  });
  console.table(toPresent);
}
main();

//Lets grab some test data
function getDeltas() {
  return [
    {
      assetName: "RVN",
      satoshis: 10000000000,
      txid: "86bed668a48eb1065e61fca74c23cc263b851ae8014bcafb48389f890af09d96",
      index: 0,
      blockindex: 1,
      height: 910412,
      address: "n1rwf41Xr1Pijap1KVLseCnRB8dc1Ypuz8",
    },
    {
      assetName: "RVN",
      satoshis: -10000000000,
      txid: "38965508c62ea2c0185e5e1f46ece8407fd58269127653f23cc341ead722a4bc",
      index: 0,
      blockindex: 1,
      height: 910448,
      address: "n1rwf41Xr1Pijap1KVLseCnRB8dc1Ypuz8",
    },
    {
      assetName: "RVN",
      satoshis: 2999000000,
      txid: "38965508c62ea2c0185e5e1f46ece8407fd58269127653f23cc341ead722a4bc",
      index: 1,
      blockindex: 1,
      height: 910448,
      address: "mswt769EVra9T8z5TdNM6hKtvvffjSgvvs",
    },
  ];
}

```

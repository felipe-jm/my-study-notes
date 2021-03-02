# Blockchain

> Created to urderstand some blockchain concepts.

<!-- TOC -->

- [Blockchain](#blockchain)
  - [Transaction](#transaction)
  - [Block](#block)
  - [Chain](#chain)
  - [Wallet](#wallet)
  - [Hash](#hash)
  - [Proof of Work](#proof-of-work)

<!-- /TOC -->

## Transaction

Fundamental purpose of any crypto currencry, to transfer funds from one to another in a transaction.

## Block

Like a container for multiple transactions, an element in an array, but more like a linked list, because each block has a reference to the previous block in the chain with the previous hash proprierty.

## Chain

It is like a linked list of blocks.  There should be only one instance of a Blockchain.

## Wallet

Just like a real wallet, that's where your cash is. It must have a public key and a private key, the public key if for receiving money and the private key is for spending money.

## Hash

A encrypted value that cannot be decrypted. But it is possible to validate that two values are identical by comparing their hashes, that is really important for blockchain, because it ensure two blocks can be linked together without being manipulated.

Creates a hash using **crypto** from nodejs:

```ts
const str = JSON.stringify(block);
// SHA256 is a secure hash algorithm created by NSA in 2001.
// It is a one way cryptographic function.
const hash = crypto.createHash("SHA256");
hash.update(str).end();
console.log(hash.digest("hex"));
// ae793d826533d3fd92533cb0856d96d3c0be182d12a18f248d7ade91b232c1b1
```

## Proof of Work

Each new block go to a through a process called mining, where a difficult computational problem is solved in order to confirm the block, but it is easy to verify that work.

Around the world, multiple computers are competing to confirm blocks on the blockchain, when confirmed, the miner wins a portion of coin as incentive.

---

[index.ts execution](index.ts)
```ts
yarn start
yarn run v1.22.10
$ tsc && node ./dist
⛏️ mining...
Solved: 94290
⛏️ mining...
Solved: 97310
⛏️ mining...
Solved: 9654
Chain {
  chain: [
    Block {
      prevHash: null,
      transaction: [Transaction],
      ts: 1614642722327,
      nonce: 753369079
    },
    Block {
      prevHash: 'ae793d826533d3fd92533cb0856d96d3c0be182d12a18f248d7ade91b232c1b1',
      transaction: [Transaction],
      ts: 1614642722542,
      nonce: 539839906
    },
    Block {
      prevHash: '3e465944a4973a62a0dc494b7c9cbf20c9b6dfd9c6c4276bdf11bada22b0cdbe',
      transaction: [Transaction],
      ts: 1614642723205,
      nonce: 342873467
    },
    Block {
      prevHash: '496540171eaaccd022e9150e52234ca5329b0aaea9def6923346ab3bbdba20af',
      transaction: [Transaction],
      ts: 1614642723732,
      nonce: 105602953
    }
  ]
}
```
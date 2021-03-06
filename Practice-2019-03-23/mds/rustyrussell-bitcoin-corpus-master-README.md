# Bitcoin Transaction Corpus #

This is a collection of bitcoind mempools, taken from running 4 full
bitcoin nodes with a patch to annotate transactions.  It's licensed
under BSD-MIT.

## How To Decode It ##

Each dump is a little-endian binary format, compressed with XZ.

Each record either reflects a new TX we accepted into the mempool from
the network, OR the relative state of the mempool when a new block
came in:

[4-byte:timestamp] [1 byte:type] [3-byte:block number] [32-byte:txid]

Where type is:
  1. INCOMING_TX: a new transaction added to the mempool, block number is 0.
  2. COINBASE: a coinbase transaction (ie. we didn't know this one)
  3. UNKNOWN: a transaction was in the new block, and we didn't know about it.
  4. KNOWN: a transaction was in the new block, and our mempool.
  5. MEMPOOL_ONLY: a transaction was in the mempool, but not the block.

The ordering is:
  1. Zero or more INCOMING_TX.
  2. A COINBASE tx.
  3. Zero or more UNKNOWN and KNOWN txs, in any order.
  4. Zero or more MEMPOOL_ONLY txs.

You can simply uncompress the corpora and load them directly into C
arrays.  See example/simple-analysis.c.

## Using the Data ##

You will usually need a full bitcoind (ie. with txindex) to actually
get the transaction contents from the hash (ie. bitcoin-cli
getrawtransaction).  To save space, they're not included here.

It takes about a day for the memory pool to reach steady state so
usually you will want to use the data from block 352305.

There are four orphaned blocks in the data set:

1) Blockheight 352560
   Coinbase 33db9755662f6b4a46dfe26a1d65ba00c4e1a2a9f1db190e711c61e4bcd060d7
      This is only in the sf-rn dataset.
2) Blockheight 352802
   Coinbase 79b1c309ab8ab92bca4d07508e0f596f872f66c6db4d3667133a37172055e97b
      This is in the sf-rn, sf, and sg datasets.
3) Blockheight 352548
   Coinbase a01b5e45d3624bc0265fb8ab81bb996bf4ffd46ddde45e083fc73e334e776e0d
      This is only in the sf dataset.
4) Blockheight 353014
   Coinbase 830178aa3b5bfffa0d8e2dc39def5d3e99029ca50aafc5c172e4556f9ac46d1e
       This is in the sf-rn and au datasets.

In addition, six transactions vanish from the sf-rn and au mempools
after the non-orphan 353014: presumably because they spent
transactions in the orphan.  If you are trying to track mempools, you
should remove the following after the non-orphan 353014 (you can
remove them unconditionally, since they don't appear on the other
nodes):

  037b4eadf63584764870bb055a2a8e755145e27a97490f46ede6db841114e14e
  2dbb81d73a6d9de85f2b78bcac7e350407a3e737f8d5da7ef1fe9338795fb0cc
  989897cac0115001c75d4913ca081b6263de487d20f70b03fcfa1b469144ad39
  82f5569c49461bbf1b67a1c7ba09c3a051292f86a5f97f7198ef364436e16dc0
  0ccd98b6940795add4bd221eac95de8f8dd92c0f2f5733032d3672d531a5d02a
  a26ba9c7ebeaebaeb98783c5133e55febc75e4e28945a886c4bc1a94a7cd9260

## How It Was Collected ##

The patch (collection/bitcoin-core-patch.diff) was applied to the
bitcoin source.  All nodes were variants of the 0.10 pre-release,
based on the autoprune pull request (commit
386039510b56ba7a224d009e5deb53f0f5b12274 Author: Alex Morcos
<morcos@chaincode.com>).  The coinbases were identified separately
using a hacky shell script on a full txindex node (see
collection/coinbases).

The nodes were:
	sf: Digital Ocean server in San Francisco
	sg: Digital Ocean server in Singapore
	au: My scratch box in Adelaide, Australia, behind a wireless network and
		NATed (twice).
	sf-rn: Digital Ocean server in San Francisco with RelayNode running.

The intent was to reflect any change in behaviour between a machine
behind a remote connection (au) and a well connected machine (sf-rn).

All nodes were re-started around 1429062669
(2015-04-15T01:51:09+0000), and ran for eight days.  The results were
converted to binary using collection/encode-dump.c.

## Questions ##

Please file a github issue, or email me at rusty@rustcorp.com.au.


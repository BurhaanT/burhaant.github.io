---
layout: post
title: Blockchain - Chaining-the-blocks
date: 2017-12-08 14:50
author: burhaan
comments: true
categories: [blockchain, Uncategorized]
---

Leading on from <a href="http://burhaantargett.tech/2017/11/30/a-foray-into-the-chain-of-blocks-the-human-element/"><b>my previous post</b></a>, no blockchain topic would be complete without the obligatory explanation of how blockchain works at a high-level.

I say high-level because the intricacies of how things like consensus algorithms work, how they address Byzantine Fault Tolerance (BFT), how nodes 'speak' to each other, what the purpose of a nonce is, etc. will come later in my journey.

Ideally, I would like to take the 'this is how we create something' approach but that wouldn't be much of a journey and some of the boring stuff is actually needed to build a foundation we can work on.

So, here we go.

One of the fundamental concepts when talking about blockchain is that of a distributed ledger (more on that later). The terms distributed and decentralised are often used interchangeably and thought to apply to the same thing - I assure you they are not the same.

The difference is probably best described by the image below.

<b><a href="http://burhaantargett.tech/wp-content/uploads/2017/12/network-differences.jpg"><img class="wp-image-340 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/network-differences-300x173.jpg" alt="" width="505" height="291" /></a></b>

As you can see, centralised is, as the name implies, where a single node controls the entire network. A failure at this node would incapacitate the entire network.

In a decentralised network, there is a greater degree of redundancy and fault tolerance however, faults at critical nodes would cripple substantial parts of the network and, potentially, orphan nodes.

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/network-failure.png"><img class="wp-image-341 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/network-failure-300x173.png" alt="" width="472" height="272" /></a>

Blockchain's resilience comes from the underlying implementation of a distributed architecture. In this topology, a node is able to operate as a discrete unit. Each node has it's own copy of the ledger and is not dependant on any other node(s) on the network.  A failure at a node would only affect that node and no other portion of the network.

We now have an abstracted understanding of the topology of the network - great. In this post, I've mentioned this thing called a (distributed) ledger. Lets dig into that a little.

<a href="https://en.wikipedia.org/wiki/Ledger#Overview">Wikipedia</a> says a ledger is "...a permanent summary of all amounts entered in supporting journals which list individual transactions..."

And that's the same as a blockchain ledger:

Assume we have a deposit of \$100 , this is recorded in our ledger and looks like this

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-1.png"><img class="size-medium wp-image-342 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-1-300x39.png" alt="" width="300" height="39" /></a>

We then continue to append more transactions

<ul>
 	<li>A deposit of $125</li>
 	<li>A withdrawal of $200; and</li>
 	<li>A deposit of $25</li>
</ul>
Eventually, our ledger look like this

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-2.png"><img class="size-medium wp-image-346 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-2-300x185.png" alt="" width="300" height="185" /></a>

In a blockchain implementation (usually after an amount of time), all these transactions are grouped together into a block and the transactions are 'hashed'. Think of a hash as a fingerprint that uniquely identifies some data. Any change in the data (no matter how small) will dramatically affect what the hash looks like. So our hash uniquely identifies our block of transaction any may look like this

<b><a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-hashed.png"><img class="size-medium wp-image-348 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-hashed-300x231.png" alt="" width="300" height="231" /></a></b>

That hash then becomes an entry in the next block of transactions and this value is taken into account when generating a hash for that block. To understand this better, take a look at the illustration below.

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-2-block.png"><img class="wp-image-347 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-2-block-300x160.png" alt="" width="454" height="242" /></a>This process continues on with each blocks hash becoming the 'seed' for the next block and we end up with something looking like this

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-multiple-blocks.png"><img class="wp-image-349 aligncenter" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transactions-multiple-blocks-300x197.png" alt="" width="480" height="316" /></a>

We end up with a chain of blocks - hence the term 'block-chain'.

You may be wondering what the point of this is. Well, lets take a look:

Imagine, if we tried altering a transaction somewhere within our chain

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-alter.png"><img class="alignnone wp-image-343" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-alter-300x197.png" alt="" width="458" height="301" /></a>

The hash for that block, now, would not match the original hash for that block. Further, because that hash is used as the seed for subsequent blocks, none of the subsequent blocks would be correct.

<a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-block-invalidate.png"><img class="alignnone wp-image-344" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-block-invalidate-300x197.png" alt="" width="463" height="304" /></a>

If a transaction was to be altered, not only would that blocks hash need to be altered but, also, every subsequent block.

Computationally, that's a very difficult task.

However, that's not all that lends immutability to a blockchain. Not only would you have to alter every block on that ledger, you would need to alter that ledger on every node in the network.

<b><a href="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-node-network.png"><img class="alignnone wp-image-345" src="http://burhaantargett.tech/wp-content/uploads/2017/12/transaction-node-network-300x174.png" alt="" width="631" height="366" /></a></b>

This is the incorruptible aspect of blockchain and this is what gives us certainty that what we see is really what transpired. Furthermore, the details of these transactions (in a public, unpermissioned, blockchain implementation) are transparent.

We've now established what a block is, how they are chained together and why blockchain lends itself to immutability, incorruptibility and resiliency.

We know we can't alter transactions already committed to the chain but, at this point it would be logical to question the validity of transactions appended to the blockchain. If any node can add transactions, how do we validate and verify that the transactions SHOULD be added to the chain. This is where consensus comes into play and there are a few different ways to reach this consensus. I'll discuss some of these in my next post.

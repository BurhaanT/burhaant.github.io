---
layout: post
title: So, you think you know Consensus Algorithms?
date: 2018-03-22 10:04
author: burhaan
comments: true
categories: [blockchain, Uncategorized]
---

When I started writing this post, the first thing I did was start diving into the different, and most common, consensus algorithms. Sometime during my first paragraph, it occurred to me that I may be taking for granted that anyone actually understands, fundamentally, what that even means. Perhaps I've got it wrong too. So lets start at the beginning...

<strong>What is an algorithm.</strong>

<a href="https://en.wikipedia.org/wiki/Algorithm"> Wikipedia</a> has a rather large entry on this (I was surprised at how in depth it was). Essentially, an algorithm is <em>"a set of rules that precisely defines a sequence of operations"</em>. Right, so the rules are a formula that defines how a task is accomplished. The <b>steps</b> you follow when making a cup of coffee is an algorithm. If you want to know more about algorithms, <a href="https://goo.gl/QHVyg1">here's a great video</a> to watch.

But for now, we have enough to go on.

The next question you may ask is:<strong> What's a consensus?</strong>

The best definition I've found is that of <a href="http://www.wordcentral.com/cgi-bin/student?book=Student&amp;va=consensus">Merriam-Webster</a>: <em>"It is  the judgment arrived at by most of those concerned"</em>

We now know that a <b>consensus algorithm </b>is a set of steps that are followed (an algorithm) so that an agreement can be formed by <b>most </b>(or all) of the parties involved.

Alright, but what are these <b>consensus algorithms</b> agreeing to?

They need to agree whether a block should be added to the blockchain. If you're not sure what that is, take a look at <a href="/2017-12-08-blockchain-chaining-the-blocks/">my previous post</a>.

With regard to blockchain, various technologies use different algorithms to arrive at this outcome. This post will cover the most popular ones.

Alright, <b>so why should you care?</b> As long as it works, does it really matter? If smarter people say it's okay, it must be okay! So set it up, run it and Bob's your uncle.

Not quite. There are many factors to consider when looking at consensus algorithms, some of them are:

<ul>
 	<li>Algorithms may be unreliable or fault intolerant and may crash under some circumstances - see  <a href="https://en.wikipedia.org/wiki/Byzantine_fault_tolerance">byzantine fault tolerance</a>.</li>
 	<li>They may take a really long time to process - not great if you're expecting something to be done in 5 minutes but it takes 5 months.</li>
 	<li>Some consensus algorithms are compute intensive and may require enormous amounts of power to run. Think electricity costs; think the environment - Did you know that <b>in the U.S, 24.25 households could be powered for 1 day by the electricity consumed by BitCoin for a single transaction</b>.</li>
</ul>
Usually, a blockchain platform implements a specific type of consensus algorithm. Selecting this platform also means that you're adopting the consensus algorithm. Better to be informed I say!

In this post I'll cover the most common consensus algorithms you're likely to encounter.

<b>Proof-of-Work</b>

This algorithm has been popularised because of it's use by BitCoin.

Proof of work is a requirement to solve a complicated math problem, this is called mining. 'Mining' is used to add a block to the chain. Given this, miners compete against each other to solve a blocks' problem. Whenever a miner wins, it is rewarded (e.g. BitCoin).

To understand how this algorithm works, we need a basic understanding of what a hash is and how its generated. So here goes.

A hash is a mathematical algorithm that converts a sequence of characters into a string of 64 letters and numbers. This process is so sensitive that even the smallest change in those characters will result in a completely different hash.

There are different types of hashing algorithms but, BitCoin uses the SHA256 algorithm and we'll use that for this example.

If we took my name, and hashed it:

burhaan = 172a0a60b8097a65c789e101c89c7d33e4344ba92926e7850de2703725e00d3e

However, if we changed the initial letter to uppercase:

Burhaan = fd1127f54ced787632f16f94f3f3c02cedd4d1bdffee8a217250f3915148407b

Any amount of text can be hashed. For instance, the hash value of the first paragraph of this post is:

344b06ccbd10f95095dd7c41993551e58a6879c91857b796328579b4f0b1a52d

Remember that an algorithm is a set of "rules". One of the rules for the proof-of-work consensus algorithm states that the <i>expected</i> time to mine a block (i.e. solving the math problem) be a certain amount of time (in the case of Bitcoin, this is 10 minutes, Ethereum's is 10-19 seconds). This rule determines the difficulty and the measurement of this is performed by taking the average time taken to mine <b><i>n</i></b> number of blocks. If the <i>average</i> time is above the <i>expected</i> time, the difficulty is reduced. If the <i>average </i>time is below the <i>expected</i> time, the difficulty is increased.

Difficulty is moderated by specifying the number of zeroes required at the beginning of the hash and more zeroes increase the difficulty of solving the mathematical problem.

Computing a hash value is best demonstrated through a simple example:

Using the example of my name, we need convert something like this:

fd1127f54ced787632f16f94f3f3c02cedd4d1bdffee8a217250f3915148407b

Into something like this: (where 1 zero is required)

0{the rest of the hash}

However, we're not allowed to alter the existing string i.e. "Burhaan". This is where a <em>nonce</em> fits in.

A <em>nonce</em> is an arbitrary, 32-bit, number that's appended to our string. This resultant string is then hashed and if it meets the rule (of the specified number of zeroes), it's calculated a successful hash and wins.

To illustrate:

Burhaandd34 = e8114f79a388a15328280d43c878313b693cf9f91e9105e46f238d8356d2b34f

That doesn't meet our rule, so lets try again:

Burhaan1234 = ca93ecf5fa607fb4e5ab72aed056568a1d08ac05479676c28346050a4bea5910

...and so we can go until we find a suitable hashed value that meets the criteria. Effectively, it's like playing the lottery, and any guess may work.

The number of times (per second) this 'guess' is attempted is the hashrate.

The steps (i.e. algorithm) that proof-of-work uses to reach consensus is as follows:

<span style="text-decoration: underline;">Step 1</span>:  Get the hash of the previous block.

<span style="text-decoration: underline;">Step 2</span>: Append the data in the current block to that hash.

<span style="text-decoration: underline;">Step 3</span>: Append a nonce to all the data from Step 1 and Step 2.

<span style="text-decoration: underline;">Step 4</span>: Hash.

<span style="text-decoration: underline;">Step 5</span>: Do we have a hash that meets the hash requirement? If not, go to Step 3. Otherwise <b>SUCCESS</b>, get a reward! and start of the next block

And that folks, is how Proof-of-Work, works.

<b>Proof-of-Stake</b>

Proof-Of-Stake is also an algorithm and has been touted as the salvation to energy hungry Proof-of-Work algorithm. Unlike Proof-of-Work however, where miners who solve mathematical problems are rewarded, Proof-of-Stake miners are chosen in a deterministic way based on various random selection mechanisms, for example <b>wealth </b>or<b> age</b> - i.e. their <b>stake</b>. There is no reward for validating (i.e. mining) a block so, <i>miners</i> in Proof-Of-Stake are instead called <i>forgers</i> and their participation on the network is relative to their <b>stake</b>.

As you can imagine, there may be some resultant centralisation in the implementation of these deterministic mechanics, as the rich would get richer and some may have a permanent advantage. There are some proposed options to deal with this and you can read about those in <a href="https://en.wikipedia.org/wiki/Proof-of-stake#Block_selection_variants">this Wikipedia article</a>.

Of course, not having to consume as much power as Proof-Of-Work introduced other problems, most notably the case of <i>forgers</i> participating on multiple blockchain histories and preventing a consensus from ever resolving. That is, if I (as a forger) can participate on multiple histories, I can act against myself and stall the entire algorithm.

One of the solutions proposed by Ethereum (who hasn't implemented this consensus algorithm at the time of this post) is that of the Casper protocol, which forces each forger to provide a deposit for each of it's 'mining' activities on the network. If it is determined that the forger acted in a malicious way, it would lose its deposit.

<b>Byzantine Fault Tolerance (BFT) </b>

When dealing with distributed computing, a problem that needs to be considered is explained by the <a href="https://web.archive.org/web/20170205142845/http://lamport.azurewebsites.net/pubs/byz.pdf">Byzantine Generals problem</a>. Effectively, some generals are laying siege to a city and can win if they all act in unison. That is, if they all attack, they win; if only some attack, they lose. The generals rely on messages to be sent between each other to communicate however, a rogue general sends mixed or arbitrary messages. Telling some generals to attack and some to withdraw.

In computer science, this problem can occur when a computer (or group of them) operates incorrectly or maliciously and sometimes sends the wrong message.

In a consensus driven, distributed compute, environment this can be a significant problem and there are a number of protocols around to address this. One of more popular applications is the Practical Byzantine Fault Tolerance algorithm.

<span style="text-decoration: underline;">Practical Byzantine Fault Tolerance (PBFT)</span>

Daniel Feichtinger, co-founder of <a href="https://www.hyperledger.org/">Hyperledger, </a>that utilizes PBFT, explains how its distributed system works:

<i>"Each node publishes a public key. Any message coming through the node is signed by the node to verify its format. Once enough responses that are identical are reached, then you can agree that is a valid transaction."</i>

The PBFT algorithm provides safety when, at most, <em>(n-1)/3</em> out of a total of <i>n</i> nodes are faulty. In other words, the network can tolerate a maximum of 33% of the nodes acting in a malicious manner.

The original whitepaper by Miguel Castro and Barbara Liskov, that describes the algorithm in detail, can be found <a href="http://pmg.csail.mit.edu/papers/osdi99.pdf">here</a>.

<b>Other Consensus Mechanisms</b>

At this stage, I need to draw a line - It's easy to disappear down a rabbit hole given the number of different algorithms. However, I need to point out that your selection of a platform needs to take into account the purpose of the application and what your requirements are. For example, if you need fast(er) throughput, you probably don't want to consider anything that uses proof-of-work.

&nbsp;

So what some of the considerations you can use to whittle down the options available to you:

<ul>
 	<li><b>Throughput</b> - How many transactions need to be processed within a timeframe.</li>
 	<li><b>Cost</b> - Both in terms of implementation and transaction costs.</li>
 	<li><b>Energy</b> - Does the implementation need to be energy efficient?</li>
 	<li><b>Scale</b> - Can the network scale to your requirements efficiently?</li>
 	<li><b>Customisation</b> - Do you have a requirement to customise the incentive or other aspects of the network?</li>
 	<li><b>Limitations</b> - Are the nodes required to be trusted?</li>
 	<li><b>Infrastructure</b> - Do you need to have a permissioned or public network? And do you know who the nodes are?</li>
</ul>
&nbsp;

&nbsp;

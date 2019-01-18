---
layout: post
title: Creating a simple DApp for Ethereum using Truffle and Ganache
date: 2018-02-09 08:36
author: burhaan
comments: true
categories: [blockchain, ethereum, ganache, solidity, truffle, Uncategorized]
---

I find that I'm enjoying the theory behind blockchain and am tempted to continue along the path of exploring the different implementations, consensus algorithms, and pros and cons.
However, now that there's a basic understanding of why we would/should look at it, and some fundamental knowledge of how it works, it seemed a good idea to actually build something - I've heard many people complain about the amount of theory they've seen but how little they've seen in the way of anything having been developed.

In that spirit, I've come up with a contrived (but practical) example that I will implement:
I would like to provide proof that a vehicle I drive actually belongs to me and that the history of that vehicles' ownership can be traced.

I'm opting to develop this for Ethereum, but the same thing could be done on Quorum or Fabric, among others. In future posts, I'll explore these platforms too - so stay tuned.

The source code for this application can be found <a href="https://github.com/BurhaanT/examples-ethereum-vehicle-ownership" target="_blank" rel="noopener">here.</a>

Firstly, ensure that you have some prerequisites setup: I'll be using the <a href="http://truffleframework.com/" target="_blank" rel="noopener">Truffle Framework</a> - I like the scaffolding it provides and it makes my life as a developer easier with contract compilation, automated testing and scriptable deployments and migrations.
My test net will be using <a href="http://truffleframework.com/ganache/" target="_blank" rel="noopener">Ganache</a>.
<a href="https://code.visualstudio.com/" target="_blank" rel="noopener">Visual Studio Code</a> will be used as my IDE along with the <a href="https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity" target="_blank" rel="noopener">Solidity Language Extension</a>.

Once you have that all setup, lets begin.

I want to store some details of the vehicle:

<ul>
 	<li>What the owner's address is (the address, in this context, is the blockchain address)</li>
 	<li>The owners name</li>
 	<li>Registration</li>
 	<li>VIN</li>
 	<li>Make; and</li>
 	<li>Model</li>
</ul>
In my "contracts" folder I'll create a contract called <strong>Vehicle.sol </strong>and, similar to a usual class, define my properties.
Simplistically, consider a contract as the template that defines what information you want stored on the blockchain and how you want to interact with that information.

A vehicle needs an owner on creation so who ever creates the vehicle, the first time, will be the owner and I only ever want the owner to have the ability to transfer that vehicle to a purchaser - I've used the modifier for this purpose - in that case it check that the entity executing the function is the owner.

Below is what my contract looks like.

<a href="/img/content/vehicle_contract.sol_.png"><img class="alignnone wp-image-370 size-large" src="/img/content/vehicle_contract.sol_-995x1024.png" alt="" width="474" height="488" /></a>

The very first line (<strong>pragma solidity....</strong>) specifies the minimum version of solidity that's required to compile this contract. This will prevent future compiler versions from compiling the contract where there may be incompatible changes.

You'll notice functions to:
<em>create a vehicle</em> - This will be used when I create a vehicle and will set the values for some properties, for example, who the owner is.
<em>change the owner</em> - in particular, when the vehicle is sold, ownership transfers to the purchaser.

I've also got a <strong>strConcat</strong> function which is the best implementation I've come across (so far) to handle string concatenations in solidity.

Ensure Ganache is running then switch over to terminal and run <strong>truffle compile</strong>.

Once our contract has compiled we can now deploy it to our Ganache testnet by running this <strong>truffle migrate</strong>.

This gives us the following output:

<a href="/img/content/out_1.png"><img class="alignnone wp-image-361 size-large" src="/img/content/out_1-1024x248.png" alt="" width="474" height="115" /></a>

It may be the case where you receive an error like this

<a href="/img/content/out_2.png"><img class="alignnone wp-image-362 size-large" src="/img/content/out_2-1024x221.png" alt="" width="474" height="102" /></a>

In this case, truffle was unable to detect the network you're attempting to deploy to. All that's required to correct this is the addition of network settings to <strong>truffle.js</strong>

<a href="/img/content/host_settings.png"><img class="alignnone wp-image-360 size-full" src="/img/content/host_settings.png" alt="" width="688" height="322" /></a>

More configuration information can be found <a href="http://truffleframework.com/docs/advanced/configuration" target="_blank" rel="noopener">here</a>.

If you look at the output, you'll notice that the migrations were run, but....
There's no mention of the contract just created!

What we're missing is the instruction to deploy the <strong>Vehicle</strong> contract. To do this, we need to create a migration. This migration will then, automatically, be executed when calling <strong>truffle migrate</strong>.
The migration files need to be numbered sequentially - when looking at the project, you'll notice there's already a <strong>1_initial_migration.js </strong>file. We will add our own and call it <strong>2_vehicle_contract.js</strong>.

The migration code is relatively simple and looks as follows:

<a href="/img/content/vehicle_migration.png"><img class="alignnone wp-image-371 size-full" src="/img/content/vehicle_migration.png" alt="" width="873" height="312" /></a>

It is important to note that '<strong>require</strong>' needs to provide the actual name of the contract and not the file name, as a file can contain multiple contracts.

Once you've got this in place, running <strong>truffle migrate</strong> migrates this contract and the output look like this:

<a href="/img/content/out_3.png"><img class="alignnone wp-image-363 size-large" src="/img/content/out_3-1024x463.png" alt="" width="474" height="214" /></a>

If you switch to Ganache, you'll notice that the current block has advanced and taking a look at "transactions" reveals our contract deployments.

<a href="/img/content/Ganache.png"><img class="alignnone wp-image-357 size-large" src="/img/content/Ganache-1024x557.png" alt="" width="474" height="258" /></a>

I'm going to use <a href="http://truffleframework.com/docs/getting_started/console" target="_blank" rel="noopener">truffle console</a> to interact with my contract - Truffle console is a basic interactive console that can connect to any Ethereum client.
I've opted to use console because I already have a network deployed in the form of Ganache - if this wasn't the case, I would use<a href="http://truffleframework.com/docs/getting_started/console#using-truffle-develop-and-the-console" target="_blank" rel="noopener"> truffle develop</a>.

Navigating to the path of our project using terminal, execute <strong>truffle console</strong>. This spins up the interactive console.

The first thing we need to do is determine where the contract is (i.e. the address of my contract). Using truffle, this is easily done by running <strong>truffle networks</strong> from within the truffle console.
This lists the contracts I have and their addresses.

<a href="/img/content/out_4.png"><img class="alignnone wp-image-364 size-full" src="/img/content/out_4.png" alt="" width="824" height="153" /></a>

Using the address we can then call our function <strong>createVehicle(...)</strong> like this:

<a href="/img/content/out_5.png"><img class="alignnone wp-image-365 size-large" src="/img/content/out_5-1024x212.png" alt="" width="474" height="98" /></a>

Lets now discover who the owner is by calling the <strong>OwnerName</strong> property.

<a href="/img/content/owner_name_1.png"><img class="alignnone wp-image-369 size-large" src="/img/content/owner_name_1-1024x52.png" alt="" width="474" height="24" /></a>

We can also see the owners address.

<a href="/img/content/owner_address_1.png"><img class="alignnone wp-image-367 size-large" src="/img/content/owner_address_1-1024x52.png" alt="" width="474" height="24" /></a>

Lets now sell our vehicle to a purchaser. In this contrived example, running on a test network, that's going to be enacted by the owner transferring the vehicle to someone else. Recall, that only the owner can execute the <strong>changeOwner</strong> function.

I'm going to select a random address from Ganache (that's not currently the owner) and use that as the new owner.

<a href="/img/content/out_6.png"><img class="alignnone wp-image-366 size-large" src="/img/content/out_6-1024x330.png" alt="" width="474" height="153" /></a>

Let's check the new owner and the address.

<a href="/img/content/owner_and_address.png"><img class="alignnone wp-image-368 size-large" src="/img/content/owner_and_address-1024x100.png" alt="" width="474" height="46" /></a>

So our contract works and we can at any time, determine who the owner is or look at any of the other properties.

You would have noticed that there was an event recorded in the code, and looking at the output from the function call after changing owners, there was an "event" property that alluded to the code tying up with this property. Let dig into that event to see what's recorded.

<a href="/img/content/get_logs.png"><img class="alignnone size-large wp-image-359" src="/img/content/get_logs-1024x54.png" alt="" width="474" height="25" /></a>

We do this by first getting all the events from the beginning of time then outputting those events

We can then see out event - <strong>Owner Changed -</strong> is the last entry in the logs.

<a href="/img/content/get_all_events.png"><img class="alignnone size-large wp-image-358" src="/img/content/get_all_events-1024x844.png" alt="" width="474" height="391" /></a>

Be aware, that typically, this is not how you handle events. Yes, it can be used to store some information like you see here. However, you would usually have other DApps listening for and responding to these events.
Also, in a production environment, be cautious of querying events in this way as it has been reported to (sporadically) crash your JSONRPC endpoint.
Read more <a href="https://github.com/ethereum/wiki/wiki/JavaScript-API#contract-allevents" target="_blank" rel="noopener">here</a>.

So folks, there you have it, a really simple Distributed Application (DApp) that stores some data and queries it.

In the next post I'll dig a deeper into consensus algorithms; why we should care what they are and what they mean to developers.

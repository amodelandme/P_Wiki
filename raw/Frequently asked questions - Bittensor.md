---
title: "Frequently asked questions (FAQ)"
source: "https://docs.learnbittensor.org/resources/questions-and-answers"
author:
published:
created: 2026-05-13
description: "General"
tags:
  - "clippings"
---
## General

### Is Bittensor a blockchain or an AI platform?

It is both!

Bittensor is a platform for the production of digital commodities, including AI inference, training, and infrastructure, as well as others unrelated to artificial intelligence.

Bittensor is backed by its own substrate blockchain, Subtensor. The distributed ledger of the Bittensor main network serves as the system of record, and TAO, Bittensor's cryptocurrency token, serves to incentivize activity across the platform.

### So what is a subnet?

A subnet is a community that produces a digital commodity in a competitive market, with Bittensor keeping track of and incentivizing the activities required for this production.

Anyone with the funds and technical know-how can create a subnet, or participate in an existing subnet.

### How does competition work in a subnet?

The work to be performed by miners is set by the subnet creator in the form of the subnet's incentive mechanism. The miners compete to best perform the task, submitting their work to the validators.

The validators then rank the quality of the work done by the miners within the subnet. The aggregated scores of the validators determine the quantity of TAO emitted to each miner.

At the same time, validators are also incentivized to do their best work, because their emissions depend on how well their miner scorings agree with the general consensus of other validators.

### What exactly is the task of a subnet miner?

The task of miners is different in each subnet. Some subnets provide AI services like specialized inference, training, or prediction. Other provide infrastructure as a service, including storage or compute.

Browse tokenomic information about the subnets on [TAO.app](https://tao.app/), and learn more about the projects and services they support on the [Learnbittensor.org subnet listings](https://learnbittensor.org/subnets).

### So where does the blockchain come in?

The blockchain records all the key activity of the subnets in its ledger. It also continuously runs an algorithm called Yuma Consensus (YC). YC takes in rankings of the subnet's miners by the subnet's validators, and computes the TAO emitted to miners, validators, stakers, and subnet creators.

### Do subnets talk to each other?

A new abstract base class, called `SubnetsAPI` is released in Bittensor `6.8.0` and your application can use this to enable cross subnet communication. Normally, however, if you are not using the `SubnetsAPI`, then the subtensor blockchain does not mix data from one subnet with another subnet data and a subnet does not communicate with another subnet.

> [!-success] -success
> See also
> 
> See [Bittensor Subnets API](https://github.com/opentensor/bittensor/blob/master/README.md#bittensor-subnets-api).

## Mining and validation

### Is mining and validation in Bittensor the same as in Bitcoin?

No, there are some key differences! Bitcoin miners work to validate blocks according to Proof of Work consensus so they can be added to the blockchain.

In Bittensor, "mining", within subnets, has nothing to do with adding blocks to the chain. Instead, it has to do with production of digital commodities. Similarly, "validation", within subnets, has nothing to do with validating blocks—it concerns validating the work performed by miners.

### So is there a separate blockchain validation on Bittensor?

Yes indeed. In Bittensor, the work of validating the blockchain is performed by the Opentensor Foundation on a Proof-of-Authority model.

### What is the incentive to be a miner or a validator, or create a subnet?

Bittensor incentivizes participation through emission of TAO. Each day, 3600 TAO are emitted into the network (0.5 TAO every 12 seconds).

The emission of TAO within each subnet is as follows:

- 18% to the subnet creator.
- 41% to validators
- 41% to the miners

See [Emissions](https://docs.learnbittensor.org/learn/emissions).

### I don't want to create a subnet, can I just be a miner or a validator?

Yes! Most participants will not create their own subnets, there are lots to choose from.

See:

- [Validating in Bittensor](https://docs.learnbittensor.org/validators)
- [Mining in Bittensor](https://docs.learnbittensor.org/miners).

### Is there a central place where I can see compute requirements for mining and validating for all subnets?

Unfortunately no. Subnets are not run or managed by Opentensor Foundation, and the landscape of subnets is constantly evolving.

Browse the subnets at [TAO.app](https://tao.app/), or on [Discord](https://discord.com/channels/799672011265015819/830068283314929684).

### Can I be a subnet miner or a subnet validator forever?

You can keep trying forever, but your success depends on your performance. Mining and validating in a subnet is competitive. If a miner or validator is one of the three lowest in the subnet, it may be de-registered at the end of the tempo, and have to register again.

See [miner deregistration](https://docs.learnbittensor.org/miners#miner-deregistration).

## Who maintains the Bittensor blockchain, software, and documentation?

Bittensor is an open-source project and is open to contributions from the community. Most of the core development on the Subtensor blockchain is done by engineers working for the [Opentensor Foundation](https://github.com/opentensor), a nonprofit organization. The Bittensor CLI (`btcli`), the Bittensor Python SDK, and this documentation, are developed by members of [Latent Holdings](https://latent.to/), a Bittensor development startup that also own maintains the blockchain explorer [tao.app](https://www.tao.app/), the Bittensor AI Assistant [Savant](https://tao.app/savant), and [Subnet 14: TAOHASH](https://taohash.com/).

The Head of Documentation for Bittensor is Michael 'Trexman' Trestman, who can be reached at [m@latent.to](mailto:m@latent.to), on [Github](https://github.com/MichaelTrestman), or [Discord](https://discord.com/users/1025598777425404006).
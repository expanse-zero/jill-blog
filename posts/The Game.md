# The Game

The distribution algorithm is the part that takes the orders and decides which order should be sent to what constellation if at all.

This is the heart of the constellation planning system. We assume that the orders are ones which some of the constellations can take, and the constellation simulations are somewhat similar to the real constellations.

The whole distribution problem is like an auction. The users supply orders, and the constellations compete to take them.

This can be viewed as a game where the whole system is a set of tenders and bids for all the orders. We need to make the game fair in order to reach good solutions - which are the outcomes of the turns. In addition, we need to make the whole game as realistic as possible in order to simulate the interests of all the participants in the game.

## The rules
### Phases
We can think of something like this:

#### PHASE 0 - preparation phase
All orders are gathered, each with some constraints. This pre-phase is also one where all the constellation agents (brokers) can check the orders, assess their access or gather-potential, and check the gain against the constellation cost (or amount of capacity taken).

#### PHASE 1 - The bidding phase
Each constellation player (broker) places a bid or set of bids on any orders he wishes. This is simply a token that he is interested in taking these orders.

In order for him not to try and take them all, for each order he bids on, he needs to submit a 'token' payment - out of his money.

#### PHASE 2 - Client update
The clients look at their orders, and who placed a bid on them. They can redistribute the bounty values according to their wishes. For instance they could lower the bounty on an order which has 2 or more bids, while rising orders that weren't taken.

Each client has some allowance - which can be added to each day for instance, and he cannot place a larger bounty than his allowance.

#### PHASE 3 - Division
[Phase 1](#PHASE-1---The-bidding-phase) and [phase 2](#PHASE-2---Client-update) can run until we reach a stable division, or until some other event - say a timeout.

Then we distribute the demands between all the brokers. Each broker gets back the bid price for the orders he did not receive in the end. (optional)

#### PHASE 4 - External update
We get data from outside whether there was an update to an order - for instance if it was already taken, or if parts of an order (or mission?) that we thought were taken haven't been covered in the end.

In addition, for each order taken, the constellation broker gets additional money that he can use to bid for more orders. The amount is the bounty decided upon for the order.

On the other hand, if he has failed to take the order - he must compensate the client. (Optional)

**I'm not sure if this should come before or after [phase 3](#PHASE-3---Division)**

#### PHASE 5 - New orders
Now new orders may be raised by the clients, and of course the bounty values updated.
Here we must take into account that some of the orders have been distributed already. If a client wants to update the value of one of those, he must pay a reimbursement fee to the constellation that has promised to take it.

This is actually [phase 0](#PHASE-0---preparation-phase), but with the addition that some orders have already been distributed.

#### PHASE 6 - commit
This phase is similar to [phase 3](#PHASE-3---Division), but it is real. Meaning that all orders are sent to the constellations, according to the division decided upon, and they are taken out of the order pool. The bounty placed on the orders is placed in the auctioneer-bank by the clients. It shall be given to the constellation broker if the constellation succeeds in taking it, and returned to the client if not.

### Game phases summary
The game is never-ending, and the phases can run asynchronously even. But assuming a  linear game the phases would be something like this:

- #PHASE 0 - preparation phase
- Do while:
	- Do while unstable:
		- #PHASE 1 - The bidding phase
		- #PHASE 2 - Client update
	- #PHASE 3 - Division
	- #PHASE 4 - External update
	- #PHASE 5 - New orders
	- If (time to commit):
		- #PHASE 6 - commit

The constellation agents use the simulations to assess cost and potential, while also taking into account the values of the orders, their wealth and expected rewards. In addition there will be things like specific contracts that we have with the constellations which constrain the agents how many orders they can take, where and so on.

The client agents need to take into account their budget, their potential future needs (which may be unknown) and of course what has been assigned already.

## Summary
All this makes a somewhat complicated game of interests where each side competes with the other players on his side, and seeks to maximize value. The mission of the 'game' rules is to make sure everyone is playing cricket, and not gaining advantages by crooked or even ungentlemanly behavior.

The advantage of treating it like an open market, is that it allows to conceptualize quite accurately what the interests of the constellations are, and how they and the clients would behave. This is especially true with the New Space constellations which are in fact private constellations seeking to maximize on profit. Even though they don't yet receive the real priority in cash, but we could imagine them changing their behavior in accordance with the free market rules.


---
- [TOC](../TOC.md)
- Previous post: [Assuming it works](./Assuming%20it%20works.md)
- Next post: [Pretend its a constellation](./Pretend%20its%20a%20constellation.md)

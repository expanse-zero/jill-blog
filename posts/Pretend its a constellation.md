# Pretend it's a Constellation
![Pretend its a city - Fran Lebowitz](../images/Pretend-Its-a-City-Fran-Lebowitz.jpg)


Constellation simulations are the linchpin of this whole system. The quality of the outcome depends almost solely on them. And they are probably going to be the most complicated and time-consuming part of the development and research.

But what are they? We'll explain here how they should behave and what they are supposed to provide us.

### :warning: TLDR :warning:
This post is really long. So if you just want to understand what we want from a constellation, just read the first paragraph under [Constellation Simulation](#constellation-simulation), and the [Summary](#summary).

## Constellation Simulation
In order to decide where to send what order, we need a way to assess the gathering potential of each constellation. Another way to look at it is to calculate the cost the constellation requires to pay in order to gather the order, or the group of orders.

There are additional functions we need from each constellation, and of course routes. It's important to remember that not all production and research needs to be undertaken by the constellations group, rather some can be given as requirements for a new constellation to be joined.

## Functionality
But first we need to deepen our understanding of how we treat a constellation.

### Access
This is a relatively simple object. It describes for each order, at what intervals it can be taken. Meaning, given a window, return an object that describes if it can be taken and when. For instance, we can use a time-interval to describe the access, or an array of time-intervals if there are, for example, several activity windows each with a different access interval.

The advantage of this function is that each order has an independent [access](#Access) that can be calculated and used regardless of all other orders. We won't easily see this feature in the following functionalities.

Another advantage of this functionality is that we have worked with it some time now, know what it looks like, and what it means:
> If a constellation has an access to some order to some moment in time, it means that if this were the only order sent to the constellation (assuming no competitors also from other users of the constellation), then the constellation would be able to cover the order at that point in time

This of course gets more complicated with more complex orders, but assuming a sub-order that requires a single scan then we can define this well.

### Potential-Cost-Capacity
These two terms are averse to each other, and I assume we'll be using both, either as opposite substitutes, or to mean different things.

The meanings of the terms are:
**Potential:** The capacity of orders a constellation can take.

**Cost:** The loss of potential required in order to take up an order or group of orders

Maybe we should also add:
**Capacity:** The amount of gathering some constellation can take

It's important to remember that often the capacity is regarding some finite area in space, and in time, whereas potential would more often refer to an order.

On the other hand, cost almost always refers to an order or group of orders, and the units are capacity.

We could describe it mathematically in the following manner:
$$
potential = \sum_{aw \in Constellation} C(apacity)
$$
$$
Cost[C]
$$
The potential is the sum of all the capacity in the constellation
The cost's units is in terms of capacity.

### Current plan
A plan (or planning) is a list of order-time-interval pairs, where each order is taken in the corresponding timeframe.
Asking for the current plan from a constellation, the constellation will return at the least what orders it is supposed to take at that moment. We would prefer to also have an understanding of whenabouts the order will be gathered, but this is of lesser importance to some confidence of what orders are planned.

Together with the plan, it is important to understand what potential that will allow us to send more orders to the constellation. Another way to ask it is how much the current plan costs the constellation.

### Summary
So the functionality we would like to have from our client constellations are:
1. [Accesses](#Access) which allow us to filter what orders can be sent to the constellation
2. [Potential-Cost-Capacity](#Potential-Cost-Capacity) which allows us to assess how many orders and in what areas we can send to the constellation
3. The [Current plan](#Current-plan) which tells us what the current state of the constellation is and gives us information on what to do next.

Together these will allow us to make informative decisions on up the orders between the constellations.

## Simulations
I wish we could get all the functionalities described in the [Functionality](#Functionality) section. But the problem is that often the constellations either aren't willing to share this information, or don't even know themselves.

For example, creating a good plan takes time, and depends on what orders have been sent. This means that assuming we are but one customer of some constellation, we might assume that there is some potential left, but after an hour that potential has been grabbed by a competing customer, and there might be no more potential left in the time-frame important to us.

Furthermore, the plan is created by the constellation handler, and he doesn't even divulge the plan, which means we don't know how he is going to take the orders we sent.

This means that there is a lot of guesswork to be done in this system.

One last major problem is the fact that most of the constellations are run on the WWW, while some are maintained within our secure network. This means that wherever we run our system, some of the constellations will not be connected to us, reducing the ability to get accurate data constantly.

Instead of being connected to the real constellations therefore, we'll need to **simulate** them instead. Meaning based on our partial knowledge of each constellation, we need to guess how the functionality we described above behaves. ^4f4fab

We'll run our algorithm against these simulations, and only once in a while will we be able to communicate with the ***real world*** and update our assumptions.

**There really is no alternative to running simulations of the constellations**

### Risks
So what are going to be the hard parts of creating these simulations?
- There are a large [Variety](#Variety) of constellations each with different capabilities, options and contracts
- When we want to simulate, we must take into account the different [Types of data to base on](#Types-of-data-to-base-on)
- [Generality](#Generality) of the simulation that will allow us to simulate multiple constellations with simplicity

### Variety
There are SAR constellations, EO, Large constellations and boutique ones. Each has differing capabilities, scan modes, with ranges of swath, agility and number of satellites.

Furthermore some constellations allow us to order more complex orders, for instance stereo, or large areas. While others need us to break down this orders into sub-orders.

Our simulation will need to replicate the capabilities of the real constellation, while allowing us to easily calculate capacity, assess cost and simulate a plan of the real constellation.

### Types of data to base on
In order to create a simulation, we need some real data to base it on. Otherwise we'll just be making vague guesses.

The problem is that different constellations give us different data. For instance some of them publish the activity-windows, others, it might be possible and yet others only from satellite-tracking organizations which makes everything much more difficult. We'll note that even if we know what the path of each satellite is, we don't know where the company opens an activity-window.

Other firms might allow us only access of orders, and we'll need to use those to understand what is possible and what isn't.

One assumption that might help us is that all the satellites, at the end of the day, work the same. They fly along given paths, roll around and scan the ground in sweeps or spots.

Another aspect that may help us is that assuming we have some history of working with the constellation, we can gather intel on the capabilities, and given some partial information, infer the potential more accurately according to our knowledge.

### Generality
Each time we add a constellation, we'll need to work hard on gathering the information to create a simulation. In order to make this more efficient and possibly nigh automatic, we need to make sure our simulations are general enough that they allow describing any constellation with minimal tweaking.

We would like to have a single simulation that, given a configuration that describes the constellation, can describe it well enough. In this manner we can simulate all constellation simply by adding more configuration files.

In order to create these configuration files that describe each constellation, research will need to be done or at the least data gathered which will allow the automatic tuning of the configuration.

### Input - Output
The constellation will be the ***advocate*** of the constellation during the planning phase. Its inputs will be:
- The accesses of all orders (This at least we should be able to get from the real constellations)
- The activity-windows or any other data that can support calculating the capacity of the constellation for the planning phase
- The current plans of the constellation
Its functionality - from the side of the planning algorithm - will be:
- Get accesses
- Get remaining potential/capacity in areas for time-intervals
- Calculate the cost of taking some set of orders
This last one can be used instead of a plan. It will allow us to understand potential remains to be used.

## Model ideas

### Capacity model
A capacity model of a constellation, means we assess how much capacity for orders the constellation has, this can be used to calculate the potential or remaining potential of a constellation given some set of promised orders.

Data we use for modelling:
**Activity windows:**
This requires the knowledge of the activity windows open in the constellation. Using the activity windows, we can assess what regions the constellation has potential in, and how much capacity it has in each region at each time.

**Statistical inference:**
Looking at the past, we can see what each constellation managed to gather, where it gathered and other statistics that define the capacity it had. Using this, in addition with some partial knowledge of the current state - whether AWs or other data - we can guess the capacity the constellation has in each region at each point in time.

### Dummy sat model
A dummy sat model means we take information about the satellite, and run a simulation of the sat's trajectory and maneuvers in order to take sent orders.

This requires both the activity windows, but also information about the satellites if possible, and if not then statistical information about the satellite's capabilities.

This will be more complicated since one constellation can have multiple types of satellites or varying generations with varying capabilities within the constellation, and unless we know what satellite was used we'll end up with an average of the whole constellation at best.

The advantage of this model is that it can create a more accurate picture of the capabilities of the constellation - going far further than a simple capacity model.

Another disadvantage is the fact that running this model will likely take up a lot of time during the planning phase.

---
- [TOC](../TOC.md)
- Previous post: [The Game](./The%20Game.md)
- Next post: [The known the unknown and the ugly](./The%20known%20the%20unknown%20and%20the%20ugly.md)

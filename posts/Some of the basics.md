# Woah
Before we rush into things, let's take a moment and talk some background...

## Some terms we need to understand
I don't want to go into too much detail here, since most of these things you can find on your own, so I'll just point to the relevant Wikipedia page and provide more details if needed.

### Observation Satellites
So these are pretty obvious - satellites that orbit earth and observe (take images) of the ground.

[go wiki!](https://wiki2.org/en/Earth_observation_satellite+Newton)

### Satellite constellation
When satellites are managed together, they form a constellation.

[All hail the font of wisdom](https://wiki2.org/en/Satellite_constellation+Newton)

### Users
These are, you know, the people that pay money in order that I have a salary, or is it just a life at this point?

Mainly we'll talk about users who aren't interested in how we get them what they want. They'll ask for stuff, and we'll try to fulfill them. users are the ones that create missions and hence orders... More on that in a sec.

[I mean, duh](https://wiki2.org/en/User_(computing)+Newton)

### Missions and orders
Finally some terminology.

A mission as a request of some user for information he needs covered. Often this is called a user demand. The are multiple types of missions, that can be explained with a story or whatever the product sold you. For instance:
> I want to remap the flow of all rivers in a county. Therefore I need a vertical scan of the county in such a way where I can identify water bodies of size above 5 meters in any direction.

From here we can form a mission of covering a large area at a certain resolution - say 2.5 meters per pixel or something.

Now that we have a mission, we need to be able to send it to the satellites. We might go into this more in depth later, but lets say that an order is a part of the mission that a satellite constellation can take. For instance, we can divide the county into several smaller areas which can be covered in a single scan by a satellite, and then send each of these segments as a separate order. Together they fulfil the [User's](#users) mission.

### Product
The product of an [observation satellite](#observation-satellite) is the image that returns following an order. This is what the user sees at the end.
[not exactly but it'll do](https://wiki2.org/en/Satellite_photo+Newton)

## Summary
So these are the main terms you'll need to continue making your way through this blog. Let's get this ride on the road.


---
- [TOC](../TOC.md)
- Previous post: [Some of the basics](./posts/Some%20of%20the%20basics.md)
- Next post: [Assuming it works](./posts/Assuming%20it%20works.md)

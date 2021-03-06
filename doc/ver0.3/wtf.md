# Swarm: real-time model sync library

## What is it?

It is a Swarm demo. Swarm is a JavaScript real-time model sync library. It synchronizes JavaScript objects for clients and servers alike. Swarm solves the M in MVC.

In the demo above, 9 onscreen iframes are connected to 9 different servers. The position of the big card suit sign is synchronized between iframes using the Swarm library. Try hovering a mouse over some iframe. The suit sign must follow.

It also syncronizes positions of other people's mouse pointers. In case somebody else is playing with the page right now, you should see their suit signs moving. Those are smaller.

Servers go offline randomly to test how other servers reconfigure the topology. Then they go back online pulling the state from other servers.

## Why did you make it?

With the spread of node.js and HTML5, server-side and client-side environments converge. The browser gets its own storage and a continuous two-way connection to the server. The server runs JavaScript. Often, the same codebase is conveniently used both at the server and at the client.

Still, the client and the server are mostly synchronized using HTTP request based APIs. That model is implicitly based on the historical "static website, dumb client" model. That model increasingly deverges from the reality. Without even mentioning real-time collaboration and updates, these days we may even need to synchronize various devices used by the same user.

Mapping the old model to the new reality is a really painful experience: fetch some model from the database in several steps of asynchronous logic, and then you need another one. Synchronizing multiple clients is even worse. Meanwhile, the problem is really basic and easy to formalize: just keep those objects synchronized.

So, we are scrathing our own itch. Or more precisely, putting bandages on our own wounds :)

## How does it work?

It uses WebSocket in the browser and node.js on the server. Servers talk to each other by WebSocket as well. Each object change is timestamped and versioned. Version identifiers are similar to those used by MongoDB and others. They are essentially Lamport timestamps.

Once a client is interested in some object, it subscribes to it at the server. The server talks to other server that is responsible for the object. We use consistent hashing to appoint the responsible server, although that is pluggable. Together, servers form a spanning tree to propagate updates efficiently. The tree is different for every object.

We assume that servers form a full mesh which can only be disrupted temporarily. As servers come and leave, the load is redustributed. Swarm gives no guarantees regarding transactions or persistence, but it can deliver the data to your backend. It is just a 1000-line JavaScript library, after all.

## Is it production-ready?

No.

## How does it differ from Meteor/Backbone/Firebase/Derby?

Meteor took the general approach of proxying MongoDB to the client side. We do it the other way around: we deliver JavaScript objects to and fro the backend.

Compared to Derby (and Meteor as well), Swarm is one tool for one job. It imposes nothing. It just synchronizes JavaScript objects.

Differently from Firebase, Swarm is an open library you may run inside your own setup. No lock-in.

Backbone is an MVC framework assuming an HTTP/JSON API behind it. Swarm only replaces the HTTP/JSON part. Well, it also emits change events. Swarm is the M. Probably, we should spell it swarM.

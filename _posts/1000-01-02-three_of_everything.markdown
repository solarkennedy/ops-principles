---
layout: principle
title: Three of Everything
category: principles
published: true
---

# Description

First to illustrate by example, here are some tools I think I've seen
that show the 3OE principle versus some tools that have the "there can
only be one" principle (highlander). In the end I'm going to use yelp
examples, but almost every place uses the same tools, its just between
us I know we have the common context:

* Sensu
* Activemq
* Ceph
* Cassandra
* etcd
* bittorrent
* mongodb
* zookeeper

Highlander:
Nagios
NFS
svn
mysql
puppet?
git?

I believe this principle is more than just distributed versus
centralized idea, but I don't know how to characterize it. However, I
think if this principle was well-defined, we could adopt it and *stop*
building things with a highlander-like design.

Example 1: Nagios
Yes, we have lots of nagioses at yelp. Even HA pairs, isn't that
great! No, it sucks.
Why? Because there is still *only one* active nagios server for a
given host. Our crazy HA nonsense and new Thruk are just hacks, trying
to solve the problem without understanding the principle

Example 2: Puppet
We have lots of puppetmasters! That's cool. You know what is not cool?
If you have 3 puppetmasters, and one fails, runs fail 1/3 of the time.
Nobody knows about it. Not super cool.

Example 3: NFS home dirs
I'm sure you heard this one suggested at the goal meeting. Wouldn't it
be nice to have the same homedir everywhere?

Example 4: mysql
Everyone is pretty aware of this. Most places that use mysql (even
wikipedia) have the highlander complex.

Example 5: Git
Even though git is "distributed", in practice we still use github,
sysrepo, git.yelpcorp.com.
---
Replacement 1: Sensu
This looks like it is built on the 3OE principle to me.

Replacement 2: .... middleware?
The puppet agent can't really take advantage of distributed puppetmasters.
Maybe instead of using dns, we could use something like zookeeper to
have puppetmasters register themselves and have them de-registered by
.. sensu? Then when a client goes to run, it tries to use a live
master from the zookeeper list?
Or maybe it can be something as simple as a healthcheck line in
"run-puppet" that just picks a good one out of the dns rr.
In the end, the server side still has no consensus. We have to "push"
to the puppet masters to make sure they are in sync. Outdated
puppetmasters are not eventually consistent

Replacement 3: Doing it wrong?
Ceph, maybe. btsync?

Replacement 4: nosql
This space is pretty well explored.

Replacement 5: Git++?
I've experimented with server hooks to automatically push to other
servers in a git cluster, but I don't know enough to make it robust.
It seems that you should be able to have 3 git servers on DNS rr, and
they should all be able to push and pull from each other
automatically? I could be thinking about it wrong. You certainly would
*not* want an eventually consistent filesystem underneath git.

The point:
I think the 3OE principle is worth adopting for more than just
databases. The problem is that my principle is not well defined, and
there is no good documentation + design patterns on how to build
things this way, even with things that have the *illusion* of being
distributed?

Do you think this principle is worth pursuing? If so, I was thinking
of maybe starting maybe a wiki, that could have list of tools, use
cases, recipes, design patterns, howtos, etc on how to think and build
based on this principle?  Even if just for my personal stuff.

I'm sure with lots of my examples you (and others) have better ideas
on how it could be done in a distributed way, maybe those examples
could be collected to make it easier for others to share and find. I
certainly would like it. I bet plenty of people who have never thought
like this would appreciate such a collection, and might stop them from
building the next distributed nagios hack?

PS
To reiterate, whatever it is I'm thinking about is more than the *No
SPOF* principle. Even with master-master mysql shards and HA DRBD NFS
pairs and our Nagios setup, it can be made with no SPOF, but that
isn't enough I think. 


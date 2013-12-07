---
title: Automatic Convergence
category: principles
layout: principle
published: true
---

All distributed systems require some sort of coordination. The principle of 
Automatic Convergence dictates that the coordination between services be
done in a way that requires no human interaction, even if the desired 
configuration requires time to converge to a working state.

## In the beginning: Hard coded database references
Every application that connects to a database probably starts with
explicit connections to a database embedded **right into the code**.

Before long it is realized that this must be isolated into some sort of
config file.

## Suddenly: The config file
For example, wordpress uses the wp-config.php. Still hard coded, but slightly
separated, probably .gitignored, but easily changed. The **config file** is
a slight improvement. 

## Aside: Environment variables
The [Twelve-Factor App](http://12factor.net/config) manifesto specifies that
the solution to this problem is to use **environement variables**. This strict
separation from configuration and code allows private credentials to be totally
isolated, and gives the flexibility to easily handle different environments 
(stage, prod, qa). 

## Automatic discovery and convergence
At some scale, it is too much for a human to keep up with changing dynamic 
infrastructure. Some sort of service or "middleware" has to connect the 
pieces together for you. 

In most cases, the actual configuration of a service will point at a dynamically
configured load balancer. Or perhaps the configuration may be baked into the
DNS records of service. Or perhaps the application itself may be able to auto-discover
its dependent services directly.

In general, a separation between the application and the discovery mechanism is 
recommended.

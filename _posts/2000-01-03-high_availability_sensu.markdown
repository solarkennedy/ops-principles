---
layout: recipe
title: High Availability Sensu
category: recipes
published: true
principles:
- No Single Point of Failure
---

This recipe was published by [Jean-Francois Theroux](http://failshell.io/). It
describes to to setup [Sensu](http://sensuapp.org/) in a high-availability 
production environment. 

# Components

## Keepalived
Keepalived is used to configure a failover virtual ip via  vrrp to fail over 
between two nodes. You might describe this as failover at layer 3. (IP)

## HAProxy
HAProxy load runs on both servers, and load balances all of the pieces. It has
frontend and backends configured for RabbitMQ, Sensu API, Sensu Admin, and Redis

## RabbitMQ
Two rabbitmq servers are setup in [http://www.rabbitmq.com/ha.html](mirrored mode).
HAProxy detects the health of each and routes connections to them.

## Redis
Two redis servers are setup in master/slave configuration, with 
[Redis Sentinel](http://redis.io/topics/sentinel) to handle automatic failover and
promotion. A special xinetd check is setup in place for HAProxy to only have the
master available for service. 

## Sensu Servers
The sensu servers have no open ports, they must simply be configured to connect
to the RabbitMQ vip.

## Sensu API and Sensu Admin Dashboard
These services are stateless and can simply be load balanced via HAProxy.

## External Links
- [Original Blog Post](http://failshell.io/sensu/high-availability-sensu/)
- [Sensu](http://sensuapp.org/)

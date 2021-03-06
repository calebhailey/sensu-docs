---
version: "0.12"
category: "Overview"
title: "Overview of Sensu"
next:
  url: installing_sensu
  text: "Installing Sensu"
---

# What is Sensu?

Sensu is often described as the "monitoring router". Most simply put, Sensu 
connects "check" scripts run across many nodes with "handler" scripts run on 
one or more Sensu servers. [Checks](checks) are used, for example, to determine 
if Apache is up or down. [Checks](checks) can also be used to collect metrics 
(such as MySQL or Apache statistics). The output of [checks](checks) is routed 
to one or more [handlers](handlers). [Handlers](handlers) determine what to do 
with the results of [checks](checks). [Handlers](handlers) currently exist for 
sending alerts via email, as well as to various external services such as 
Pagerduty, IRC, Twitter, etc. [Handlers](handlers) can also feed metrics into 
Graphite, Librato, etc. Writing [checks](checks) and [handlers](handlers) is 
quite simple and can be done in any language.

Key details:

- Ruby (EventMachine, Sinatra, AMQP), RabbitMQ, Redis.
- Excellent test coverage with continuous integration via 
  [travis-ci](http://travis-ci.org/#!/sensu/sensu).
- Messaging oriented architecture. Messages are JSON objects.
- Ability to re-use existing Nagios plugins.
- Plugins and handlers (think notifications) can be written in any language.
- Supports sending metrics into various backends (Graphite, Librato, etc).
- Designed with modern configuration management systems such as Chef or Puppet 
  in mind.
- Designed for cloud environments.
- Lightweight, less than 1200 lines of code.
- "Omnibus" style packages for easy, low-friction deployments!

## Components

The [Sensu platform](https://github.com/sensu/sensu) is made up of a number of 
components.

## The Sensu server

The Sensu server triggers clients to initiate checks, it then receives
the output of these checks and feeds it to [handlers](handlers). (As of version 
0.9.2, clients can also execute checks that the server doesn't know about and 
the server will still process their results, more on these 'standalone checks' 
elsewhere in the documentation. See 
[standalone checks](adding_a_standalone_check)).

The Sensu server relies on a Redis instance to keep persistent data. It also
relies heavily (as do most Sensu components) on access to RabbitMQ for
passing data between itself and Sensu client nodes.

## The Sensu client

The Sensu client runs on all of your systems that you want to monitor.
The Sensu client will execute check scripts (think `check_http`,
`check_load`, etc) and return the results from these checks to
the Sensu server via RabbitMQ.

## The Sensu API

A REST API that provides access to various pieces of data maintained on
the Sensu server (stored in Redis). You will typically run this on the same
host as your Sensu server or Redis instance. It is mostly used by
internal sensu components at this time.

## The Sensu dashboard

A Web based dashboard providing an overview of the current state of your Sensu
infrastructure and the ability to perform actions, such as temporarily
silencing alerts.

---
title: "Introducing alertmon"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Introducing alertmon

> [!INFO]
> This document's audience is engineers or engineering managers evaluating alertmon as a part of their infrastructure. As a sample of my work, it's an important document because it offers an introduction to an open-source project, allowing the project a wider use.

## What alertmon is

In production systems, you need a bridge between time-series databases, which generate an enormous amount of metrics, and a paging system. Alertmon acts as that bridge.

Alertmon is a production alerting platform built for internal use. It offers a web user interface to create and configure production alerts, and acts as the mechanism to check and notify firing alerts. It's built for use with Graphite, and uses MongoDB to store alerts.

You can use alertmon to list all alerts, search by alert tag, view firing alerts, create and configure a specific alert, and silence a firing alert.

## What alertmon isn't

Alertmon isn't a replacement for storing and analyzing time-series data. Instead, it stores and runs queries on time-series data.

Alertmon also isn't a replacement for a notification system like PagerDuty. Instead, it works with PagerDuty to notify on firing alerts.

## Why use alertmon

Alertmon solves the problem of making alert configuration self-service. It enables large engineering teams to coordinate with the production team to define and manage their own alerts, while allowing engineers a way to change their alerts without involving production. Alertmon supports the needs of a 99.9% uptime environment.

## Reliability and failure tolerance

Although alertmon depends on MongoDB for storing alerts, it doesn't depend on MongoDB for checking alerts. Instead, it stores the alert data on disk and uses this data to check alerts. This means MongoDB can be down and alertmon will still function.

Alerts are also checked in a staggered manner, to avoid overloading the Graphite cluster.

## Project status

Alertmon is no longer actively maintained. It will take some modification in its current state to get a working version from scratch, including configuring a Graphite cluster and MongoDB database.

## More information

To learn how to create an alert, see [Creating an alert with alertmon]({{< ref "docs/creating-an-alert-with-alertmon.md" >}}).

To view the reference manual, see [Alertmon Reference]({{< ref "docs/alertmon-reference.md" >}}).

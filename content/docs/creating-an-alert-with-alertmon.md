---
title: "Creating an alert with alertmon"
weight: 40
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Creating an alert with alertmon

> [!INFO]
> This document's audience is engineers looking to create an alert on an already configured instance of alertmon. As a sample of my work, it's an important document because it shows how I would write a tutorial, saving valuable user time and reducing support work.

In this tutorial you will create an alert with alertmon. Along the way you will dive into the different pieces of configuration that go into creating an alert.

## Prerequisites

Make sure you have a running instance of alertmon, usually inside of a VPN authenticated with your work email. You should also have access to a working graphite cluster. If you haven't already, familiarize yourself with alertmon with the document [Introducing alertmon]({{< ref "docs/introducing-alertmon.md" >}}).

Then, open the url of alertmon, and click "Add Alert."

You will see a form with a variety of fields to fill out. This tutorial will walk you through these fields and what they do.

## 1. Define a graphite target

The first field for you to fill out is the "Graphite targets" field. Add more graphite targets with the "Add target" button.

A graphite target is the query string from a graphite metric. Experiment with different metrics from the Graphite composer. When you are ready, copy and paste the query string into this field.

For example, `timeShift(machines.fsao41.gmond.system.net.out,"1day")` will look at the system load of a given machine compared to the system load 1 day before.

## 2. Add tags

Next, add tags. This enables you to search for existing alerts later. For example, tag by team, or by environment.

## 3. Define time to check

Specify the time range this alert will check. By default, it checks the last five minutes: `-6mins` to `-1mins`. This makes sense for many alerts, but sometimes you will want to change this, for example, to look at the last half hour with `-31mins` to `-1mins`. Note that you need to follow the Graphite standard for time intervals, with a `-` symbol before the interval.

## 4. Define warning and critical thresholds

Define warning and critical level thresholds. A threshold is a value on a graph that the currently measured value gets checked against. An alert fires if enough measured data crosses the threshold.

Warning and critical alerts can fire to differently configured emails. They also can give the person on-call a rough idea of the severity of the alert.

You don't have to configure both a warning and a critical threshold, but you should configure at least one.

For each of warning and critical thresholds, you:

1. Define a threshold value
2. Define how many number or percentage of minutes can cross the threshold before alerting. A standard value is 50%
3. Define who gets the alert email. These can be further configured to go to a Slack channel, or to a phone number, or to PagerDuty, outside of alertmon

## 5. Define alert title

After defining the thresholds, define the alert title. This should explain what this alert tests. For example, "System load too high on mongoc machines."

A good alert title will give the on-call engineer a quick view into the possible issue with the system.

## 6. Add annotations/playbook links

After defining the alert title, it's important to define annotations. This is a place to add notes to the person responding to the alert on what to do about it. Add links to dashboards or to possible playbooks to aid in the response to the alert.

## 7. Optional features

There are a few optional and advanced features to adding an alert of which you might want to take advantage.

- Select a post-query function. This is an advanced feature that allows some queries to be partially computed by alertmon, rather than graphite. This speeds up some queries.
- Select a graphite cluster. This allows you to take advantage of certain metrics which might be only hosted on one cluster.
- Select daytime paging. This means your alert will only fire during business hours, and not overnight.

## 8. Create the alert

At this point, click the button that says "Save." Your alert will be checked every polling interval, with a default of every five minutes, along with the rest of the configured alerts.

On the alertmon home screen, view your alert under "Recent Alert Definitions."

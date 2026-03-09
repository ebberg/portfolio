---
title: "On-call incident response how-to"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# On-call incident response how-to

> [!INFO]
> This document's audience is junior Site Reliability Engineers getting accustomed to on-call responsibilities. As a sample of my work, it's an important document because it reduces procedural errors when going on-call, making incidents smoother to resolve.

This how-to guide walks you through what you should do if you're paged for an incident. It's written to keep you aware of best practices and to make responding to an incident routine.

## Prerequisites

- Make sure you have email access and can log into the internal instance of **alertmon** to manage alerts that fire
- Make sure you have access to the **Graphite cluster** and can log into the **Grafana dashboards**
- Make sure you can log into **PagerDuty** to acknowledge alerts and that it's configured on your phone to page you
- Make sure you have access to **Slack** to communicate with your coworkers
- Make sure you have **VPN access** and your **Terminal** set up with SSH keys or other CLI commands you need to debug or use playbooks

## Remain calm

The most important thing to remember about being on-call is to remain calm. Keeping calm means you can communicate clearly about the current situation, and you are better equipped to run commands error-free.

Often, alerts are false positives or aren't immediate concerns. Then, your effort is best spent modifying alerts to better map only to emergencies.

When you do get paged for an actual emergency, you need to make sure you:

- Keep lines of communication open on Slack
- Make changes or run commands with clarity into what you are doing
- Keep a clear record of every change you make and when

## 1. Acknowledge the alert on PagerDuty

Acknowledge the alert on PagerDuty to stop the alert from ringing your phone again or (if your team has escalation set up) paging the back-up on-call person. Acknowledging the alert also creates a record of your response, which can be useful when creating a post-mortem.

## 2. Mention you're responding in Slack

Communicate in Slack that you're responding to the alert. Communicating with the team during an alert response prevents confusion about the status of the incident and can help with post-mortem analysis later.

> [!INFO]
> **Tip**: Try to update Slack with your status every 15 minutes while an alert is ongoing, even just to say you are still working on it.

## 3. View alert in alertmon

Start investigating by viewing the alert in alertmon. The link is in the alert from PagerDuty. You'll need a VPN connection and email authentication to view the alert.

The alert page will give you information you can use to determine if the alert is a false positive and further diagnosis steps. You can also mute the alert.

For more information about alertmon, see:

- [Introducing alertmon]({{< ref "docs/introducing-alertmon.md" >}})
- [Creating an alert with alertmon]({{< ref "docs/creating-an-alert-with-alertmon.md" >}})
- [Alertmon reference]({{< ref "docs/alertmon-reference.md" >}})

## 4. View dashboards in Grafana

Follow the alert links to relevant dashboards in Grafana and view the system health as a whole. Use this to determine overall trends or issues that are relevant to the alert firing.

> [!INFO]
> **Tip**: Your first responsibility during an on-call incident is to restore service to the site. Your second responsibility is to determine root cause or *why* the incident occurred. Optimize towards restoring service first, doing investigative work later.

## 5. Follow playbook steps if relevant

Act to restore service if the alert is serious. Look at the alert or the playbook documentation directory for information relevant to this alert. The alert itself might point to a relevant playbook.

Follow the playbook steps to restore service. Make sure you understand each command before running it in production. Let your coworkers know on Slack what command you are running. Double-check the command before pressing enter, verifying the service you are impacting and the environment you are running in.

For example, consider getting an alert that errors have increased for a given service after a deploy. In this case, you may need to roll back the service to the last deployed version. You should double-check the service, the version, and the environment before running, and let your coworkers know on Slack that you are doing this.

## 6. Escalate to other engineers if necessary

You aren't alone when you are on-call. Ping other engineers to assist you when you are working on an incident. Consider escalating when:

- You're stuck, with 15 minutes or more going by without answers
- The service impacted requires specialist knowledge to diagnose
- The steps to fix aren't documented

It's not a flaw to need help during an incident. It's best to bring people into the incident resolution to get the problem resolved in short order.

## 7. Mention resolution in Slack

When the incident has concluded, graphs will return to normal, and the alert will stop firing. At this time, let your coworkers know on Slack that you resolved the incident. This prevents confusion wondering whether the incident is still ongoing.

## 8. Start a post-mortem if appropriate

In the case of a major incident, a post-mortem is appropriate. A post-mortem should be *blameless*. Its goal isn't to point fingers, but to identify where the processes worked and where they need improvement. Use a template to start a post-mortem, and use the Slack timestamps from your messages to create a timeline of the incident.

This isn't required for every incident, but it's useful for problems that take significant time to resolve or for unique issues.

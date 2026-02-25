---
title: "Alertmon reference"
weight: 50
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Alertmon reference

> [!INFO]
> This document's audience is engineers looking to understand the scope of alertmon, reference how to use a specific url, or look up how to use a specific feature. As a sample of my work, it's an important document because it shows how I would write a reference guide, saving valuable user time and reducing support work.

This reference guide explains each of the endpoints associated with alertmon. It walks through the url, the methods, associated parameters, and a description of the endpoint.

## Alert urls

This is the reference material for the URLs that reference a specific alert and enable functionality based on a specific alert.

### Clone alert

**URL**: `/alert/<alert_id>/clone`

**Methods**: GET

**Parameters**: None

**Description**: Makes a copy of the given alert and inserts it into the alert database.

### Mute alert

**URL**: `/alert/<alert_id>/mute/add`

**Methods**: GET, POST

**Parameters**:

- **hours**: number of hours to silence alert
- **seconds**: number of seconds to silence alert

**Description**: Silence the given alert from firing for a given number of hours or seconds.

### Unmute alert

**URL***: `/alert/<alert_id>/mute/del`

**Methods**: GET

**Parameters**: None

**Description**: Unsilence the given alert, allowing it to fire again.

### Delete alert

**URL**: `/alert/<alert_id>/del`

**Methods**: GET

**Parameters**: None

**Description**: Delete the given alert from the alert database.

### Edit alert

**URL**: `/alert/<alert_id>/edit`

**Methods**: GET, POST

**Parameters**: None

**Description**: Edit the given alert in the alert database. Get the alert form, or post an update to the alert.

### View alert

**URL**: `/alert/<alert_id>/view`

**Methods**: GET

**Parameters**: None

**Description**: View the given alert, including current firing state. Doesn't support editing the alert from this view.

## API v0 urls

This is the reference material for the URLs that comprise the Alertmon API.

### List alerts API

**URL**: `/api/v0/alert`

**Methods**: GET

**Parameters**: None

**Description**: Returns a JSON object of all alerts configured in the system.

### Add alert API

**URL**: `/api/v0/alert/add`

**Methods**: GET, POST

**Parameters**: None

**Description**: Allows posting of an alert object to create an alert. The alert object form isn't specified here. It will be available in a future version of this document.

### Check alert firing status API

**URL**: `/api/v0/alert/<alert_id>/check`

**Methods**: GET

**Parameters**: None

**Description**: Checks the current status of the given alert, and returns the formatted email explaining the alert status.

### Delete alert API

**URL**: `/api/v0/alert/<alert_id>/del`

**Methods**: GET

**Parameters**: None

**Description**: Deletes the given alert from the alert database.

### View alert API

**URL**: `/api/v0/alert/<alert_id>/view`

**Methods**: GET

**Parameters**: None

**Description**: Returns a JSON object for the given alert.

### Firing alerts API

**URL**: `/api/v0/firing`

**Methods**: GET

**Parameters**: None

**Description**: Returns the alerts currently firing from the time last checked.

### Last export API

**URL**: `/api/v0/lastexport`

**Methods**: GET

**Parameters**: None

**Description**: Gets the timestamp of the last `alertsnap` run, which saves the alerts to a flat file from the alert database.

### Last run API

**URL**: `/api/v0/lastrun`

**Methods**: GET

**Parameters**: None

**Description**: Gets the timestamp of the last `check_alertmon` run, which checks all alerts at a configurable period.

## Authentication urls

This is the reference material for the URLs that comprise the alertmon authentication system.

### Authentication done

**URL**: `/auth/done`

**Methods**: GET

**Parameters**: None

**Description**: This page signals successful login to alertmon.

### Authentication

**URL**: `/auth/login`

**Methods**: GET

**Parameters**: None

**Description**: This page handles the Google OAuth 2 login into alertmon.

### Logout

**URL**: `/auth/logout`

**Methods**: GET

**Parameters**: None

**Description**: This page handles logging out of alertmon.

## Page urls

This is the reference material for the URLs that comprise alertmon's pages that aren't specifically designed for a given alert.

### Homepage

**URL**: `/`

**Methods**: GET

**Parameters**: None

**Description**: Displays a list of recently firing alerts, recently created alerts, and muted alerts.

### Alert list page

**URL**: `/alert`

**Methods**: GET

**Parameters**:

- **tags**: Filterable tags associated with given alerts

**Description**: Displays all alerts, or just the alerts specified by a given tag.

### Add alert page

**URL**: `/alert/add`

**Methods**: GET, POST

**Parameters**: None

**Description**: Displays a form for adding a new alert, or saves the new alert to the alert database.

### Firing page

**URL**: `/firing`

**Methods**: GET

**Parameters**: None

**Description**: Displays all alerts currently firing. Doesn't check individual alerts. Rather, it looks at the last reported status of the `check_alertmon` function, which checks all alerts at a configurable period.

### Mute page

**URL**: `/mute`

**Methods**: GET

**Parameters**: None

**Description**: Displays all silenced alerts, including for how long they're silenced.

### On-call hours page

**URL**: `/oncall/hours`

**Methods**: GET, POST

**Parameters**: None

**Description**: View the current on-call daytime hours, or change the hours.

### Service list page

**URL**: `/service`

**Methods**: GET

**Parameters**: None

**Description**: Returns a JSON object of all configured services.

## Static url

This is the reference material for the URLs that comprise alertmon's static assets.

### Static pages

**URL**: `/filez/*`

**Methods**: GET

**Parameters**: None

**Description**: Returns a given static file for use by alertmon.

---
title: InfluxDB Cloud monthly updates
description: Important changes and what's new in each InfluxDB Cloud 2.0 update.
weight: 101
menu:
  influxdb_2_0_ref:
    parent: Release notes
    name: InfluxDB Cloud
aliases:
  - /cloud/about/release-notes
---

InfluxDB Cloud updates occur frequently. Find a compilation of monthly updates below.

## 2020-9-25

### Install and customize InfluxDB community templates in the Cloud UI

Install and customize any [InfluxDB community template](https://github.com/influxdata/community-templates#templates) directly in the Cloud user interface (UI). For more details, see how to [install and customize a template in the UI](/influxdb/v2.0/influxdb-templates/cloud/#install-and-customize-a-template-in-the-cloud-ui).

## 2020-09-02

### Pricing updates and Azure region

- Update [pricing vectors](/influxdb/v2.0/account-management/pricing-plans/#pricing-vect) to determine pricing by the total data out and query count.

- Add [Microsoft Azure support](/influxdb/v2.0/reference/urls/#microsoft-azure) for the `eastus` and `westeurope` regions. Each region has a unique InfluxDB Cloud URL and API endpoint.

### Flux updates

 - Add time-weighted average [`timeWeightedAvg()` function](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/aggregates/timeweightedavg/).
 - Update [`integral()` function](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/aggregates/integral/) with linear interpolation.
 - Add [Flux query profiler](/influxdb/v2.0/reference/flux/stdlib/profiler/#use-the-query-profiler) to output query statistics that help you better understand query performance.
 - Add [`tasks.lastSuccess()` function](/influxdb/v2.0/reference/flux/stdlib/influxdb-tasks/lastsuccess/) to retrieve the time of the last successful run of an InfluxDB task.
 - Add the [`array.from` function](/influxdb/v2.0/reference/flux/stdlib/experimental/array/from/) to build ad hoc tables in a Flux script.

### Bug fixes

 - Resolve issues in checks and notifications.

## 2019-09-10 _Monitoring & Alerts_

## Features
- **InfluxDB 2.0 alpha-17** –
  _See the [alpha-17 release notes](/influxdb/v2.0/reference/release-notes/influxdb/#v2-0-0-alpha-17-2019-08-14) for details._
- Alerts and Notifications to Slack (Free Plan), PagerDuty and HTTP (Usage-based Plan).
- Rate limiting on cardinality for Free Plan.
- Billing notifications.
- Pricing calculator.
- Improved Signup flow.

## 2019-07-23 _General Availability_

### Features

- **InfluxDB 2.0 alpha-15** –
  _See the [alpha-9 release notes](/influxdb/v2.0/reference/release-notes/influxdb/#v2-0-0-alpha-15-2019-07-11) for details._
- Usage-based Plan.
- Adjusted Free Plan rate limits.
- Timezone selection in the user interface.

---

## 2019-05-06 _Public Beta_

### Features

- Add rate limiting for Free Plan users.
- Add client libraries for Go and JS.

### Bug fixes

- Users cannot delete themselves from their Cloud 2.0 account.
- The bucket retention period for Free Plan users is set to 72 hours.
- Free Plan users cannot change a bucket's retention period.

---

## 2019-05-02

### Features

- **InfluxDB 2.0 alpha-9** –
  _See the [alpha-9 release notes](/influxdb/v2.0/reference/release-notes/influxdb/#v2-0-0-alpha-9-2019-05-01) for details._

### Bug fixes

- Usage statistics on the Usage page show correct values.
- Existing tasks with duration specified in nanoseconds no longer need to be resubmitted.
- Removed the additional user that showed up as an owner under the Cloud 2.0 organization.
- Cloud users can use CLI tools to interact with their Cloud tenant.


---

## 2019-04-05

### Features

- **InfluxDB 2.0 alpha-7** –
  _See the [alpha-7 release notes](/influxdb/v2.0/reference/release-notes/influxdb/#v2-0-0-alpha-7-2019-03-28) for details._

### Bug fixes

- Logout works in InfluxDB Cloud 2.0 UI.
- Single sign-on works between https://cloud2.influxdata.com and https://us-west-2-1.aws.cloud2.influxdata.com.
- Able to copy error message from UI.
- Able to change a task from every to cron.
- Able to create a new bucket when switching between periodically and never (retention options).

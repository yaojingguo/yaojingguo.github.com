---
layout: post
title: "Prometheus"
description: ""
category:
tags: []
---
{% include JB/setup %}

`data` directory saves time series database. Here is doc [storage](https://prometheus.io/docs/prometheus/latest/storage/). The following
steps show how time series database works.

- Start a data source
- Start Prometheus
- See some data in Prometheus
- Stop Prometheus and the data database
- Start Prometheus again
- The old data will show up

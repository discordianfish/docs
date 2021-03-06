---
title: Data model
sort_rank: 1
---

# Data model

Prometheus fundamentally stores all data as [_time
series_](http://en.wikipedia.org/wiki/Time_series): streams of timestamped
values belonging to the same metric and the same set of labeled dimensions.
Besides stored timeseries, Prometheus may generate temporary derived timeseries
as the result of queries.

## Metric names and labels
Every time series is uniquely identified by its _metric name_ and a set of
_key-value pairs_, also known as _labels_.

The _metric name_ specifies the general feature of a system that is measured
(e.g. `http_requests_total` - the total number of HTTP requests received). It
may contain ASCII letters and digits, as well as underscores and colons. It
must match the regex `[a-zA-Z_:][a-zA-Z0-9_:]`.

Labels enable Prometheus's dimensional data model: any given combination of
labels for the same metric name identifies a particular dimensional
instantiation of that metric (for example: all HTTP requests that used the
method `POST` to the `/api/tracks` handler). The query language
allows filtering and aggregation based on these dimensions. Changing any label
value, including adding or removing a label, will create a new time series.

Label names may contain ASCII letters, numbers, as well as underscores. They
must match the regex `[a-zA-Z_][a-zA-Z0-9_]`.

Label values may contain any Unicode characters.

See also the [best practices for naming metrics and labels](/docs/practices/naming).

## Samples
Samples form the actual time series data. Each sample consists of:

   * a float64 value
   * a millisecond-precision timestamp

## Notation
Given a metric name and a set of labels, time series are frequently identified
using this notation:

    <metric name>{<label name>=<label value>, ...}

For example, a time series with the metric name `api_http_requests_total` and
the labels `method="POST"` and `handler="/messages"` could be written like
this:

    api_http_requests_total{method="POST", handler="/messages"}

This is the same notation that [OpenTSDB](http://opentsdb.net/) uses.

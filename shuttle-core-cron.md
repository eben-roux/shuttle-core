---
title: Shuttle.Core.Cron
layout: api
---
# Shuttle.Core.Cron

```
PM> Install-Package Shuttle.Core.Cron
```

Provides [cron](https://en.wikipedia.org/wiki/Cron) expression parsing.  This implementation starts from the `minute` field (so no `second`).

## CronExpression

``` c#
public CronExpression(string expression) : this(expression, DateTime.Now)
public CronExpression(string expression, DateTime date)
```

Creates a `CronExpression` instance and parses the given `expression`.  The `date` specifies to root date from which to determine either the next or previous occurrence.

``` c#
public DateTime NextOccurrence()
public DateTime NextOccurrence(DateTime date)
```

Returns the next date that would follow the given `date`.  If no date is provided the root date will be used.  This method also sets the root date to the result.

``` c#
public DateTime GetNextOccurrence(DateTime date)
```

Returns the next date that would follow the given `date`.

``` c#
public DateTime PreviousOccurrence()
public DateTime PreviousOccurrence(DateTime date)
```

Returns the previous date that would precede the given `date`.  If no date is provided the root date will be used.  This method also sets the root date to the result.

``` c#
public DateTime GetPreviousOccurrence(DateTime date)
```

Returns the previous date that would precede the given `date`.

## Cron Samples

Format is {minute} {hour} {day-of-month} {month} {day-of-week}

```
{minutes} : 0-59 , - * /
{hours} :     0-23 , - * /
{day-of-month} 1-31 , - * ? / L W
{month} : 1-12 or JAN-DEC    , - * /
{day-of-week} : 1-7 or SUN-SAT , - * ? / L #

Examples:
* * * * * - is every minute of every hour of every day of every month
5,10-12,17/5 * * * * - minute 5, 10, 11, 12, and every 5th minute after that
```


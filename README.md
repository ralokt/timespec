**timespec** is a simple syntax that allows you to quickly specify dates and times. This is the specification of, and a Python module implementing, version 1.1.2 of timespec. See [`sleeptill`](https://github.com/fenhl/syncbin/blob/master/python/sleeptill.py) for an example program that uses this module.

# Syntax

A *timespec* consists of a list of whitespace-separated *predicates*, making them ideal as command-line arguments, as well as an optional start datetime and timezone. The start datetime defaults to the current system datetime, and the timezone defaults to UTC. The date described by a timespec is defined as the first datetime that is no earlier than the start datetime and matches all predicates. For performance reasons, this implementation will fail to find datetimes which are more than 10 years after the start datetime.

The following predicates are currently supported:

## Date

A date in `YYYY-MM-DD` format, that is, the year given as 4 or more digits, and the month and day given as any number of digits. Matches all datetimes on the given date. This implementation also supports years with less than 4 digits, but this is *not* part of the specification and may be used for shorter representations of years relative to the start date in future versions of timespec.

## Time of day

An optional hour, followed by a colon, followed by an optional minute, followed by an optional colon, followed by an optional second if the second colon is present. Matches datetimes where the time of day is equal to the specified parts. For example, `5:` and `05::` both match times from 05:00:00 to 05:59:59, while `05:00` and `5:0:` both match times from 05:00:00 to 05:00:59. `::30` matches 30 seconds after every full minute.

## Weekday

The strings `mon`, `tue`, `wed`, `thu`, `fri`, `sat`, and `sun` (all case insensitive) match all datetimes on the given day of the week.

## Modulus

A number followed by an `s` for second, `m` for minute, `h` for hour, or `d` for day of month (all case sensitive) matches datetimes where the remainder of the specified part divided by the given number is zero. For example, `15m ::0` matches datetimes on every quarter hour.

## POSIX timestamp

A number with 10 or more digits matches the datetime with that exact POSIX timestamp.

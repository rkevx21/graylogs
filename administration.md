# Administration

## Search

### Syntax

By default all message fields are included in the search if you dont specify a message field to search in.

Messages that include the *term* **ssh**: ```ssh```

Messages that include the *term* **ssh** or **login**: ```ssh login```

Messages that include the *exact phrase* **ssh login**: ```"ssh login"```

Messages where the *field type* includes **ssh**: ```type:ssh```

Messages where the *field type* includes **ssh or login**: ```type:(ssh login)```

Messages where the *field type* includes the *exact phrase* **ssh login**: ```type:"ssh login"```

Messages that *do not have the field type* : ```_missing_:type```

Messages that *have the field type*: ```_exists_:type```

By default all terms or phrases are OR connected so all messages that have at least one hit are returned.

**Boolean operators and Groups**
```
"ssh login" AND source:example.org 
("ssh login" AND (source:example.org OR source:another.example.org)) OR _exists_:always_find_me
```

```
"ssh login" AND NOT source:example.org NOT example.org
```

*Note that **AND**, **OR**, and **NOT** are case sensitive and must be typed in all upper-case.*

**Wildcards**

Use ? to replace a single character or * to replace zero or more characters

```
source:*.org
source:exam?le.org
source:exam?le.*
```

**Fuzziness**

```
ssh logni~
source:exmaple.org~
source:exmaple.org~1
"foo bar"~5
```

**Range Queries**

```
http_response_code:[500 TO 504]
http_response_code:{400 TO 404}
bytes:{0 TO 64]
http_response_code:[0 TO 64}
http_response_code:>400
http_response_code:<400
http_response_code:>=400
http_response_code:<=400
http_response_code:(>=400 AND <500)
```

### Escaping

Characters must be escaped with a backslash

```&& || : \ / + - ! ( ) { } [ ] ^ " ~ * ?``` e.g. ```resource:\/posts\/45326```

### Time frame selector

Defines in what time range to search in.

Searches between one month ago and now: ```last month```

Searches between four hours ago and now: ```4 hours ago```

Searches between 1st of April and 2 days ago: ```1st of april to 2 days ago```

Searches between yesterday midnight and today midnight in timezone +0200 - will be 22:00 in UTC: ```yesterday midnight +0200 to today midnight +0200```

## Streams

Mechanism to route messages into categories in realtime while they are processed

Create Stream and Manage Rules to identify the field you want to check, and the condition that satisfy.
*Field, Type, Value*

## Dashboard

Build pre-defined views for data that is important as easy to access.

Adding widget based from the **Search** results.

### Analysis

Analyze field based on the search result .

### Field statistics

Compute different statistics on fields.

### Quick values

Distribution of values for a field, A graphic representation of the common values contained in a field.

### Field graphs

It creates a graph of a field that can be cuztomize from statistical funtion to graph interpolation, as well as, time resolution and the kind of graph to use to represent values.


## System

### Overview

Shows the overview of graylogs components and processses, including notification and information (*System jobs, Graylog cluster, Elasticsearch cluster, Indexer failures, Time configuration, System messages*).

### Nodes

Shows list of nodes (active and inactive) and can be configured.

### Inputs

Message inputs are responsible for accepting log messages and different inputs can be selected.

### Authentication

*Authentication Management (Users and Roles)*, a system which secures the access to its features. Each interaction which can look at data or change configuration be performed as an authenticated user. Each user can have varying levels of access to Graylog’s features, which can be controlled with assigning roles to users. 

### Sidecars

A lightweight configuration management system for different log collectors.
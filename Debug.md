# System Debug

## Goal during an incident

1. Identify problem

2. Reduce impact (mitigation)

3. Restore service

4. Find root cause

5. Prevent recurrence

Flow:

```
Identify → Scope → Mitigate → Root Cause → Long-term Fix
```

## Identify the issue

First **confirm the symptom using metrics/logs**.

Typical signals:

* Latency

* Error rate

* Traffic

* Resource usage

* Dependency health

Always determine:

Is this:
- single host
- subset of hosts
- entire fleet
- one region
- global

This quickly tells you whether it is:
```
* host problem
* deployment problem
* dependency problem
* systemic issue
```
### Latency high or System Slow
#### Check scope

* Is it single host?
* Single AZ / region?
* Full fleet?

Commands
```
top
vmstat
iostat
ss
```
#### Check system resources


CPU
```
top
mpstat
```
Memory
```
free -m
vmstat
```
Disk
```
iostat
df -h
```
Network
```
ss -s
netstat
```
Look for
```
CPU saturation
memory pressure
disk IO wait
network congestion
```
#### Check dependencies

Common latency causes
```
database
cache
downstream service
DNS
network routing
```
Questions to ask

* Is DB latency high?
* Is cache hit rate low?
* Are downstream timeouts increasing?
* Check application behavior

Possible causes

* thread pool exhausted
* queue backlog
* lock contention
* GC pause
* infinite loop

Tools
```
strace
perf
thread dump
```
### Error rate/count high
* Downstream error?
* Internal server error?
* Network error?

### System not responding
* Request not reaching server?
* Host down?

### System malfunctioning
* System not returning expected response?
* Input error?
* Config error?
* Recent code change?

### How to escalate
Follow the defined SOP for the system, usually page senior engineer and manager as a start and then they call pull in more people if required

## Mitigate the issue
### Error rate/count high

### System not responding


### System malfunctioning


## How do we prevent similar issue in the future




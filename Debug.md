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

- Latency
- Error rate
- Traffic
- Resource usage
- Dependency health

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

- Is it single host?
- Single AZ / region?
- Full fleet?

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

- Is DB latency high?
- Is cache hit rate low?
- Are downstream timeouts increasing?
- Check application behavior

Possible causes

- thread pool exhausted
- queue backlog
- lock contention
- GC pause
- infinite loop

Tools

```
strace
perf
thread dump
```

### Error rate/count high

First determine error type.

- 4xx → client issue
- 5xx → server issue
- timeouts → dependency or resource
- connection error → network or service down

#### Downstream error

Signs

- timeout calling dependency
- connection refused
- HTTP 5xx from dependency

Check

- dependency dashboards
- network connectivity
- retry storm

Commands

```
curl
ping
traceroute
tcpdump
```

Mitigation

- failover
- disable feature
- fallback cache
- reduce traffic

#### Internal server error

Possible causes

- new deployment bug
- null pointer
- config mismatch
- memory corruption
- thread pool exhaustion

Check

- logs
- recent deployments
- feature flags

Commands

```
journalctl
tail -f logs
Network error
```

Possible causes

- packet loss
- connection backlog
- DNS failure
- load balancer issue

Commands

```
ss -lnt
netstat
tcpdump
dig
```

### System not responding

First determine where the request stops.

Flow

```
Client → Load Balancer → Service → Dependency
```

#### Check each stage.

Request not reaching server

Possible causes

- DNS failure
- load balancer failure
- network ACL
- firewall rule
- routing issue

Commands

```
curl
ping
traceroute
dig
tcpdump
```

#### Host down

Possible causes

- host crash
- OOM kill
- kernel panic
- disk full

Check

- host health dashboard
- system logs

Commands

```
dmesg
journalctl
df -h
```

### System malfunctioning

Example

- system returns incorrect result
- unexpected behavior
- data corruption

#### System not returning expected response

Possible causes

- logic bug
- data inconsistency
- version mismatch
- cache inconsistency

Check

- logs
- recent deploy
- data state

#### Input error

Possible causes

- invalid request
- schema change
- backward incompatibility

Check

- API logs
- request payload
- validation logic

#### Config error

Very common production issue.

Examples

- wrong environment variable
- feature flag change
- wrong service endpoint
- incorrect timeout

Check

- config service
- deployment config
- recent config changes

#### Recent code change

One of the most common root causes.

Check

- recent deployments
- feature rollouts
- experiment flags

Mitigation

- rollback
- disable feature flag
- revert config

## Mitigate the issue

Goal

1. restore service quickly
2. reduce customer impact

**Not necessarily fix root cause immediately.**

### Error rate/count high

Possible mitigations

- rollback deployment
- disable feature flag
- increase capacity
- route traffic to healthy region
- failover dependency
- rate limit traffic

Example

```
rollback new release
```

or

```
disable expensive feature
```

### System not responding

Possible mitigations

- restart service
- remove unhealthy hosts
- scale fleet
- restart dependency
- clear connection backlog

Example

```
kill stuck process
restart service
```

### System malfunctioning

Possible mitigations

- rollback deployment
- disable feature
- switch to safe mode
- rebuild corrupted cache
- revert config

## Post incident

### Root cause analysis

After service is restored:

Investigate

- logs
- metrics
- deployment history
- system state

Look for

- resource exhaustion
- software bug
- configuration change
- dependency failure
- traffic spike

Use tools

```
perf
strace
heap dump
thread dump
tcpdump
```

### How do we prevent similar issue in the future

Typical improvements:

#### Monitoring

Add alerts for

- latency
- error rate
- CPU
- memory
- queue depth
- dependency health

#### Better observability

Improve

- logging
- metrics
- tracing
- dashboards

Example

- add request tracing

#### Capacity planning

Prevent resource exhaustion

- auto scaling
- load shedding
- rate limiting

#### Resilience

Improve system reliability

- timeouts
- retries
- circuit breakers
- fallbacks

#### Deployment safety

Reduce risk of bad deployments

- canary deploy
- gradual rollout
- feature flags
- automatic rollback

#### Runbooks

Create documentation

- incident playbooks
- debugging steps
- mitigation actions

#### Postmortem

Write incident report

Structure

- what happened
- impact
- root cause
- mitigation
- action items

Goal

- blameless learning
- system improvement

TODO:
- figure out how to make the TOC float on the left hand side

Background context:

- links to various sections of the Manta Operator's Guide
- artedi, metrics, starfish

Incident response quick reference:

- Investigating a decrease in throughput
  - Has the error rate changed?
    Yes: investigate errors
  - Has Muskie tail latency increased?
    Yes: Check for increase in tail latency at electric-moray
      Yes: Check for increase in tail latency at moray
        Yes:
	- Identify which shards are affected.
	- Does it apply uniformly to all instances in the shard?
	  Yes: Investigate PostgreSQL latency
	  No: Examine specific instances with high latency
	  - Are they over 85% CPU utilization?
	    Yes: Is it mostly GC?
	      Yes: There's likely a memory leak.  gcore and restart.
	      No: Are there other instances using much less CPU?
	      - Yes: The load is imbalanced.  Are all instances in DNS?
                  Yes: This is likely a new bug requiring core file analysis of
                       cueball state.
                  No, and the ones not in DNS are the ones that are lightly
                  loaded.
                      Is registrar running in the zones that aren't in DNS?
                      No: Determine why and try to bring up registrar.
                      Yes: Registrar/ZK bug.
            No (not mostly GC): profile it.
        No (no tail latency at Moray):
        - Does electric-moray tail latency affect only some instances?
          No:
          - Is CPU usage above 75% per process (300% per zone) for most
            instances?
            Yes: Pick one.  Is it mostly GC?
	      Yes: There's likely a memory leak.  gcore and restart.
              No:  Profile it.
            Yes: Likely out of capacity.  Deploy more electric-moray instances.
                 Also check whether workload has generally increased with CPU
                 usage.
            No: Trace Electric-Moray processes.
          Yes: find an affected instance (zone and process):
          - Is it over 85% CPU utilization?
            Yes: Is it mostly GC?
	      Yes: There's likely a memory leak.  gcore and restart.
              No:  Profile it.
  - No.  Manta appears healthy.  Check upstream network or whether clients have
    recently changed their workload.
- Investigating an increase in error rate
  - See status code quick reference
  - What type of error is it?
    - 503
      - Manta is reporting that it's overloaded.
      - check for moray queueing
      - check for moray p99 latency
    - 500
      - Find and investigate an individual request.
    - XXX status code for out-of-space
    - XXX status code for no storage nodes available (may be the same; may have
      its own decision tree based on minnow records)
    - 499
      - Client abandoned upload.
      - Check for muskie p99 latency
    - 412, 404
      - Innocuous.
    - 401/403
      - Likely client problem, but not necessarily.
- Investigating an individual request that failed
  - Do you have response headers?
    - Does it have an x-server-name?
      - No: loadbalancer returned the error
	- find loadbalancer log entry
      - Yes: find muskie log entry
	- go through various example stack traces

Subtrees:
- Finding and investigating an individual request
    - do you already have one? (see tree above involving headers)
    - try to generate one using node-manta
    - try mlive
    - scan the muskie logs for one
- Checking for GC / memory leak
  - Memory leak sub-tree:
    - Check if the process is on-CPU
    - Checking if a process is doing GC
    - Check the vsz/rss
    - Gcore and restart
    - Watch for a few minutes.  Is it better?
      Yes: check other instances for vsz/rss and restart them
      No: maybe leak is too fast?
      - Was there a recent change that may have introduced this?
        Yes: rollback
        No: debug memory leak

- Specific investigation tasks:
  - checking metrics:
    - error rate
    - muskie tail latency
    - moray queue depth
    - moray tail latency
  - finding muskie log entry
  - finding loadbalancer log entry
  - locating metadata for an object
  - locating a particular server
  - locating a particular zone
  - locating a particular database shard
  - debugging elevated PostgreSQL latency
  - Save debugging state and restart a process. (include filing a bug)
  - Checking what's in DNS
  - Checking what's in registrar
  - Why is a process on CPU? (or, profiling a busy process)
  - Why is a process off CPU?

Quick reference:
- HTTP status code quick reference
- Glossary of terms
  - component names: point to operator guide
  - tail latency
  - shards, instances

General FAQs:
- What's with the spikes at the top of the hour?
- What's with PostgreSQL rollbacks?

TODO:
- figure out how to make the TOC float on the left hand side
- add a detailed section about decoding Muskie log entries

Background context:

- links to specific sections of the Manta Operator's Guide
- artedi, metrics, starfish

Subtrees:
- Finding and investigating failing requests
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
- Glossary of terms
  - component names: point to operator guide
  - tail latency
  - shards, instances

General FAQs:
- What's with the spikes at the top of the hour?
- What's with PostgreSQL rollbacks?

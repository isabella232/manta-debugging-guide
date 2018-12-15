Document the graphs / metrics on the most important dashboards.

Background context:
- links to specific sections of the Manta Operator's Guide
- artedi, metrics, starfish
- move operator guide references directly into this guide

General FAQs:
- What's with the spikes at the top of the hour?
- What's with PostgreSQL rollbacks?

At the end, go through a variety of historical failure modes and make sure the
path through the decision tree goes where it should.
- latency at various points in the stack
- load balancer problems
- muskie slowness

- message:
{"phase":"0","what":"phase 0: input \"/khangngu/stor/books/treasure_island.txt\"","code":"InvalidArgumentError","message":"failed to dispatch task: requested image is not available","input":"/khangngu/stor/books/treasure_island.txt","p0input":"/khangngu/stor/books/treasure_island.txt"}
from: https://chatlogs.joyent.us/logs/manta/2018/07/26#00:11:10.748Z

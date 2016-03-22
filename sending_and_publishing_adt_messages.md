# Sending and Publishing ADT Messages

## Configurable Levers

One can configure a series of properties for the various components that can make a difference in the performance of the system.  The following is a list of the configurable items by component.

| Component | Item | Description | Initial Value |
| -- |-- |-- | -- |
| Data Ingress | *Message/minute* | <ul><li>Controlling this in the production environment will not be possible.</li></ul> | 0 |
|  | *Unique Accounts* | <ul><li>unique accounts = number of facilities x number of accounts/facility</li><li>Controlling this in the production environment will not be possible.</li></ul> | 30 = 3x10 |
| Main Router | *Number of routers* |<ul><li>The routers handle creating/destroying temporary queues for account sequences</li></ul> | 1 |
|  | *Prefetch Count* | <ul><li>By default, MassTransit will prefetch 1 message/worker.  In this case, the main router is set up for 1 worker in order to maintain message sequence.</li></ul> | 1 |
| Worker(s) | *Number of workers* | <ul><li>Mass Transit recommends 4 workers/core</li><li>Per process, a Mass Transit bus consumes messages as 1 thread with a pre-fetch count equal by default to the number of workers it services.</li></ul> | 8 |
|  | *Receive Timeout (ms)* | <ul><li>When workers consume from a temporary queue, they indicate a timeout period.  If a message is not received within that period, it will move on to the next account to process.</li></ul> | 250 |
| | *Max Messages to Process* | <ul><li>In addition to a timeout period for moving on to another account, the workers also limit the number of messages they will process before moving on.</li></ul> | 7 |
| | *Max Processing Delay (s)* | <ul><li>This is the number of seconds that a worker will wait while processing a given message.</li></ul> | 0 |
| | *Use Random Processing Delay* | <ul><li>If this is true, the worker will randomly pick a number between 0 and the Max Processing Delay to wait before retrieving the next message.  This simulates random work patterns.</li><li>If this is false, the worker will consistently wait for the number of seconds specified in the Max Processing Delay.</li></ul> | false |
| | *Prefetch Count* | <ul><li>By default, MassTransit will prefetch 1 message/worker.</li><li>This can be increased if the amount of time it takes to fetch is greater than or equal to the amount of time it takes to work a message.  For example, it makes sense to prefetch more than 1 message/worker if the work is 0 ms because it takes longer to fetch than it does to work.</li></ul> | 8 |

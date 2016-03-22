# Sending and Publishing ADT Messages

## Levers

One can configure a series of properties for both the publishers and consumers that can make a difference in the performance of the system.

The following are the available properties.

* **Data Ingress**
  * *Messages/minute*
    * Controlling this in the production environment will not be possible.
  * *Unique Accounts*
    * unique accounts = number of facilities x number of accounts/facility
    * Controlling this in the production environment will not be possible.
* **Main Router**
  * *Number of routers*
    * The routers handle creating/destroying temporary queues for account sequences
  * *Prefetch Count*
    * By default, MassTransit will prefetch 1 message/worker.  In this case, the main router is set up for 1 worker in order to maintain message sequence.
* **Worker(s)**
  * *Number of workers*
    * Mass Transit recommends 4 workers/core
    * Per process, a Mass Transit bus consumes messages as 1 thread with a pre-fetch count equal by default to the number of workers it services.
  * *Receive Timeout (ms)*
    * When workers consume from a temporary queue, they indicate a timeout period.  If a message is not received within that period, it will move on to the next account to process.
  * *Max Messages to Process*
    * In addition to a timeout period for moving on to another account, the workers also limit the number of messages they will process before moving on.
  * *Max Processing Delay (s)* 
    * This is the number of seconds that a worker will wait while processing a given message.
  * *Use Random Processing Delay*
    * If this is true, the worker will randomly pick a number between 0 and the Max Processing Delay to wait before retrieving the next message.  This simulates random work patterns.
    * If this is false, the worker will consistently wait for the number of seconds specified in the Max Processing Delay.
  * *Prefetch Count*
    * By default, MassTransit will prefetch 1 message/worker.
    * This can be increased if the amount of time it takes to fetch is greater than or equal to the amount of time it takes to work a message.  For example, it makes sense to prefetch more than 1 message/worker if the work is 0 ms because it takes longer to fetch than it does to work.

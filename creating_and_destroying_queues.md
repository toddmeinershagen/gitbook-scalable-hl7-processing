# Creating and Destroying Queues

### Overview

Processing HL7 messages requires the creation and deletion of temporary durable queues.  The number required for any given minute is based on the number of unique accounts across all facilities that might arrive.  Data from mid-February to mid-March indicates that 95% of the time there are 28 unique accounts / minute / facility.  Assuming 54 existing clients, there would be 1512 unique accounts / minute most of the time. The future system is planning for 10 times growth which would roughly equate to 15,000 unique accounts / minute.

The following are a set of benchmark tests for creating/destroying unique durable queues against a variable number of existing queues.  Per iteration, this was done 500 times to get a statistically significant sample with 1 warm up.  For each iteration, the number of existing queues increased by 10 times the previous iteration.

The test below was run Saturday, March 19, at 8:15 am CST against a server with the following specs:

```ini
RabbitVersion=3.3.1
ErlangVersion=15B02
Clustered=no
OS=Microsoft Windows 2008 R2 Standard Service Pack 1
Server=rcm41vqperapp01.medassets.com
Processor=Intel(R) Xeon(R) CPU E7-L8867 @ 2.13 GHz
ProcessorCount=2
Memory=4.00 GB RAM
```

### With 0 - 15,000 Queues (Non-clustered)

Below are the results for 4 iterations with 0, 150, 1,500, and 15,000 existing queues.  It was conducted against a non-clustered Rabbit server.

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Xeon(R) CPU E7-L8867  @ 2.13GHz, ProcessorCount=2
Frequency=10000000 ticks, Resolution=100.0000 ns
HostCLR=MS.NET 4.0.30319.34209, Arch=32-bit RELEASE

TargetCount=500  

```
|                Method |        Min |       Mean |     StdDev |          Max | Number Of Existing Queues |
| ---------------------- |----------- |----------- |----------- |------------ |------------------------------ |
| CreateAndDestroyQueue | 16.6170 ms | 22.0060 ms |  5.2344 ms |  83.9826 ms |                             0 |
| CreateAndDestroyQueue | 16.7268 ms | 27.7536 ms | 16.1928 ms | 133.9115 ms |                           150 |
| CreateAndDestroyQueue | 18.0483 ms | 27.5022 ms | 12.8215 ms | 147.6780 ms |                          1,500 |
| CreateAndDestroyQueue | 17.9970 ms | 25.4694 ms |  7.6663 ms | 110.8189 ms |                         15,000 |

### With 0 - 15,000 Queues (clustered)

Below are the results of 4 iterations with 0, 150, 1,500, and 15,000 existing queues.  It was conducted on a Rabbit cluster of two nodes using mirrored clustering.

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Xeon(R) CPU E7-L8867  @ 2.13GHzIntel(R) Xeon(R) CPU E7-L8867  @ 2.13GHz, ProcessorCount=2
Frequency=10000000 ticks, Resolution=100.0000 ns
HostCLR=MS.NET 4.0.30319.34209, Arch=32-bit RELEASE

TargetCount=500  
```

|                Method |        Min |       Mean |     StdDev |         Max | DesiredNumberOfExistingQueues |
| ---------------------- |----------- |----------- |----------- |------------ |------------------------------ |
| CreateAndDestroyQueue | 34.8848 ms | 49.3922 ms | 37.5925 ms | 626.2283 ms |                             0 |
| CreateAndDestroyQueue | 35.3080 ms | 51.9788 ms | 20.9110 ms | 278.8768 ms |                           150 |
| CreateAndDestroyQueue | 35.7645 ms | 49.6903 ms | 15.4349 ms | 186.2789 ms |                          1,500 |
| CreateAndDestroyQueue | 38.9271 ms | 59.7106 ms | 46.4486 ms | 472.7796 ms |                         15,000 |


### With 150,000 Queues

After loading 150,000 existing queues, the memory was at 97%. The server eventually failed and could not return any requests or function properly.  Thus, we were not able to run the create/destroy benchmarks for this scenario.

### With 1,500,000 Queues

Due to memory limitations, we were not able to load or benchmark at this level of queues.

### Conclusions

The performance of creating/destroying queues is not affected by the number of queues that exist. But there are limits to the number of queues that can exist based on the existing memory.  In this case, it appears that 2 GB of RAM will be operating at 97% with 150,000 queues.

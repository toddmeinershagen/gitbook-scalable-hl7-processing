# Creating and Destroying Queues

The following are a set of benchmark tests that create/destroy a unique queue.  This was done 500 times to get a statistically significant sample with 1 warmup for each iteration.  The test below was run Saturday, March 19, at 8:15 am CST against a server with the following specs:

```ini
Rabbit Version=3.3.1
Erlang Version=15B02
Clustered=no
Server=rcm41vqperapp01.medassets.com
Cores=2
RAM=4GB
```

## 1.  With 0 - 15,000 Queues

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Xeon(R) CPU E7-L8867  @ 2.13GHzIntel(R) Xeon(R) CPU E7-L8867  @ 2.13GHz, ProcessorCount=2
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
 
## 4. With 150,000 Queues

We attempted to load 150,000 queues, but the memory was at 97% and the server eventually failed and could not return any requests or function properly.  Therefore, we were not able to run the create/destroy benchmarks for this scenario.

## 5. With 1,500,000 Queues

Due to memory limitations, we were not able to load or benchmark at this level of queues.



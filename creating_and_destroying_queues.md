# Creating and Destroying Queues

The following are a set of benchmark tests that we ran that create/destroy a unique queue.  This was done 500 times to get a statistically significant sample with 1 warmup.

## 1. With No Queues

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, ProcessorCount=4
Frequency=2630644 ticks, Resolution=380.1351 ns
HostCLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE
```
|                Method |        Min |        Mean |      StdDev |         Max |
|---------------------- |----------- |------------ |------------ |------------ |
| CreateAndDestroyQueue | 86.9532 ms | 195.6182 ms | 104.5288 ms | 586.2241 ms |



## 2. With 1,500 Queues

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, ProcessorCount=4
Frequency=2630644 ticks, Resolution=380.1351 ns
HostCLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE
```
|                Method |        Min |        Mean |     StdDev |         Max |
|---------------------- |----------- |------------ |----------- |------------ |
| CreateAndDestroyQueue | 77.3750 ms | 184.9941 ms | 98.1808 ms | 553.9298 ms |


## 3. With 15,000 Queues

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, ProcessorCount=4
Frequency=2630644 ticks, Resolution=380.1351 ns
HostCLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE
```
|                Method |        Min |        Mean |     StdDev |         Max |
|---------------------- |----------- |------------ |----------- |------------ |
| CreateAndDestroyQueue | 79.5893 ms | 187.5949 ms | 97.3382 ms | 529.2369 ms |

## 4. With 75,000 Queues

```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, ProcessorCount=4
Frequency=2630644 ticks, Resolution=380.1351 ns
HostCLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE
```
|                Method |        Min |        Mean |      StdDev |         Max |
|---------------------- |----------- |------------ |------------ |------------ |
| CreateAndDestroyQueue | 86.8951 ms | 206.0526 ms | 106.9076 ms | 601.3068 ms |

## 4. With 150,000 Queues

We attempted to load 150,000 queues, but the memory was at 97% and the server eventually failed and could not return any requests or function properly.  Therefore, we were not able to run the create/destroy benchmarks for this scenario.

## 5. With 1,500,000 Queues

Due to memory limitations, we were not able to load or benchmark at this level of queues.


```ini
BenchmarkDotNet=v0.9.2.0
OS=Microsoft Windows NT 6.1.7601 Service Pack 1
Processor=Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, ProcessorCount=4
Frequency=2630664 ticks, Resolution=380.1322 ns
HostCLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE

TargetCount=500  

```
|                Method |         Min |        Mean |     StdDev |         Max | Existing Queues |
|---------------------- |------------ |------------ |----------- |------------ |------------------------------ |
| CreateAndDestroyQueue | 237.4009 ms | 422.1171 ms | 64.7106 ms | 762.9264 ms |                             0 |
| CreateAndDestroyQueue | 219.3089 ms | 411.2985 ms | 74.0306 ms | 929.0092 ms |                           150 |
| CreateAndDestroyQueue | 247.0323 ms | 418.7872 ms | 59.2265 ms | 602.9717 ms |                          1,500 |
| CreateAndDestroyQueue | 198.5187 ms | 373.2021 ms | 65.8538 ms | 713.4165 ms |                         15,000 |
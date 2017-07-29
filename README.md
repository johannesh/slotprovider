# slotprovider

## SlotProvider
Experimental package.
Manages a number of free slots which can be acquired and released concurrently.
There are 3 implementations
 - Using Mutex
 - Using 1 channel directly (2 x slower)
 - Using 2 channels and go routine to coordinate (30 x slower)

### Benchmark results:
```
Benchmark_Mutex-4               30000000                43.7 ns/op
Benchmark_SingleChan-4          20000000                71.6 ns/op
Benchmark_MultiChan-4            1000000              1364 ns/op
```

## Usage
Usage of channel based implementation
```
nrOfSlots := 2
var sp slotprovider.SlotProvider
// mutex based implementation:
sp = slotprovider.NewWithMutex(nrOfSlots)
sp = slotprovider.NewWithSingleChannel(nrOfSlots)
sp = slotprovider.NewWithMultiChannel(nrOfSlots, context.Background())


hasSlot := sp.AcquireSlot() // hasSlot2 = true
hasSlot2 := sp.AcquireSlot() // hasSlot2 = true
hasSlot3 := sp.AcquireSlot() // hasSlot3 = false

sp.release()

hasSlot4 := sp.AcquireSlot() // hasSlot4 = true
```
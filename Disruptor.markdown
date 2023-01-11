---
layout: page
title: Disruptor notes
date: 2023-01-06 22:57:00 +0800
permalink: /Java/Disruptor
---

### Disrupter
1.`RingBuffer`

2.`Executor`

3.`ConsumerRepository `
One concrete class is `EventProcessorInfo`
Provides a repository mechanism to associate `EventHandlers` with `EventProcessors`
- `batchEventProcessor`
- `eventHandler`


### EventProcessor
*Has*
- `RingBuffer`
- `SequenceBarrier`
- `EventHandler`
- `Sequence`

*Implementation*
- `BatchEventProcessor`

### RingBuffer
Ring based store of reusable entries containing the data representing
an event being exchanged between event producer and `EventProcessor`s.

*Field*

- `EventFactory`
- `Sequencer`
- `Object[]`

*Method*
- publish

*Tips*
- `Sequence & indexMask` means Sequence `mod` indexMask

- In the following code
```
protected final E elementAt(long sequence)
    {
        return (E) UNSAFE.getObject(entries, REF_ARRAY_BASE + ((sequence & indexMask) << REF_ELEMENT_SHIFT));
    }
```

`<< REF_ELEMENT_SHIFT` equals to `* scale`

It locates the object by array index

### RingBufferFields

*Field*
- `int REF_ARRAY_BASE`
- `int REF_ELEMENT_SHIFT`
- `BUFFER_PAD`???

**Question**

1. Why the size of entries is `sequencer.getBufferSize() + 2 * BUFFER_PAD`

### Sequencer
Coordinates claiming sequences for access to a data structure while tracking dependent `Sequence`s.

*Fields*
- `sequence`
- `int[] availableBuffer`

*implementation*
- MultiProducerSequencer

*Field*




### Sequence
Concurrent sequence class used for tracking the progress of the ring buffer and event processors

padding ?

### SequenceBarrier
Coordination barrier for tracking the cursor for publishers and sequence of
dependent `EventProcessor`s for processing a data structure

*Field*
- sequencer
- sequence
- waitStrategy

*Method*
- waitFor


*implementation*
- ProcessingSequenceBarrier

### SequenceGroups
Provides static methods for managing a `SequenceGroup` object.

### AtomicReferenceFieldUpdater

A reflection-based utility that enables atomic updates to
designated {@code volatile} reference fields of designated
classes.  This class is designed for use in atomic data structures
in which several reference fields of the same node are
independently subject to atomic updates. For example, a tree node
might be declared as

```
class Node {
    private volatile Node left, right;
 
    private static final AtomicReferenceFieldUpdater<Node, Node> leftUpdater =
      AtomicReferenceFieldUpdater.newUpdater(Node.class, Node.class, "left");
    private static AtomicReferenceFieldUpdater<Node, Node> rightUpdater =
      AtomicReferenceFieldUpdater.newUpdater(Node.class, Node.class, "right");
 
    Node getLeft() { return left; }
    boolean compareAndSetLeft(Node expect, Node update) {
      return leftUpdater.compareAndSet(this, expect, update);
    }
    // ... and so on
  }}
```

### WaitStrategy
Strategy employed for making `EventProcessor`s wait on a cursor `Sequence`.

Implementation
- `BlockingWaitStrategy`


### EventHandler
Callback interface to be implemented for processing events as they become available in the `RingBuffer`

*Implementation*
- `LongEventHandler`

### EventProcessorInfo
has `BatchEventProcessor`

### BatchEventProcessor (one of the key class)

### AtomicReferenceFieldUpdater

### SequenceGroup
A `Sequence` group that can dynamically have `Sequence`s added and removed while being
thread safe.


### SequenceGroups

### Unsafe
Refer article 

[the-unsafe-class-unsafe-at-any-speed][Unsafe at any speed]

[Guide to Unsafe][Baeldung unsafe]

[High performance][High performance]

putOrderedLong

putLongVolatile


[Unsafe at any speed]: https://blogs.oracle.com/javamagazine/post/the-unsafe-class-unsafe-at-any-speed
[Baeldung unsafe]: https://www.baeldung.com/java-unsafe
[High performance]: https://www.jianshu.com/p/9883e53e3b31

- Directly access CPU and other hardware features
- Create an object but not run its constructor
- Create a truly anonymous class without the usual verification
- Manually manage off-heap memory
- Do many other seemingly impossible things

#### Obtaining an instance of the Unsafe
```
Field f = Unsafe.class.getDeclaredField("theUnsafe");
f.setAccessible(true);
unsafe = (Unsafe) f.get(null);
```

#### Instantiating a Class Using Unsafe
```
Field field = Unsafe.class.getDeclaredField("theUnsafe");
field.setAccessible(true);
return (Unsafe) field.get(null);
```
#### Altering Private Fields

#### Throwing an Exception

#### Off-Heap Memory

#### CompareAndSwap Operation

```
public final int getAndAddInt(Object var1, long var2, int var4) {
        int var5;
        do {
            var5 = this.getIntVolatile(var1, var2);
        } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

        return var5;
    }
```
#### Park/Unpark
Very simliar to Object.wait(). But it is calling the native OS code,
thus taking advantage of some architecture specifics to get the best performance.
When the thread is blocked and needs to be made runnable again, the JVM uses the unpark() method. 


### AccessController
`doPrivileged`


Memory Barrier
```
public native void loadFence();
public native void storeFence();
public native void fullFence();
```

#### Methods of Unsafe
- arrayBaseOffset

方法是一个本地方法，可以获取数组第一个元素的偏移地址。

- arrayIndexScale

可以获取数组的转换因子，也就是数组中元素的增量地址。



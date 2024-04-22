# GPU的硬件基本术语解析
SP（streaming Process），SM（streaming multiprocessor）是硬件（GPU hardware）概念。而thread，block，grid，warp是软件上的（CUDA）概念。

## 硬件概念
- streaming processor（sp）
最基本的处理单元，也称为CUDA core。最后具体的指令和任务都是在sp上处理的。GPU进行并行计算，也就是很多个sp同时做处理。现在SP的术语已经有点弱化了，而是直接使用thread来代替。一个SP对应一个thread。

- streaming multiprocessor（sm）
多个sp加上其他的一些资源组成一个sm, 也叫GPU大核。其他资源也就是存储资源，共享内存，寄储器（warp scheduler，register，shared memory）等。SM可以看做GPU的心脏（对比CPU核心），register和shared memory是SM的稀缺资源。CUDA将这些资源分配给所有驻留在SM中的threads。因此，这些有限的资源就使每个SM中active warps有非常严格的限制，也就限制了并行能力。

## 软件概念
- warp
GPU执行程序时的调度单位，目前cuda的warp的大小为32，同在一个warp的线程，以不同数据资源执行相同的指令。

- grid、block、thread
在利用cuda进行编程时，一个grid分为多个block，而一个block分为多个thread。其中任务划分到是否影响最后的执行效果。划分的依据是任务特性和GPU本身的硬件特性。

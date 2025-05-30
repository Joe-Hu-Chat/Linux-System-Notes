---
typora-copy-images-to: ./.42_memory_management
---

2024年2月18日

# 内存管理

![img](.\.42_memory_management\wps3.jpg)

在Linux系统中，虚拟内存和物理内存都是由kernel管理的，当进程需要分配内存时，都需要通过系统调用陷入内核空间来分配，再将虚拟内存起始地址返回给用户态。内核提供了多个系统调用来分配虚拟内存，包括**brk**, **mmap**和**mremap**等。

在Linux操作系统标准**libc**库(glibc)中，`malloc`函数的实现中会根据分配内存的size来决定使用哪个分配函数，当size小于/等于128KB，调用`brk`分配，当size大于**128KB**时，调用`mmap`分配内存。



## 系统调用 (syscall)

The `brk` and `mmap` system calls are both used for memeory management in Unix-like operating systems, but they serve different purposes and have distinct charcteristics.

### brk

The `brk` system call is used to adjust the size of the data segment (heap) of a process by changing the program's "break" value.

The "program break" is the address of the first memory location beyond the current end of the heap. By increasing or decreasing the break value, the heap size can be adjusted dynamically during program execution. Newly allocated memory is initialized to zero.

Memory allocated via `brk` must be configuous with the existing heap, which can lead to fragmentation issues.

### mmap

The `mmap` system call maps **files** or **anonymous memory** into a process's address space, providing more flexible memory allocation.

It can allocate memory by mapping a file or using an anonymous mapping (with `MAP_ANON`). The allocated memory does not need to be configuous with other regions in the process's address space. Memory is lazily loaded (demand paging), meaning physical memory is only allocated when accessed.

Slightly slower than `brk` for small alllcations due to additional overhead (e.g., managing page tables).



## 分配器（allocator）

在`malloc`和系统调用之间插入一层中间件。分配器首先通过系统调用从内核批发大块内存，然后切成不同大小的内存片**缓存**起来，例如8/16/24/32/64byte等，当调用` malloc`的时候，直接从cache的空间内存片中分配；同时，为了解决性能问题，分配器对每个线程<span style="color:red">或者</span>每个cpu预留单独的cache，每个线程从自己的cache中分配，可以减少线程之间的锁竞争。

![img](.\.42_memory_management\wps7.jpg)

现在业界主流的分配器有ptmalloc, tcmalloc, jemalloc, scudo等。（这些都在glibc库中）

> [!CAUTION]
>
> 内核的分配器与用户空间/库函数的分配器都是什么？

内核没有使用glibc中提供的库函数的实现，而是自己实现了一套所需要的各种功能的库函数。因此，内核和用户空间的程序有各自的分配器。



### buddy system

Buddy System是一种程序，其中两个人作为一个单元一起操作，以便他们能够相互监视和帮助。 Buddy System的主要优点是提高安全性，每个人都可以防止另一个人成为伤亡者或在危机中拯救另一个人。当这个系统作为培训或新来组织的新手引导的一部分时，经验不足的伙伴通过与经验丰富的伙伴的密切和频繁的接触更快地学习，而不是独自操作。 Buddy System在美国武装部队中使用，并在每个分支中以不同的名称（空军中的“Wingmen”，陆军中的“Battle Buddies”，海军中的“Shipmates”）以及美国男童军和美国女童军中使用。

此外，Buddy System还被广泛用于新员工的引入和知识共享。它涉及将新员工分配给工作场所的伙伴。伙伴是指导新项目经理在工作的前几周或几个月中的现有员工。 Buddy System是一种有效的方法，可以提供支持，监测压力并强化安全程序。

Buddy System Memory Allocator是指用二进制拆分的方法来管理内存。以2幂次方大小的形式组织这些内存块，每个块都有一个大小相等的伙伴。

### slab allocator

预先分配一些常见大小的内存块（slabs）来管理内存。

SLUB Sparse, Local, and Unreliable Buddy allocator.


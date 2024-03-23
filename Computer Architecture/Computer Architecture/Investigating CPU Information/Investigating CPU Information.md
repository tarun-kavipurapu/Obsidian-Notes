Alright GPT gave me commands for further finding out about my CPU Architecture

```JavaScript
lscpu
cat /proc/cpuinfo
```

lscpu gave me this

![[Images-attachments/Untitled 39.png|Untitled 39.png]]

Lets Investigate one-by-one

1. **Architecture**:x86_64 as discussed before it Represent x86 intel architecture and 64bit represents that it has capability for 64bit instruction set.(Further investigate about x86 and instruction set length)
2. **CPU op-mode(s)**:

- The CPU supports both 32-bit and 64-bit operation modes, allowing it to run software designed for both architectures.

1. **Address sizes**:

[[Physical Address and Virtual Address]]

- The physical address size is 39 bits, meaning the CPU can address up to 2^39 bytes of physical memory. The virtual address size is 48 bits, allowing the CPU to address up to 2^48 bytes of virtual memory.

1. **Byte Order**:

- The byte order is Little Endian, which means the least significant byte of a multi-byte value is stored at the lowest memory address.

1. **CPU(s)**:(Investigate more about hyper-threading technology)
    - There are 12 CPU cores in total.
    - Each core supports 2 threads, indicating support for hyper-threading technology.
    - The CPU is from the 11th generation Intel Core family, with the model name being "i5-11400H."
2. **CPU Max/Min MHz**:

- The maximum and minimum clock speeds of the CPU are 4500.0000 MHz and 800.0000 MHz, respectively.
- **Flags**:
    - These are various CPU feature flags indicating supported features and instructions. Some notable flags include SSE, SSE2, AVX, AVX2, AES, and others, which represent different instruction set extensions and capabilities of the CPU.
- **Virtualization features**:
    - The CPU supports VT-x, indicating hardware-assisted virtualization capabilities.
- **Caches**:
    - The CPU has multiple levels of cache memory, including L1d (data cache), L1i (instruction cache), L2, and L3 caches. The sizes of these caches are provided.
- **NUMA**:
    - NUMA (Non-Uniform Memory Access) information is provided, indicating the presence of a single NUMA node with all CPU cores belonging to it.
- **Vulnerabilities**:
    - Various vulnerabilities and their mitigations are listed, including data sampling, Speculative Store Bypass, Spectre, Meltdown, and others. Mitigations are applied where necessary to protect against these vulnerabilities.
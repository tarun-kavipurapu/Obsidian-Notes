```JavaScript
uname -m in linux gives me ---> x86_64
```

I got to know that x86—> architecture 64—>instruction bit capability

Two types of architecture x86 and ARM(advanced RISC Machine)

x86 is a CISC based machine (let us investigate about CISC and RISC more in depth)

i saw from chatgpt that ARM is mostly used in mobile phones due to its less resources and power so lets investigate and find out architecture of my mobile phone

for mo

```JavaScript
uname -m --> for mobile gives me arrch64
```

The output "aarch64" from the `uname -m` command indicates that your mobile phone is running on an ARMv8-A architecture, which is a 64-bit variant of the ARM architecture.

---

Here's what it breaks down to:

- **ARM Architecture**: ARM (Advanced RISC Machine) is a family of reduced instruction set computing (RISC) architectures for computer processors, configured for various environments. It's commonly used in mobile devices, embedded systems, and increasingly in other computing platforms.
- **ARMv8-A**: This refers to the ARM architecture version 8, which introduced several enhancements over previous versions, including support for 64-bit computing. The "A" designation signifies that it's an application profile, optimized for general-purpose computing tasks.
- **64-bit**: The "64" indicates that the processor supports 64-bit instructions, enabling it to handle larger amounts of memory and perform calculations with higher precision compared to 32-bit processors.

So, "aarch64" essentially means a 64-bit version of the ARMv8-A architecture, which is commonly found in modern mobile devices running 64-bit operating systems like Android or iOS.

**So it raised a doubt with in me whether Iphone also uses ARM or not so chat-gpt gave me this answer**

---

Yes, iPhones and other iOS devices use ARM architecture for their processors. Since the launch of the original iPhone in 2007, Apple has utilized various iterations of ARM-based processors in their mobile devices. These processors are custom-designed by Apple and are known as Apple Silicon, featuring optimizations tailored for their devices and operating system.

Apple transitioned from using third-party ARM-based processors to designing their own custom chips for iPhones and iPads with the introduction of the Apple A4 chip in 2010. Since then, they have continued to develop and refine their custom ARM-based processors, leading to significant performance and efficiency improvements with each new generation.

In June 2020, Apple announced their plan to transition their Mac lineup from Intel processors to custom-designed Apple Silicon chips, further solidifying their commitment to ARM architecture across their entire product ecosystem. This move allows Apple to have greater control over the performance, power efficiency, and integration of hardware and software across their devices.

---

Before Going Deep Into What is RISC And What is CISC let me investigate more about my PC and mobile architecture
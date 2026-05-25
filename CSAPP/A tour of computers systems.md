---
share_link: https://share.note.sx/bmsu3zv4#VAg2UBKkr63ZIyZe/lqvbZys6ErleSuBtB3HztKYcOg
share_updated: 2026-04-14T22:05:42+02:00
---

## ==Information is bits==

Our code starts as a source file , Source files are organized in 8bit chucks called Bytes , Each byte represents some text character in the program.

All the information written in our code is then later stored in memory.

>Same sequence of bytes can represent an integer, floating point , string , machine structure. So we got to know how systems interpret those numbers

## ==Program compilation phase==

C statements must be translated by other programs into a sequence of low-level machine language instructions. These instructions are then packaged in a form called an executable object program and stored as a binary disk file.

The translation from source file to object file is performed by the compiler driver.
For example, the GCC compiler driver reads the source file hello.c and translates it into an executable object file hello.

The translation is performed in 4 phases ( **Preprocessor , compiler , assembler , linker** )

![[Pasted image 20260406182255.png]]

**Preprocessor Phase**: The preprocessor (cpp) modifies the original C program according to directives that begin with the ‘#’ character. For-example, the \#include command in line1 of `hello.c` tells the preprocessor to read the contents of the system header file `stdio.h` and insert it directly into the program text. The result is another C program , typically with the .i suffix.

**Compiler phase**: The compiler (cc1) translates the text file `hello.i` into the text file `hello.s` , which contains an assembly language program (assembly representation of the original program).

**Assembly phase**: the assembler (as) translates `hello.s` into machine language instructions , packages them in a form known as a relocatable object program , and stores the result in the object file `hello.o` 

**Linkage phase**: Notice that our hello program calls the printf function, which is part of the standard C library provided by every C compiler. The printf function resides in a separate precompiled object file called `printf.o`, which must some how be merged with our `hello.o` program. The linker (ld) handles this merging. The result is a ready to use executable.

## ==**Understanding the compilation phase**==

We can rely on the compilation system to produce correct and efficient machine code. However, there are some important reasons why programmers need to understand how compilation systems work:

1. **Optimizing program performance**: Modern compilers are sophisticated tools that usually produce good code. However, in order to make good coding decisions in our C programs, we do need a basic understanding of machine-level code and how the compiler translates different C statements into machine code. For example, is a switch statement always more efficient than a sequence of if-else statements? We will later describe how compilers translate different C constructs into this language, tune the performance of your C programs , learn how C compilers store data arrays in memory, and how your C programs can exploit this knowledge to run more efficiently.
   
2. **Understanding link-time errors**: some of the most complex programming errors are linker related error, For example, what does it mean when the linker reports that it cannot resolve a reference? What is the difference between a static variable and a global variable? What happens if you define two global variables in different C files with the same name? All covered in chapter 7

3. **Avoiding security holes**: For many years, buffer overflow vulnerabilities have accounted for many of the security holes in network and Internet servers. These vulnerabilities exist because too few programmers understand the need to carefully restrict the quantity and forms of data they accept from untrusted sources. We cover the stack discipline and buffer overflow vulnerabilities in Chapter 3 as part of our study of assembly language. We will also learn about methods that can be used by the programmer , compiler , and operating system to reduce the threat of attack.

## ==**Hardware Organization of a System**==

```c
#include <stdio.h>

int main(){
	printf("Hello world\n");
	return 0;
}
```

At this point, our `hello.c` source program has been translated by the compilation system into an executable object file called hello that is stored on disk.

To understand what happens to our hello program when we run it, we need to understand the hardware organization of a typical system:

![[Pasted image 20260409223513.png]]

#### **Buses**: 
Running throughout the system is a collection of electrical conduits called buses that carry bytes of information back and forth between the components. Buses are typically designed to transfer fixed-size chunks of bytes known as words. The number of bytes in a word (the word size) is a fundamental system parameter that varies across systems. Most machines today have word sizes of either 4 bytes (32 bits) or 8 bytes (64 bits).

>Motherboard factories decide the Data buss width which is typically 8 bytes per word. “word” (hardware term) and “word” (data type in programming) are related but not the same thing.

#### **I/O devices**:
Input/output (I/O) devices are the system’s connection to the external world. Each I/O device is connected to the I/O bus by either a controller or an adapter. **Controllers** are chip sets in the device itself or on the system’s main printed circuit board (often called the motherboard). **An adapter** is a card that plugs into a slot on the motherboard. Regardless, the purpose of each is to transfer information back and forth between the I/O bus and an I/O device.

>Chapter 6 has more to say about how I/O devices such as disks work.
>Chapter10, you will learn how to use the Unix I/O interface to access devices from your application programs.

#### **Main Memory**:
The main memory is a temporary storage device that holds both a program and the data it manipulates while the processor is executing the program. Physically, main memory consists of a collection of dynamic random access memory (DRAM) chips. Logically, memory is organized as a linear array of bytes, each with its own unique address (array index) starting at zero.

>Chapter 6 has more to say about how memory technologies such as DRAM chips work, and how they are combined to form main memory

#### **Processor**:
The central processing unit (CPU), or simply processor, is the engine that interprets (or executes) instructions stored in main memory. At its core is a word-size register called the program counter (PC). From the time that power is applied to the system until the time that the power is shutoff, a processor repeatedly executes the instruction pointed at by the program counter and updates the program counter to point to the next instruction.

Executing a single instruction involves multiple steps done by the CPU:
- Read , The processor reads the instruction from memory pointed at by the program counter.
- Interpretation , interprets the bits in the instruction.
- Executing , performs some simple operation dictated by the instruction.
- Updating , updates the PC to point to the next instruction.

#### **Processor affiliates**:
There are only a few of simple operations that a CPU can execute , and they revolve around main memory, the register file, and the arithmetic/logic unit (ALU).

**The register file** is a small storage device that consists of a collection of word-size registers, each with its own unique name.
**The ALU** computes new data and address values , and is responsible for executing simple athematic operations.

#### **Processor Execution**:

Here are some examples of the simple operations that the CPU might carry out at the request of an instruction:

- Load: Copy a byte or a word from main memory into a register, overwriting the previous contents of the register.
- Store: Copy a byte or a word from a register to a location in main memory, overwriting the previous contents of that location
- Operate: Copy the contents of two registers to the ALU, perform an arithmetic operation on the two words, and store the result in a register, overwriting the previous contents of that register.
- Jump: Extract a word from the instruction itself and copy that word into the program counter (PC), overwriting the previous value of the PC

The shell then loads the executable hello file by executing a sequence of instructions that copies the code and data in the hello object file from disk to main memory. The data includes the string of characters "hello, world\n" that will eventually be printed out.

Using a technique known as direct memory access the data travel directly from disk to main memory, without passing through the processor. Once the code and data in the hello object file are loaded into memory, the processor begins executing the machine-language instructions in the hello program’s main routine. These instructions copy the bytes in the hello, world\n string from memory to the register file ,and from there to the display device ,where they are displayed on the screen.

![[Pasted image 20260410001449.png]]

#### **Caches**:
A system spends a lot of time moving information from one place to another. The machine instructions in the hello program are originally stored on disk. When the program is loaded, they are copied to main memory. As the processor runs the program, instructions are copied from main memory into the processor. Similarly, the data string "hello,world\n",

From a programmer’s perspective ,much of this copying is overhead that slows down the “real work” of the program also larger storage devices are slower than smaller storage devices , For example, the disk drive on a typical system might be 1,000 times larger than the main memory, but it might take the processor 10,000,000 times longer to read a word from disk than from memory.

All of those are factors that lead to slower execution and its an exponential downfall for performance

To deal with the processor–memory gap, system designers include smaller, faster storage devices called cache memories (or simply caches) that serve as temporary staging areas for information that the processor is likely to need in the near future.

![[Pasted image 20260410105319.png]]

in a typical system. An L1 cache on the processor chip holds tens of thousands of bytes and can be accessed nearly as fast as the register file. A larger L2 cache with hundreds of thousands to millions of bytes is connected to the processor by a special bus.

It might take 5 times longer for the processor to access the L2 cache than the L1 cache, but this is still 5 to 10 times faster than accessing the main memory.

The L1 and L2 caches are implemented with a hardware technology known as static random access memory (SRAM).

>Newer and more powerful systems even have three levels of cache: L1, L2, and L3.

The idea behind caching is that a system can get the effect of both a very large memory and a very fast one by exploiting *locality* (the tendency for programs to access data and code in localized regions)

## ==**Memory hierarchy**==

This notion of inserting a smaller, faster storage device (e.g., cache memory) between the processor and a larger, slower device (e.g., main memory) turns out to be a general idea.
In fact, the storage devices in every computer system are organized as a memory hierarchy.

![[Pasted image 20260410110444.png]]

As we move from the top of the hierarchy to the bottom, the devices become slower, larger, and less costly per byte. Every storage level holds cache from the storage device below it and are categorized by levels , ex CPU registers are L0 (Level 0) as its the closest to the CPU.

## ==**Operating system and hardware**==

We can think of the operating system as a layer of software imposed between application and hardware for 4 main reasons:
	1. To protect the hardware from any miss-use be applications
	2. To add a simple uniform way for applications to communicate with hardware
	3. Leverage the hardware for max performance and optimize system resources
	4. Add a layer of abstraction

![[Pasted image 20260414204322.png]]

We can see the Virtual memory being an abstraction of Main memory and I/O Devices , same for processes being an abstraction of CPU , Main memory , I/O Devices
#### **Processes**:

A process is the operating system’s abstraction for a running program. Multiple processes can run concurrently on the same system, and each process appears to have exclusive use of the hardware.

When a program such as hello runs on a modern system, the operating system provides the illusion that the program is the only one running on the system. The program appears to have exclusive use of both the processor, main memory, and I/O devices. The processor appears to execute the instructions in the program, one after the other, without interruption. And the code and data of the program appear to be the only objects in the system’s memory.

Modern CPUs can run multiple processes concurrently at the same time , this is achieved by having a multi core CPU where each core has a processor that runs the instructions of a process. Even in Single core CPUs we can run multiple processes by having the processor switch between processes , this is called ==*context switch*== mechanism.

**How does Context switching work ?** When the CPU decides to change the execution to another process , some important data must be saved to keep track of the current process execution flow such as PC value , registers , and content of main memory. When the CPU saves the data of the previous process , it loads the data of the new process to run and the new process runs from where it left off (if it wasn't newly loaded)

![[Pasted image 20260414205917.png]]

**How can we perform a context switch ?** Suppose we have 2 processes ==*shell*== and ==*hello*== program , the shell is waiting for us to run an application , Once we put the application name , the shell does a special function called a *system call* where it passes the control to the operating system and the CPU does the context switch to run our hello program , once it finishes it returns the shell program context and the shell waits for input again.

the transition from one process to another is man aged by the operating system ==*kernel*== . The kernel is the portion of the operating system code that is always resident in memory. When an application program requires some action by the operating system, such as to read or write a file, it executes a special system call instruction, transferring control to the kernel. The kernel then performs the requested operation and returns back to the application program.

#### **Threads:**

We said program execute instructions , but that statement was somewhat incorrect , Processes contain inner objects called ==*Threads*== that actually execute code , They can even contain different code and execute them under the same process name. Each thread has its own context and its own memory space aswell. Biggest advantage of threads is that they allow concurrency in a single process meaning running code at the same time which can decrease the load from one thread and increase performance.

#### **Virtual Memory:**

![[Pasted image 20260414212601.png]]

==*Virtual memory*== is an abstraction that provides each process with the illusion that it has exclusive use of the main memory. Each process has the same uniform view of memory, which is known as its ==*virtual address space*==.

The virtual address space seen by each process consists of a number of well defined areas, each with a specific purpose:
	- **Program Code and Data**: Code begins at the same fixed address for all processes, followed by data locations that correspond to global C variables. The code and data areas are initialized directly from the contents of an executable object file—in our case, the hello executable
	- **Heap**: The code and data areas are followed immediately by the run-time heap. Unlike the code and data areas, which are fixed in size once the process begins, its dynamic meaning it contracts and expands on allocations in run time
	- **Share libraries**: Near the middle of the address space is an area that holds the code and data for shared libraries such as the C standard library and the math library
	- **Stack**: At the top of the user’s virtual address space is the user stack that the compiler uses to implement function calls. Like the heap, the user stack expands and contracts dynamically during the execution of the program. In particular, each time we call a function, the stack grows. Each time we return from a function, it contracts.
	- **Kernel Virtual Memory**: The top region of the address space is reserved for the kernel. Application programs are not allowed to read or write the contents of this area or to directly call functions defined in the kernel code. Instead, they must invoke the kernel to perform these operations.

For virtual memory to work, a sophisticated interaction is required between the hardware and the operating system software, including a hardware translation of every address generated by the processor. The basic idea is to store the contents of a process’s virtual memory on disk and then use the main memory as a cache for the disk. This terminology is called ==*Mapping*==

#### **Files:**

A file is a sequence of bytes, nothing more and nothing less. This simple and elegant notion of a file is nonetheless very powerful because it provides applications with a uniform view of all the varied I/O devices that might be contained in the system. Every I/O device, including disks, keyboards, displays, and even networks, is modeled as a file. All input and output in the system is performed by reading and writing files.

## ==**Network**==

network can be viewed as just another I/O device, When the system copies a sequence of bytes from main memory to the network adapter, the data flow across the network to another machine, instead of, say, to a local disk drive. Similarly, the system can read data sent from other machines and copy these data to its main memory.

![[Pasted image 20260414214451.png]]

## ==**Amdahl's law**==

Gene Amdahl, one of the early pioneers in computing, made a simple but insightful observation about the effectiveness of improving the performance of one part of a system. The main idea is that when we speed up one part of a system, the effect on the overall system performance depends on both how significant this part was and how much it sped up.

![[Pasted image 20260414215048.png]]

T-old -> Time application requires to run
a -> The required time by some system part to perform it's purpose
k -> Percentage increase in performance in part *a*
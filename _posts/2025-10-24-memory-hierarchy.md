---
layout: default
title: "Memory hierarchy"
permalink: /memory-hierarchy/
author: energyc0
---

### Why should I know all of this?

Every system programmer must know how memory works. This is an optimization issue. Processor spends time not only processing data, but also moving it. Speed of applications depend on this.

### Memory hierarchy

![Computer memory hierarchy](/assets/hierarchy_algorithmica.png)

Memory hierarchy can be represented as a pyramid. On the top of it is the fastest and smallest data storage type and at the bottom is the slowest and biggest data storage type.

#### Primary storage

CPU Registers, CPU Caches and RAM represent **Primary storage**. Primary storage, including *ROM*, *EEPROM*, and *NOR flash*, is usually byte-addressable. Such memory is usually connected to the CPU via memory bus, comprising an address bus and a data bus.

![Computer storage types](/assets/computer_storage_types.png)

- **CPU Registers** is the most expensive and fast, but smallest storage type. Their capacity is typically 16-64 bits. CPU instructions instruct the *arithmetic logic unit* to perform various calculations or other operations on this data.

![Execute cycle CPU](/assets/execute-cycle-cpu.png)

- **Cache memory** is small, high-speed area in a computer. It stores copies of data from frequently used main memory locations. There are various independent caches in a CPU. The concept of cache memory is to load nearby items because they are likely to be accessed next.

![Cache memory and main memory](/assets/cache-memory-and-main-memory.jpg)

- **RAM (Random Access Memory)** is a primary memory of a computer system. There are two types of RAM:
    1. **SRAM (Static RAM)**
    2. **DRAM (Dynamic RAM)**

    SRAM is faster than DRAM and holds data permanently in the presence of power while data in DRAM decays in seconds and thus must be refreshed periodically. SRAM is more expensive than DRAM in terms of silicon area and cost so SRAM is used as cache memory. DRAM can contain more information than SRAM and is usually used for computer's main memory. These differences are caused by the structure of RAM: for DRAM module you need only one transistor and a capacitor for a bit of data, but for SRAM module you may need six transistors.

- **ROM (Read-Only Memory)** is a type of non-volatile memory. There are a couple of types of ROM:
    1. **Mask ROM** - factory-made mask ROM. The desired data is converted into a custom photomask/mask layer for the final metallization of interconnections on the memory chip (hence the name). 
    2. **PROM (Programmable Read-Only Memory or one-time programmable ROM)** is ROM that can be reprogrammed once by the user.
    3. **EPROM (Eraseable Programmable Read-Only Memory)** is ROM that can be erased or reprogrammed by the user multiple times, but the endurance of most EPROM chips exceeds 1000 cycles of erasing and reprogramming.
    4. **EEPROM (Electrically Erasable Programmable Read-Only Memory)**  is based on a similar semiconductor structure to EPROM, but allows its entire contents (or selected banks) to be electrically erased, then rewritten electrically, so that they need not be removed from the computer (whether general-purpose or an embedded computer in a camera, MP3 player, etc.). Types of EEPROM:
        - **Electrically alterable read-only memory (EAROM)** is a type of EEPROM that can be *modified* one or a few bits at a time. EAROM may be used as non-volatile storage for critical system setup information; in many applications, EAROM has been supplanted by CMOS RAM supplied by mains power and backed up with a lithium battery. 
        - **Flash memory** is a modern type of EEPROM invented in 1984. For flash memory you need to *erase* the area where you are willing to write.
    5. **NVRAM (Non-volatile Random-Access Memory)** is random-access memory that retains data without applied power. There are a few types of NVRAM. The first type of NVRAM is static RAM with a built-in battery (most often lithium) and enhanced protection against information distortion at the time of power on and off. The second is differs from the first that its memory can be replaced into EEPROM when machine is shutting down and from EEPROM memory can be replaced into NVRAM when machine is turning on.
#### Secondary storage

**Secondary storage (also known as auxilliary storage or external memory)** is not directly used by the CPU, but accessed via replacing data to a primary storage with input/output calls (for example asking BIOS to read/write to a sector on a disk). Secondary storage is non-volatile that means it retains data even after shut off. This type of memory is less expensive, can contain more data, but slower than primary storage. Secondary storage is addressable by block and often formatted according to a filesystem format that provides abstraction layer to organize data into files and directories.

In modern computers, **hard disk drives (HDDs)** and **solid-state drives (SSDs)** are usually used as secondary storage. Other examples are *USB flash drives*, *floppy disks*, *magnetic tapes*, *paper tapes*, *punched cards* and *RAM disks*.

- **Hard disk drive (HDD)** is an electro-mechanical data storage device that stores and retrieves digital data using magnetic storage with one or more rigid rapidly rotating platters coated with magnetic material. 

![HDD image](/assets/hdd-image.jpeg)

- **Solid-state drive (SSD)** is a type of solid-state storage device that uses integrated circuits to store data persistently. It is sometimes called *semiconductor storage* device, *solid-state* device, or *solid-state disk*. SSDs rely on *flash NAND* memory to store data. Unlike HDDs they have no moving parts that allow them to access data faster; it reduces latency, power consumption and makes them more silent. However SSDs are more expensive and have less write cycles than hard disk drives.

![SSD image](/assets/ssd-image.jpg)

- **USB flash drive (UFD)** is a data storage device that includes flash memory with an integrated USB interface. Some allow up to 100,000 write/erase cycles, depending on the exact type of memory chip used, and are thought to physically last between 10 and 100 years under normal circumstances.

![USB image](/assets/usb-image.jpg)

- **

![Floppy disk image](/assets/floppy-disk-image.jpg)

![Computer memory hierarchy](/assets/computer_memory_hierarchy.png)
---
author: energyc0
title: "Zero width bitfields"
layout: default
permalink: /zero-width-bitfields/
---

### About bitfields

Bitfields is a very convenient tool in C. For example, you write a program that uses flags and you always need to access bits. You can access them without bit operations by creating a structure with bitfields!


```C
struct my_flag {
    int color : 4;
    int priority : 4;
    int type : 3;
    int is_global : 1;
};
```

Structure ``my_flag`` has size of 4 bytes on my machine. For ``color`` reserved 4 bites, for ``priority`` 4 bits, for ``type`` - 3 and for ``is_global`` - 1 bit.

This is comparable to having an int variable and accessing its bits by bit operations:

```C
int my flag;
...
my_flag & 0xF           // color
(my_flag >> 4) & 0xF    // priority
(my_flag >> 8) & 0xC    // type
(my_flag >> 11) & 0x1   // is_global
```

or you can use

```C
struct my_flag flag;
...
flag.color;
flag.priority;
flag.type;
flag.is_global;
```

### Zero width bitfields

You may have known all of it, but I've got something interesting for you. It is zero width bitfields.

Let's look at our changed struct:

```C
struct my_flag {
    int color : 4;
    int priority : 4;
    int : 0;        // zero width bitfield
    int type : 3;
    int is_global : 1;
};
```

Now size of the structure is 8 bytes on my machine. Zero width bitfield tells the compiler to place following structure fields at new ``int`` boundary. It means that the memory layout of this structure looks like this:

```
=== my_flag structure ===
| - - - - / - - - - |- - ... - -| - - - /     -     / - - - - |- - ... - -|
|  color  /priority |  *empty*  | type  / is_global / *empty* |  *empty*  |
|       1 byte      |  3 bytes  |          1 byte             |  3 bytes  |
|              4 bytes          |                   4 bytes               |
|                               8 bytes total                             |
```

If you change type of zero width bitfield it changes the size boundary.

```C
struct my_flag {
    int color : 4;
    int priority : 4;
    long long : 0;        // zero width bitfield, align to long long int
    int type : 3;
    int is_global : 1;
};
```

Now ``my_flag`` structure is 12 bytes on my machine.

Additionally, you cannot get the address of bitfield variable:
```c
struct my_flag A;
...
&A.color; /* Error: Address of bit-field requested */
```

And you cannot get the offset even if it is aligned to byte boundary:
```c
struct my_flag A;
...
offsetof(struct my_flag, type); /* Error: Cannot compute offset of bit-field 'type' */
```

That's because pointers work with bytes, bits. That's because processor operates with bytes, not bits. Registers are usually 8, 16, 32 ... bits and it is not convenient due to architecture to operate with bits.
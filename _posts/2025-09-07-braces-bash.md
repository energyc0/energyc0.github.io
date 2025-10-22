---
author: energyc0
title: "Braces in bash"
layout: default
permalink: /braces-bash/
---

# Braces in bash
Do you know the difference between *\$\{\}*, *\$()*, *\$(())* and just *\$* in shell?

### Explanation
**\$** - just extracts a variable. Example:
```bash
$ echo $PWD 
/home/energyc0
```

**${}** - also extracts a variable, but with some features. Example:
```bash
$ echo ${PWD}
/home/energyc0
```
Also you can modify variables \"on fly\". This is called **"Parameter Expansion"** - <https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html> and <https://tldp.org/LDP/abs/html/parameter-substitution.html>. Example:
```bash
$ var=abcdef
$ rep='& '
$ echo ${var/abc/& }
abc def
$ echo "${var/abc/& }"
abc def
$ echo ${var/abc/$rep}
abc def
$ echo "${var/abc/$rep}"
abc def

$ A="abababcdef"
$ echo ${A#*ab}
ababcdef
$ echo ${A##*ab}
cdef
```

**\$\(\)** means: "firstly, evaluate this and then the other". Example:
```bash
$ echo "Today is $(date). Beautiful day!"
Today is Sun Sep  7 08:50:37 AM MSK 2025. Beautiful day!
```
So you can use output of different commands in shell to run other commands!

**\$\(\(\)\)** evaluates an expression. Example:
```bash
$ echo $((5*2/4 + 1))
3
```
As you can see, it works with integers.

### One problem
Just explore these examples:
```bash
var="abc"
if [ $var == abc def ]; then
    echo "Victory!"
else
    echo "Defeat!"
fi

bash: [: too many arguments
Defeat!
```
```bash
var="abc"
if [ $var == "abc def" ]; then
    echo "Victory!"
else
    echo "Defeat!"
fi

Defeat!
```
```bash
var="abc def"
if [ $var == "abc def" ]; then
    echo "Victory!"
else
    echo "Defeat!"
fi

bash: [: too many arguments
Defeat!
```
```bash
var="abc def"
if [ ${var} == "abc def" ]; then
    echo "Victory!"
else
    echo "Defeat!"
fi

bash: [: too many arguments
Defeat!
```
```bash
var="abc def"
if [ "$var" == "abc def" ]; then
    echo "Victory!"
else
    echo "Defeat!"
fi

Victory!
```
The only conclusion you can make is that you need "" to use variables correctly.
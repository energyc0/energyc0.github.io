---
author: energyc0
title: "NMOS and PMOS transistors"
layout: default
permalink: /nmos-pmos/
---

## Introduction

In digital electronics, a *circuit* is understood as an electrical circuit that processes digital signals. A circuit is considered as a 'black box' that has:

- one or more digital inputs
- one or more digital outputs
- functional specification (describing correlation between inputs and outputs)
- timing specification (describing delay between changing signals at input and responses of output signals.

Digital circuits are divided into *combinational* circuits and *sequential* circuits. The outputs of combination circuits depend only on the current values at the inputs. he outputs of sequential circuits depend on both the current and previous values at the inputs, that is, they depend on the sequence of changes in the input signals. Unlike sequential circuits, combinational circuits do not have memory. 

#### Rules of combinational circuits
1. Every element of the ciruit is combinational
2. Every circuit connection is either an input or connected to a single output of another circuit element.
3. The circuit does not contain cyclic connections: every way in the circuit goes through any element no more than once.
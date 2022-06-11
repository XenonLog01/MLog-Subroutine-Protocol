# MLog Subroutine Protocol

The MLog Subroutine Protocol (MLSP) is a de-facto protocol for logical processors in
Mindustry. It describes a universal way of implementing subroutines in logic processors,
in such a way that any two processors could be linked.

NOTE: All code is written in .mlog files(or occasionally just in markdown), and ran through the 
[minblur](https://github.com/Bindernews/minblur) transpiler. It is reccomended to become familiar
with the text format for Mindustry logic(mlog), as all logic is in text format.

## Overview

Materials:

 * 'Master' logic processor, from which the call to the routine is made.
 * A memory block to store input, output, and activation data.
 * A 'child' processor, which contains the desired subroutine.

Planned:
 * Async routines
 * A way to pass non-number inputs and outputs.
 * More complete documentation on a full website.

## Memory

The MLSP defines the layout for a given function call.

### Mapping

 1. {0}     Subroutine run - Setting this to 1 in the master process begins the subroutine. NOTE: this will halt the main process.
 2. {1}     Unused         - Available for future expansion of the protocol.
 3. {2-30}  Inputs         - All of the provided inputs. NOTE: Non-number inputs cannot be passed currently.
 4. {31-\*}  Output(s)     - The return value(s), if there are any.

## Processors.

### Master Process

 * should know cell names as variables.
 * should fall into a loop until the child returns.
 * Setting the subr start should only occur AFTER all inputs are set.

### Child Process

 * Should sit in loop until activated.
 * Once it's task is finished, it should clear the cell (except for outputs), and resume looping.
 * On finish, it should also reset the cell at address {0}, causing it to loop again.


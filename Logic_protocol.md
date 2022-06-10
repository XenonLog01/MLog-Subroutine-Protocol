# MLog Subroutine Protocol

The MLog Subroutine Protocol (MLSP) is a de-facto protocol for logical processors in
Mindustry. It describes a universal way of implementing subroutines in logic processors,
in such a way that any two processors could be linked.

## Overview

Materials:

 * 'Master' logic processor, from which the call to the routine is made.
 * A memory block to store input, output, and activation data.
 * A 'child' processor, which contains the desired subroutine.

Planned:
 * Async routines
 * A way to pass non-number inputs and outputs.
 * More complete documentation on a full website.
 * Kill process should act as a _kill now, very important_

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
 * On finish, it should also reset the cell at adress {0}, causing it to loop again.

## Code:

The master process:

```
set SUBR cell1

set in 5

write in SUBR 2

write 1 SUBR 0

read continue SUBR 0

jump 4 equal continue 1

read result SUBR 31

print result

printflush message1

jump 8 always x false

```

The subroutine:

```
set SUBR_CALL cell1

read start SUBR_CALL 0

jump 1 notEqual start 1

read in SUBR_CALL 2

op add result in 3

write result SUBR_CALL 31

write 0 SUBR_CALL 0

write 0 SUBR_CALL 2

jump 1 always start 1

```


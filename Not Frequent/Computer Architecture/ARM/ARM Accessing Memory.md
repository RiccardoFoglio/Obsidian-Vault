![[Pasted image 20241103140513.png]]
![[Pasted image 20241103140530.png]]
![[Pasted image 20241103140547.png]]

### Exercise:
If r0 = 0x00008004, what are the values of the registers r1-r5 after the following instructions?
```
LDR r1, [r0] 
LDRB r2, [r0] 
LDRH r3, [r0] 
LDRSB r4, [r0] 
LDRSH r5, [r0]
```

This is the memory:

| 0x41 | 0x00008000 |
| ---- | ---------- |
| 0x73 | 0x00008001 |
| 0x73 | 0x00008002 |
| 0x65 | 0x00008003 |
| 0x8D | 0x00008004 |
| 0x62 | 0x00008005 |
| 0x6C | 0x00008006 |
| 0x79 | 0x00008007 |

- LDR r1, r\[0] --> load word (32 bits) LSB to MSB so r1 = 0x**796C628D**
- LDRB r2, \[r0] -->  load byte (8bits) so r2 = 0x000000**8D**
- LDRH r3, \[r0] -->  load half word (16 bits) LSB to MSB so r3 = 0x0000**628D**
- LDRSB r4, \[r0] -->  signed byte, convert all 0s to 1s before the number if negative, so r4 = 0xFFFFFF8D
- LDRSH r5, \[r0] --> signed half word, since 628D is positive keep 0s, so r5 = 0x0000628D


- STR copies the content of a register into 4 consecutive memory locations
- STRB copies the LSB of a register into memory location
- STRH copies lower 16-bit content of a register into two consecutive memory locations
- STRD copies content of two registers into 8 consecutive memory locations

## Addressing Mode
- Pre-indexed : immediate with or without writeback, with left-shiftable registers
- Post-indexed : immediate but no register

### Pre-Indexed Addressing
address is computed by summing the offset to the value in the base register `Rn`
`load/store <Rd>, [<Rn>, <offset>]{!}`
if offset is a register, it can be shifted left up to 3 positions (with `LSL #number`)
if offset is an immediate, the range is:
- \[-255, +4095] without writeback
- \[-255, +255] with writeback : ! is added at the end (`Rn` is updated after the instruction)

### Post-Indexed Addressing
Address is given by the base register `Rn`
`load/store <Rd>, [<Rn>], <offset>`
Then `Rn` is updated by adding the offset
The offset is an 8-bit immediate value
! is missing because `Rn` is always updated

### Look-up Table
a look-up table is an array of pre-calculated constants
Pro: frequently used values are not computed at run-time or are computed only the first time
Con: additional memory space is required
The look-up table can be easily accessed with indexed addressing
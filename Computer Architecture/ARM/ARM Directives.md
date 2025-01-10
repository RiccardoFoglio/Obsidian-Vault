
Instruction format: `{label} {operation} {; comment}`
where *operation* may be an instruction, a pseudo-instruction or a directive

line: up to 4095 chars, and can be split in several lines using backslash at the end

Common Directives: 
	``AREA, RN, EQU, DCB, DCW, DCWU, DCD, DCDU, ALIGN, SPACE, FILL, LTORG, PROC/ENDP or FUNCTION/ENDFUNC, END``

``AREA sectionName {,attr} {,attr}...``
- if `sectionName` starts with a number, it must be enclosed in bars `|1_DataArea|`
- `|.text|` is used by the C compiler
- at least one `AREA` directive is mandatory

Section Attributes:
- `CODE` : the section contains machine code
- `DATA` : the section contains data
- `READONLY` : the section can be placed in read-only memory (default for `CODE`)
- `READWRITE` : the section can be placed in read-write memory (default for `DATA`)
- `NOINIT` : all bytes in the section must be 0 (only for `DATA` section)
- `ALIGN = expr` : the section is aligned on a $2^{expr}$ byte boundary

RN defines the alias of a register: `name RN registerIndex` , like `coeff1 RN 8`

EQU gives a symbolic name to a numeric constant : `name EQU expression`, like `COLUMNS EQU 8`

Numbers can be expressed in any base, characters in quotes

Memory allocation: `{label} DCxx expr{,expr}...`
- `DCB` : constant byte
- `DCW` : constant half-word
- `DCWU` : constant half-word unaligned
- `DCD` : constant word
- `DCDU` : constant word unaligned

ALIGN directive aligns the current location to a specified boundary by padding with zeros:
`ALIGN {expr{, offset}}`
The current location is aligned to the next address of the form: 
$$n * expr + offset$$
If *expr* is not specified, ALIGN sets the current location to the next word boundary

The SPACE directive reserves a zeroed block of memory: `{label} SPACE expr`
where expr is the number of bytes to reserve.

The FILL directive reserves a block of memory to fill with a given value:
`{label} FILL expr {,value {,size}}`
- expr = number of bytes
- value = fill block of expr bytes, if omitted, value = 0
- size = size of value in bytes, 1/2/4. if omitted size = 1

Literal pools are constants that can not be assigned to a register with a MOV instruction. 
By default literal pools are put at the end of code sections.
The LTORG directive forces the assembler to put a literal pool within the code section

Only a label is required to call a function:
````
my_func ADDS r0,r0,r1   ;add 2 params 
        BX LR           ;return
````
To improve clarity, directives can be used to indicated start and end of function
````
my_func PROC            ;or FUNCTION
		ADDS r0,r0,r1   ;add 2 params 
        BX LR           ;return
		ENDP            ;or ENDFUNC
````

The END directive tells the assembler that the current location is the end of the source file.
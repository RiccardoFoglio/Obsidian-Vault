C and assembly can be mixed:
- calling a C function from assembly file
- calling an assembly subroutine from C file
- inline assembly: assembly code in a C file

Visibility of the C function:
- two directives notify the assembler of the name of a symbol defined in another file:
```
IMPORT symbolName {[options]}
EXTERN symbolName {[options]}
```
IMPORT always imports the symbol : The linker generates an error if the symbol is not defined elsewhere

EXTERN imports the symbol only if it is used : The linker generates an error if the symbol is used in the assembly file but not defined elsewhere

Options:
- WEAK : prevents the linker generating an error if symbol is not defined, searching libraries that are not included already
- DATA | CODE : treats the symbol either as data or code when source is assembled and linked
- SIZE = value : specifies the size. 

```
Reset_Handler PROC 
	EXPORT Reset_Handler [WEAK] 
	IMPORT SystemInit 
	IMPORT __main 
	LDR R0, =SystemInit 
	BLX R0 
	LDR R0, =__main 
	BX R0 
	ENDP
``` 


- `main()` is the user-defined function: embedded applications need an initialization sequence before `main()` function starts. it's called the startup code or boot code.
  The ARM C library contained pre-compiled an pre-assembled code sections for startup.
  The linker includes the necessary code from the C library to create a custom startup code

- `__main` is the entry point to the startup code in the ARM C library

Data exchange is regulated by ARM Architecture Procedure Call Standard (AAPCS)
First 4 parameters are passed in `r0-r3`
Further parameters are passed in the stack. After returning, they must be popped from the stack.
C function returns the value in `r0-r3`


In C File:
- prototype of the subroutine
- the prototype begins with `EXTERN`

in Assembly file:
- subroutine is implemented
- the symbol is exported with one of the equivalent directives:
```
IMPORT symbolName {[options]}
EXTERN symbolName {[options]}
```
## C

### Main components

* Heap - Manually memory allocation, `malloc`, `calloc`, `global` and `static` variables
* Registers - Small storage areas within the processor, contains addresses, values etc. of 8 bytes or less
	* General registers: `eax`, `ecx`, `esi`, `ebx`, `edx`, `edi` 
	* Reserved registers: `ebp`, `esp`, `eip`
* Stack - Top of the stack has a lower address (`pop()`) than the base of the stack
	* Stack frame: A frame on the stack containing all local variables of a function (from `ebp` to `esp`)vvvvvvvvvvvvvvvvvvv
	* `ebp`: this is pushed onto the stack (inside a stack frame)
	* `esp`: is given the value of `ebp` and decremented (now above the top of the stack?) to give room for local variables
	* If any function's return condition gets satisfied, no matter if it has loaded the local variables or not, it will immediately return from stack with it's stack frame (base condition in recursion).


## Assenbly

All assembly code is just numbers, even the `mov` instructions (`mov` just points to a fixed unique number.) We read `mov eax 5` but what's actually there is `B8 05000000` which is what the computer sees (1s and 0s), but it means the same. CPU registers are global variables (`rax`, `rsi`, `eax`) and there are 8-32 of them. On a 64-bit machine the registers can store up to 64-bit numbers. If you need more, you need to use more than one variable.
The program counter (`pc`, `eip`, etc) is a special registers which tells the CPU which instruction to execute next. 

Assembly is like a sheet of intructions from top to bottom where each has an address (`0x4005db`) and the program counter will point to the next address in the program. Like a programming language, assembly fetches values from memory and puts them into registers (like variables), does something with those variables, and returns the result to memory. Registers are much faster than memory, but space is limited so we need to utilize memory. If some memory addresses keep on being reused the CPU can store them in CPU cache which is slower than registers, but faster than memory.

```
0x4005db mov rax, rsi
0x4045db add rbx, 0x1
0x4085db test rbx, rbx
0x4105db mov rax, rsi
...
```
Assign 5 to register, add 3.
Assign 8 to another register.
Subtract the first register from the second register

```
mov eax, 0x5
add eax, 0x3
mov ebx, 0x8
sub eax, ebx
```
`eax` now holds 0.

What doesn't fit in registers lives in memory (using an address to store a value instead of an instruction?)

Accessing memory with loads and stores at addresses (like an array of memory)
```
0x00000000 0x1234
0x00000005 mov eax, [0x00000000]
```
Accessing memory through `PUSH` and `POP` operations on a stack. `sp`, `esp`, or `rsp` point to the top of the stack. If `esp = 20` then the top of the stack is at address 20. The bottom of the stack can be thought of as the last memory address in RAM. Pushing to the stack therefore decrements the address each time. If the top of the stack is address 30, then `push 0x3` puts `0x3` at the top of the stack at address **29** (30-1).
```
0x00000005 push 0x5
0x00000010 pop eax 
```
`eax` now holds 5.

**Control flow** is done via `GOTO`s (`jump`, `branch`, `call`)
```
0x0000ac mov rax, rsi
...
0x4005db jmp 0x0000ac	; mov eip, 0x0000ac 
```
**Status flags** says something about the resulting effects of previous instructions (e.g. last value computed to zero -> zeroflag = 1) 
**Branches** are `GOTO`s that are predicated on status flags. Here it jumps to address 0x000 because the flags set following the subtraction instruction tells `jl` that "Yes, eax < ebx"
```
mov eax, 0x5
mov ebx, 0x8
sub eax, ebx
jl 0x000
```
`call` pushes the next address on the stack, and `ret` pops it off and keeps going where the `call` instruction was executed. This allows you to jump to a section, execute a bunch of instruction sequentially, and when the program reaches `ret` it will return back (now you have a regular function).

### .bss vs .data
`.bss` refers to uninitialized global and static objects
```
unsigned char var; 		// 8 bit
static int svar;		// 16 bit
```

`.data` refers to initialized global and static objects
```
unsigned char var = 25;		// 8 bit
static unsigned int svar = 2;	// 16 bit
```

## NASM

The instruction field may contain any machine instruction: Pentium and P6 instructions, FPU instructions, MMX instructions and even undocumented instructions are all supported. 

### Instruction operands
* registers (`ax`, `bp`, `ebx`, `cr0`, etc)
* constants
* expressions
* effective addresses (addresses enclosed with `[ ]`), any operand to an instruction which references memory, e.g.
```
wordvar dw	123
	mov	ax, [wordvar]
```
Involving more than one register
```
mov	eax,[ebx*2+ecx+offset]
```

### Passing arguments

* RAX	System call number
* RDI	1st argument
* RSI 	2nd argument
* RDX 	3rd argument
* RCX 	4th argument
* R8 	5th argument
* R9 	6th argument

### Declaring uninitialized data

```
buffer: 	resb	64	; 64 bytes
wordvar:	resw	1	; one word
realarray: 	resq	10	; array of ten reals
```

### Defining constants
```
message		db	'hello world'
msglen		equ	$-message	; (this address) - (message address) ?
```


### Instruction suffix
Show how large the operands are going to be 
```
movq	rax, rbx	; moving quadword (8-byte value)
```

### Pseudo-Instructions 
Used in the instruction field, but are not real machine instructions.
Pseudo-instructions `DB`, `DW`, `DD`, `DQ`, `DT`, `DO`, `DY` and `DZ`  
Uninitialized pseudo-instructions `RESB`, `RESW`, `RESD`, `RESQ`, `REST`, `RESO`, `RESY` and `RESZ`  
`DT`, `DO`, `DY` and `DZ` do not accept integer numeric constants as operands.

## Stuff

`strcmp` and `cmp` is different. `cmp` (at the processor level) compares to registers `A` and `B` and store the result in some flag. `strcmp` (a "high-level" function) compares two C-style strings. Results/instructions will vary from different platforms and compilers.
```
call 0x400480b0 <strcmp@plt>
```



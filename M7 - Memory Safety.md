reprogramming the system
**memory corruption attacks**
- corrupt some values and gain some advantage
- hijack the control flow
what can be corrupted:
- data
	- read wrong values
	- privilege escalation (CSR)
	- fault injection
- code
	- force the system to run malware
		- hijack the control-flow
		- inject the payload
###### control flow
mechanism - CPU logic to update PC (program counter) - update what mem
adversary want to take control of PC, jump around to otherwhere
###### basic block
consecutive statements with a single entry and a single exit (unique), may coincide
think of the graph
##### how to hijack the control flow?
*buffer overflow:*
heap grows upward, stack grows downward
###### calling convention
- PC needs to be updated
- remember the return addr
**stack layout:**
- params
- return addr - PC
- saved frame pointer (ebp, base pointer)
- buffer(20bits, local variable)
can we manipulate the stack using inputs?
- stack overflow, buffer overflow (the buffer gets 30 bits injected) - control-flow hijack, jumpt to what we want
- $gets$ dont check the input size
why we still have this problem (buffer overflow)?
- legacy code: linux and windows are written in C!
	- when we design, no knowledge of security problem
invalid dereferencing:
- dangling pointer: temporal (free())
- out-of-bounds pointer: spacial
**Jump to where?**
- code injection attacks: jump to the code you want to execute
	- eg. ret addr point to the beginning of the buffer (some program i wanna run)
		- we need to find the exact beginning: "landing area", "NOP" sled, CPU will jump these directly which adds to the success rate![[Pasted image 20250303131715.png]]
		- in the payload: "Shellcode", open the shell (most of the times); def is the short code used by the attackers
- code reuse attacks: using existing code for some malicious goals
##### how to defend against code injection?
**non-executable memory**: each memory unit (page) shld be either writable or executable (read-only) - Write XOR Execute
so the stack is not executable, only writable
##### code reusable attack
search through the executable code to reuse them to get to the same end
find a sequence of code can do something malicious (gadget) - create a shellcode - how to run the gadgets BACK TO BACK?
- inject overloaded data
- stack overflow to rewrite the return address
- execute the gadgets one by one, jump to malicious locations
- can even create ur own stacks!
dispatcher gadgets - dispatch table (search through the code) - binary instructions
diff types of chaining gadgets:
- jump oriented programming (jop)
- return orienred programming (rop)
##### memory safe languages
safe practices and safe functions
a completely different, memory safe language (government is thinking about this): RUST, WedAssembly
##### stack canary
check the canary word (between local variables which is buffer and return addr) before the return: error printed if canary is overwritten (alive or not)
by the compiler
adversary can read and write; brute-forcing and leak the value
##### data execution prevention (DEP)
make parts of the memory non-executable - but will be inconvenient sometimes -不可运行
##### address space layout randomization (ASLR)
move blocks/sections around (the addr every time is diff)
adversary can leave some mark in that
##### control-flow integrity
find all legal transfers
#### how about finding bugs?
##### static tools: 
- disassemble code (even lift it)
- draw control-flow and data-flow graphs to find some patterns
problem: scalability, **coverage issue**
##### dynamic tools
run the app, extract important info as the program runs
eg, intel pintools: dynamic binary instrumentation, for debugging and testing, security reasons
performance counters: eg, perf
![[Pasted image 20250303163018.png]]
analysis could be on real hardware or another hardware(rehosting): angr, qemu
run on the real system and faster than static tools, but also have coverage issue: shld cover all the possible paths
###### fuzzing
eg, AFL, coverage-guided fuzzing
![[Pasted image 20250303175735.png]]
generate a sequence of inputs to test the program following a specific algorithm for better coverage
###### symbolic execution
assign symbolic values to the input - create symbolic graph - give path condition, find the particular variables fit that path
SAT problem / solver (NP)
challenges:
- path explosion and comlexity - loops
###### concolic execution
concrete + symbolic, much simpler, reduce the search space significantly
##### recap
interact with memory and cause corruption
data and code
control-flow hijacking
memory safety is a critical challenge
- ranging from simple buffer overflows to sophisticated code-reuse attacks
- static and dynamic analysis tools
- runtime techniques: canary, ASLR
still a challenge!
- open issue, unknown problems
- runtime and storage overhead tradeoffs
#### runtime monitoring and prevention
- monitoring for prevention
- for detecting an attack has happened
##### runtime monitoring for prevention
###### control-flow integrity (CFI)
how to prevent control-flow hijacking: is it a *legitimate jump/transfer*?
- forward edge (call, jump)
- backward edge (return)
**basic idea:**
- check all the indirect transitions in the program
- statically list all the possibilities
- check if the current transition is part of the list, and **block** it if not
how to **find** all the possible transitions: fuzzing, etc
how to **enforce**: implement checking before assigning the PC
**overhead**: check and store all the possible transitions
**forward edge:**
fptr: virtual calls
two phases:
- analysis, compute CFG, record all possible transitions
- enforcement: check the observed transition intervene if needed
where: hw, sw, os
**backward edges:**
add to every single place calling - target sets are too latge to provide meaningful
##### shadow stacks
a copy of the regular stack (not writable), just store the return addr, shadow RA: if the two values dont match, will call an exception
The two methods to implement a shadow stack are direct mapping and indirect mapping. In **direct mapping**, there is a constant offset between the stack and the shadown stack in memory, whereas in **indirect mapping**, the shadow stack and its location can grow on demand
**advantages**:
- know at runtime what functions u call from
- dynamic defense: NOT rely on static analysis (dont care CFG, 100% cover for every time calling functions)
indirect mapping: allocate space for actual use
##### tag architecture
tag every pointer (actually everything including code and data can be tagged) check the tag before processing
with overheads, remove the issue of overwriting, strong safety but more mem, performance and time
#### remote attestation
check the status of a remote device (occasionally)
eg, firmware / mem - Hash -> not match -> sth has been changed
a two-player game:
- prover (remote device)
- verifier (ground truth, trusted)
**goal**: prover needs to show the mem isnt modified (must have guarantee)
**attack scenarios:**
- **replay attack**, save the HASH and send back if need - **fix**: **nonce** from the verifier can help, use a random seed
- **forging attack**: copy the memory - **fix**: reduce the memory (no spare mem) - adversary: compress the mem - **fix**: RoT hardware, compute other features like time (processing needs time? but can send to a third-party with faster processor)
	- response
	- some evidence proof of integrity
##### secure remote attestation:
- use the hardware (eg TPM trust platform modules)
	- challenge: internal but we need to make it available to a remote user (trusted execution environment (TEE), extending TPMs to run dynamic code, needs a lot of overhead)
- **pure software methods** (time, can be cheated)
	- verification code+application code
	- verification code will take more **time** than the application code! they must *using additional changes or jumps* (the diff must be apparent, so using some loop!) computing time + communication time (assuming to be same)
	  attackers may modify the DP or PC -> so HASH BOTH (DP, PC, Mem(DP)); so the hash and time cant be correct at the same time if attacked
		- H()
		- initialization
		- communication
- hybrid
###### check everything
large overhead
###### considerations
- what hash functions to use?
	- MAC vs secure hash vs ad-hoc checksum
- time optimality of the code (or the time can not be trusted, must be **determinist** or adversary can take advantage of the optimal space: stable latency)
	- challenge $t_sent>>t_attack$
	- software is not a good solution for very far-away attacks
	- other challenge: 
		- every time the attestation calls, the malicious code hides into the **data mem**, so the program memory remains unchanged
		- **proxy** that has much faster **clock**
	- solution: continuous attestation, no time to send to other proxy (time of check (toc); time of use (tou), vulnerable to attacks! this is runtime, we cant check anytime)
- how to know the exact timing
###### summary for software attestation
- very versitile and low-overhead
- need hardware support
###### hybrid methods attestation
hardware is expensive, can we use a lil for low-overhead?
- **read-only memory**: verification code. attestation code is moved to a ROM
- **keyed hash function** (only the attestation code can read the secure key, the bus shld be specially designed (the hardware part))
**concerns**:
**the key needs to be protected** when
- attestation ends
- interrupts (it shldnt be interrupted! good to restart)
- DMA direct mem access (an agent configued to enable i/o to access the memory), must handle or data could be lost
**fix by**: secure reset and monitoring
TOC TOU (how to know it is still secure?)
- multiple checks with specific timeline, etc
- monitor the previous attested memory, if its modified, run the attestation again (just monitor when not running the attestation)
**relaxed properties:**
- atomic execution
- invocation flexibility
- flexible reset
- DMA monitoring
![[Pasted image 20250304160233.png]]
HMAC is 2 times of HASH

##### attestation is not for code-reuse attacks!
-> **control flow attestation (CFA)**:
hash the CFG path instead of the memory! just in background, monitor but not prevent!
check which basic blocks do u run (an after)
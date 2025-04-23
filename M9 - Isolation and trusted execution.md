system boots securely (TPM and hardware)
but there re many bugs that can curruot the system like system software
OS is compromised, now what?

#### Trusted execution environment 
bigger OS is not trusted, but we do trust hardware
##### software changes: 
a paradigm to run arbitrary software (enclave) it has to be small
- TCB (trusting computing base) very small, minimal capability, strip down and hope no bug in it
- secure monitor (SM)
##### hardware changes:
- capability to achive hardware isolation
##### eg ARM TrustZone: 
2 worlds normal REE and trusted TEE
- TEE can access resources in REE but not vice versa
- apps in TEE have access to each others resources (no way to isolation)
![[Pasted image 20250304165229.png]]
secure monitor has highest level trust
SMC (branch) -> secure API
add 1 bit to every memory
implementation:
- bus TZC controller: check the flag bit for transaction
- time slicing to execute either secure or non-secure (switch back to non-secure takes overhead : context switch has to pass a monitor mode)
![[Pasted image 20250304170120.png]]
limitations:
- only too worlds
- exclusive-only access to peripherals
- principle of least privilege
can have more layers for diff trust levels

how a new program is loaded in TEE?
- first a hardware attestation with key
- then a real computation
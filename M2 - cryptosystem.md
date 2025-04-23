we are not aware its importance
**loop for iot:**
sensing- local processing and analysis - network - edge/cloud - actuation
(not necessarily them all)
**so we just concerned about:** 
computer itself; interactions with physical and digital world

#### how to build a secure system
shld be based on smth we trust!
correct, hard to modify for adversaries
- **hardware**: logic and memory
- **software**: hardware is better than software, not so good for now but generally still holds **cons** for software: bugs; interactions with the out world
##### root of trust (ROT)
smallest thing that we can trust
allow us to achieve a good known state
Something that cannot be modified, changed, easily read, etc.
###### TPM
secure enclave, as small and simple as possible
a *common way of ROT* is **Trusted Platform Module** -- on chip, a module, a piece of *hardware* this is carefully designed to be secure
**functions needed for a TPM:**
- memories to hold data and code
- some functions that securely verify next level of system
- ability to run some basic applications for verification
###### secure boot:
**procedures:** from the initial code in TPM to check the untrusted ones, after verification the trusted parts are added into RoT, thats a bigger concept TCB (Trusted Computing Base) only checked the basic kernel, and then its the process of cryptography to extend it to other components until the whole OS is trusted, then the control is passed to the OS
rot is an important core of TCB
TCB includes all!
kernel by kernel
##### the whole process
we start from a small piece of hardware that we can trust, we then **gradually add software to this trusted base** until we pass the control to a trusted piece of software.
Afterward, we just **do our best to prevent things from happening, and detect them, and mitigate them**
#### cryptography
**Kerckhoffs's principle / Shannon:** A cryptosystem should be secure even if everything about the system, except the key, is public knowledge.
- key
- algorithms
- I/O
###### cryptographic primitives:
○ Encryption/decryption (symmetric and asymmetric).
○ Secure hash functions
○ Message authentication codes
○ Digital signatures

**symmetric**: the key is the same for E and D 
**asymmetric**: diff keys (sk and pk)
homomorphism (optional)

**attacks:**
- freq analysis
- brute-force
**attackers knowledge:**
partial / full: chosen plaintext is the strongest, they can choose m& c
##### perfect secrecy
for a random variable k, m0 and m1 has same probability to have a same c
- **one-time pad (OTP)**
  XOR: m and k is same length (also, its reversable) -> storage and computation overheads!
  ##### **issues**
- (1) k must have **same size as data**: key storage and exchange
- (2) **many-time pad**
  adversaries could get the pattern; so we dont want a deterministic model but *a probablistic one* -> so we want to generate diff keys/ciphers for diff times
##### semantic security
the chance the attacker finds out which encryption belongs to which message should be **negligible (not zero)**
it can achieve OPT with k<<m! by using a secure mechanism to **strech** the key
**examples**:
- for massage recovery attack: cant figure which msg it is, semantic security guarantees that
-attackers even cant tell any individual bit

**so how to build semantically-secure OPT?**
using a seed and a special algorithm G to strech the seed. and later would be same as perfect secrecy
the attacker shld not tell the diff of G(s) - PRG and a random string (CTR, LFSR)
**a stream cipher**: shld pass test to prove its security, rigorous process
- SEAL, chacha
- salsa
**HOW it tackle the 2 issues**: 
(1) length smaller
(2) many-time encrption: probabilistic cipher -> **nonce**: number used only once, never repeats! thus adding randomness
--> G(s, n) , n could be **shared publically**
##### Block cipher: Pseudo-Random Permutations (PRPs)
E(k,x)=E_k(x) 根据k生成E来encode plaintext, based on key generate diff permutation
a proper randomization and chaining are used -> semantically secure
encryption block by block
should be rlly random, cant distinguish from random permutation
shld be very easy to compute F but very hard to F-1 without the key
3DES, AES (no known attacks)
##### AES
- key expansion
- add round key
- shffle (final round dont mix columns)
- one-way function, non-linear
##### PRF(function) - 输出伪随机值 (HMAC)
map X -> Y instead of X - X (PRP) PRG has same length
stream, higher-efficient, more efficient than PRG
###### AES+counter: counter-mode block cipher: 
counter+nonce, and generate AES with key.
IV has 2 parts: counter and nonce which is random (nonce never repeats!!!)
#### Asymmetric (public key)
the keys for encrytion and decryption are diff
public key and private key (only verifier can decrypt)
in this kind of asymmetric attacks, adversaries can choose an encrypted text
-- trapdoor -- easy to compute but cant get back. ONE-WAY
- G is **probablistic** to generate sk and pk
- F is encryption with pk and X to Y (shld be one-way, cant be invertible)
- I is with sk Y to X (based on a hard known problem)
the inverse function typically reduces to a known hard problem
**e.g. RSA**
using asymmetric to encrypt the key and only someone has the sk knows the key.
factor numbers: so quantum computing can break it
drawback: computation overhead
**e.g. hybrid encryption: KEM**
key encapsulation mechanism
- G() -> pk, sk
- x is a nonce
- only person has sk can get x -> Hash -> k
- only do RSA once for the key
and later using this key for future symmetric (**speed** -uppppp)

symmetric faster
asymmetric needs wider bandwidth, not for lot of data
similiar security. asymmetric is sometimes stronger
##### message integrity: MAC
check the tag
forgery attack
- cant provide valid (m,t) pairs
- secure MAC: PRF and/or a block cipher
replay attack
- add session key or nonce!
nested MAC: cascaded
tag shld be big enough to avoid brute force
avoid extend: last function uses other key
##### HASH
input domain *is much bigger than* the output domain: cause collision
so **secure hash (SHA)**: collision resistence
- very difficult for any 2 msgs to produce the same hash: negligiable
properties of secure hash:
- cant return the m from the hash
- cant find another m_1
Hash is not a key, anyone can compute it
For MAC, key is required
Hash+MAC=**HAMC**, speed up the encryption process
- replace the AES in MAC with HASH
##### Authenticated Encryption - AE
make sure to encrypt first and tag later (encrypt, MAC)
##### Digital Signature
sk to sign; pk to get the msg


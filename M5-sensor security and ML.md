bc of interactions with physical world, there re fault/transduction/sensor attacks, physical limitations
### authentication
**distance** authentication: contactless payment
**key idea**: using communication as a means for measuring distance and finding the location
#### relay attack: 
unlock the car / contactless payment, just need a strong enough transmitter. just lie about where is (dist)
main way to prevent it is using nonce (never encrypt the same)
byzantine paradox? in network
**HOW TO prevent**: time flag; observe the signals physical properties
signal strength-based dist estimation (can be not ideal); phase-based; **time of flight(TOF)**, tp is the processing delay
can an adversary attack the TOF system?
lie about the distance by:
1. amplify and relay
2. delay and relay (modify the phase)
**HOW TO PROTECT:** the return signal can only generate by the true owner. he must have the secret key to generate. so the signal from the attacker must take longer than the true receiver (challenge-response paradigm)
**distance bounding:** a distance threshold
**problem:** delay issue (processing and network)
#### The Brands-Chaum Protocol
cuz its delay-sensitive, using simple operations like XOR (1 bit 1 time) and thousand times to get the average 
**BC protocol:**
-repeat a fast activity many times
- make the response simple yet dependent on the challenge
- make the challenge unpredictable
-sign the final response to make sure that its unforgeable (digital signature with a public key)

![[{105710E6-EFE2-4199-80A2-4B861950BC2C}.png]]
whats the real challenges>
for xor in digital domain must take a long time to demodulate, which introduces severe delay in **digital domain**
##### optimization: challenge reflection with channel selection (CRCS):
we dont have to enter digital domain, purely **analog**. select a diff channel to reflect, this will be faster to tackle the delay issue!
##### **summary:**
combination of avaraging, simple operation and basic cryptography can fix the DB frauds
##### other problems:
the adversary can predict the challenge and/or can precompute the challenge
### AR/VR
most VR headsets use pw typing as the sole method of identification. typing is generally slower than a traditional keyboard 
**motivation:** possible to detect what the user is typing in the VR domain (key logging)
Threat model:
- no access to the hw/sw
- observations are made externally
- occur publically
**method**
- click det
- feature ext
- keyboard location
detect when the clicks happen and locate the keyboard position
relevent positions of the shoulders
realistically u cannot defend it, move the keyboard a lil bit
also can use **acoustic** to detect the sound of the clicks (of controllers) non in-sight can also be eavesdropped
##### summary:
they re much closely integrated with physical world so suspectible for sensor and side-channel attacks
### sensor and ML
GAN: adversarial machine learining
modify inputs
ML is not rule-based, so they re intuitively not 100% accurate, kinda unexplainable
add pertubation (can find the minimum perdubation 干扰)
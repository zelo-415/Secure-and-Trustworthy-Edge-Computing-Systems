sensing and actuation(digital or physical, impact very large)
sensor parts: physical, analog, digital

**attack types:**
- transduction: change the env (drones)
- denial of service: prevent the sensor to read a correct value
- integrity/fault attack: analog domain
- digital

attack impact:

- transduction:
light, sound(vibration), tem, em/rf, magnetic, force, ...
<threat model>
most things determined by the signal strength of the attacker
equipment, range, time, access

defense:
- hardware
- software

defense mechanism
- detection: create a pattern and sense the pattern
- prevention: random, better design, sensor fusion

fault types:
- instruction
- circuit
- micro-arch
- env
fault injection
vs. faults: intension and intensity

DVFS:
maximum of freq and voltage pairs
如果f没有跟上V的下降速度，flip-flop就有可能output错误的结果，而且这是可控的
confidentiality attack(eavesdrop) / integrity attack (modify)

summary: transduction / faults 
hardware limitation, not even bugs!


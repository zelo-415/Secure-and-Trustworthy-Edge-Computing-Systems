#### security: CIA
confidentiality: is it known?
integrity: is it real?
availability: makes smth u need unavailable
- **机密性（Confidentiality）**：防止未授权访问数据。
- **完整性（Integrity）**：防止数据被篡改。
- **可用性（Availability）**：确保服务不中断。

need some policies: 2 aspects
**providing security**: implement and enforce these policies
**detection and reaction to attacks**

a cross-stack task in nature
#### security principles (some intuitions)
##### 1 threat model:
what we and the attackers can do?

Threat Model:
**attacker side**
What the attacker knows?
● Level of access?
Physical, temporal, privilege, etc.
● What the attacker wants to achieve?
● What are the attacker’s resources?
**User/Victim**
● What needs to be protected?
● Level of existing protection?
● What can be trusted?
○ Trusted Computing Base (TCB)
● Human factors
○ Mistakes in the program/design
○ Not following the right protocols (various reasons)

##### 2 Overheads/Tradeoffs
○ Achieving security almost always lead to some overheads (e.g., cost, performance, power, etc.). Thus, the solution should provide a right(?) tradeoff.
○ The solution has to be convenient/user-friendly.

##### 3 Multilayered defense

##### 4 Least privilege
##### 5 Distributed trust
##### 6 complete mediation

##### 7 Do not rely on obscurity

##### 8 fail-safe defaults

##### 9 isolation
do not share things as much as u can
##### 10 simlicity
check very fast
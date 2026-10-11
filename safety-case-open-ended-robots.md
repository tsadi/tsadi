# A Safety Case Framework for Behaviorally Open-Ended Robots

*Why ISO 10218 and standard HARA methodology break down once behavior is generated at runtime, not programmed at design time.*

Tsadi Shvo · [tsadishvo.com](https://tsadishvo.com) · [LinkedIn](https://www.linkedin.com/in/tsadishvo/)

## The problem

ISO 10218, ISO 12100 and ANSI/A3 R15.08 all assume a closed world. A robot's hazard analysis is built by enumerating a defined set of programmed tasks and motions at design time, then verifying the machine stays inside that envelope. That holds for an arm running a fixed pick-and-place routine. It stops holding the moment the robot's next action is generated live by a foundation model instead of a programmed sequence. The same hardware can produce motion combinations nobody designed, tested or anticipated during the original hazard analysis. HARA depends on being able to list what the machine will do. A general-purpose robot breaks that premise at the root.

## Why the usual fixes don't hold up

Most teams handle this one of two ways. Either they constrain the robot's capability until its behavior is small enough to enumerate, which quietly defeats the point of building a general-purpose system, or they lean on empirical testing to catch edge cases, which cannot scale against the combinatorics of open-ended behavior generation.

UL 4600 already solved a version of this for autonomous vehicles, replacing prescriptive checklists with a structured safety case built on claims, arguments and evidence. But it was built for navigation decisions on open roads. It hasn't been adapted for embodied manipulation, where the hazard is direct contact force against a person, and where ISO/TS 15066's force and pressure limits still assume the robot is executing a known program rather than one it just generated.

Even the newest work in progress, ISO/CD 25785-1 for dynamically stable industrial mobile robots (legged, wheeled or other), still at committee draft in 2026, targets stability and power-loss hazards in industrial settings. It doesn't touch a robot that is balanced, powered and simply choosing an action nobody wrote.

## A workable direction

The fix is architectural, not a smarter risk spreadsheet. Separate the layer that is certifiable from the layer that isn't, and never let the second one touch a person directly.

**Layer one** is hardware and firmware, enforcing hard limits on force, velocity and workspace boundaries independent of whatever the policy decides. This stays inside familiar territory, safety-rated interlocks and PLCs under ISO 13849, and gets certified the normal way regardless of what the model generates upstream.

**Layer two** is a runtime monitor that treats every proposed action as untrusted input. Before anything reaches actuation, it checks the request against physical plausibility and known safe envelopes, and clamps or rejects what falls outside them. This borrows from runtime assurance architectures used in aerospace and autonomous vehicles, where a simple, verifiable supervisor sits around a complex, unverifiable core.

**Layer three** is the safety case itself, built UL 4600 style as a living argument rather than a one-time certification. Every time the underlying model is retrained, the robot has effectively been reprogrammed, and the evidence behind the safety case needs to be revisited on that cadence, not once at launch.

## The open question

The architecture above isn't the hard part. Versions of it already exist across aerospace and AV. The unsolved piece is layer two's acceptance criteria. You cannot write a meaningful test for whether a behavior is out of distribution when you haven't defined what in-distribution means for a system whose value is generating behavior nobody explicitly programmed. That is the question to build the safety function around now, while a robotics team is still small enough to bake the answer into the architecture instead of retrofitting it after a field incident forces the issue.

---

Tsadi Shvo has spent his career on the harder version of this problem: building certification pathways for products that didn't fit any existing regulatory category, including the first UL 8139-certified vaporizer at PAX Labs and the machine safety case for Ample's robotic battery-swap system.

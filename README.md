Drone Project Plan
Team: Felipe (hardware), Sophie and Akim (software) Budget: €200 total · Frame: 3D printed (free) · Special feature: LoRa telemetry link

1. Goal
Build a small quadcopter from scratch that flies stably, and send live telemetry (GPS, battery, altitude, attitude) over a LoRa radio link to a ground station that displays it.

Done means:

The drone takes off, hovers and lands safely.
Telemetry reaches the ground station over LoRa and is displayed live.
We can explain and demo every part of the system we built.
2. System overview
Flight link (RC): a normal radio link (e.g. ExpressLRS) for stick control. LoRa is too slow for this.
Flight controller (FC): reads the IMU and runs the control loop (PID) to drive the motors through the ESC.
Telemetry link (LoRa): a LoRa module on the drone sends small binary packets. A second LoRa module on the ground receives them.
Ground station: a program (Python or web dashboard) that parses packets and shows live data and logs.
3. Roles
Person	Role	Main tasks
Felipe	Hardware lead	Print frame, solder, wire, mount parts, bench tests, safety, parts and budget
Sophie	Flight software (suggested)	FC firmware or configuration, sensors, control loop, tuning
Akim	Comms and ground station (suggested)	LoRa driver, packet format, duty-cycle limiter, dashboard
Sophie and Akim can swap roles. Everyone reviews each other's work so all three understand the whole drone.

4. Budget (approximate, check real prices before ordering)
Item	Est. cost
4 brushless motors	€28
4-in-1 ESC	€22
Flight controller (with IMU)	€22
GPS module	€10
2 LoRa boards (SX1262 / SX1276)	€22
RC receiver and transmitter (ask about a used or borrowed transmitter)	€30
2 LiPo batteries	€22
Balance charger	€20
Props (many spares)	€8
Wires, connectors, screws, standoffs	€8
Total	€192
Crash reserve	€8
Ways to save: borrow a transmitter or charger from the school or a club, 3D print everything structural, and buy props in bulk. Ask the teacher whether borrowed equipment counts against the budget.

5. Timeline
Adjust to the real deadline. Weeks are relative to the start.

Week	Hardware (Felipe)	Software (Sophie, Akim)
1	Finalise parts list, order, start printing frame	Choose firmware route, set up repo, write the packet spec
2	Print and assemble frame, solder motors and ESC	Read IMU, set up build tools, LoRa hello-world between two boards
3	Bench tests without props, then tethered hover	Control loop or FC config, first tuning
4	Mount GPS and LoRa on the drone, weight check	Send real telemetry packets, start dashboard
5	Outdoor test flights, repairs	Duty-cycle limiter, logging, fix packet loss
6	Final flight and demo prep	Polish dashboard, write documentation
6. LoRa design notes
LoRa gives about 0.3-50 kbps depending on settings: telemetry and commands only, no video.
The EU 868 MHz band has duty cycle limits (typically 1%, about 36 s of transmit time per hour). Send compact binary packets at a low rate, and implement a limiter in software.
Add a sequence number and CRC to every packet so the ground station can detect loss and corruption.
Never depend on LoRa for flight safety. The RC link and a failsafe must work on their own.
Draft packet (to be agreed by the team):

Field	Size
Sequence number	2 bytes
Battery voltage	2 bytes
Altitude	2 bytes
Latitude / longitude	8 bytes
Roll / pitch / yaw	6 bytes
Flags (armed, failsafe)	1 byte
CRC	2 bytes

Does borrowed equipment count toward the €200?
What is the deadline, and what is the final deliverable (demo, report, code, or all)?
Where are we allowed to test fly?

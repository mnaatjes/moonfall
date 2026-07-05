# Specifications: Spacecraft

This document tracks the distinct spacecraft blueprints (orbiters, landers, and rovers) that the player can construct, equip with science packages, and launch.

---

## Spacecraft Blueprint Database

| Spacecraft ID | Display Name | Base State | Base Dry Mass (kg) | Max Instruments | Power Output (W) | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `lro_orbiter` | Lunar Reconnaissance Orbiter | `Orbiting` | 1,000 | 6 | 800 | Standard high-altitude mapping satellite. |
| `luna_lander` | Luna Robotic Land Probe | `Landed` | 800 | 3 | 100 | Basic stationary lander for localized scientific scanning. |
| `pragyan_rover` | Pragyan Micro-Rover | `Roving` | 26 | 2 | 50 | Low-mass robotic explorer with micro-chemical sensors. |
| `yutu_rover` | Yutu Heavy Rover | `Roving` | 140 | 4 | 150 | Heavy explorer chassis capable of deep radar scanning. |
| `apollo_lm` | Apollo Lunar Module | `Landed` | 4,280 | 8 | 350 | Manned habitat lander with extensive science payload bays. |

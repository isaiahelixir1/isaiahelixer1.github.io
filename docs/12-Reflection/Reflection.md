---
title: Reflection
---

# Reflection

## Review of Module's Success

Overall, the Environmental Sensor Subsystem successfully met most of the core design requirements defined at the start of the project. The final system operates from a single 9V DC input and reliably regulates power down to 3.3V using the LM2575T-3.3G switching regulator. This meets the requirement for a stable and efficient power system capable of supporting both the microcontroller and sensor load.

The subsystem successfully implements temperature sensing using the TC74 digital I2C sensor, which meets the requirement for accurate and reliable environmental data collection while minimizing pin usage and simplifying firmware design. The PIC18F57Q43 microcontroller successfully handles sensor communication, system control, and inter-board communication through UART and GPIO expansion lines.

The system also successfully includes ICSP programming support and a debug interface, which allows for reliable firmware development and troubleshooting during testing.

The main requirements that were not fully realized include full system integration testing with all teammate boards under final load conditions. While interfaces are implemented and verified individually, complete end-to-end system validation across all subsystems was limited due to time and integration constraints. Additionally, some initial design explorations involving additional environmental sensors were reduced in scope to prioritize stability and simplicity.

---

## Microcontroller / Module Startup Tips

Several challenges were encountered during initial system bring-up and debugging. The following list summarizes key lessons and advice that would improve future startup efficiency:

1. Always verify power rails first before connecting any sensors or communication lines, as unstable voltage can lead to misleading debugging results.  
2. Ensure the ICSP programming connections are tested early using a known-good programmer setup before assembling the full circuit.  
3. Confirm I2C pull-up resistors are correctly sized and connected, as missing or incorrect pull-ups can make sensors appear non-functional.  
4. Validate UART communication independently using a loopback test before connecting to external subsystems.  
5. Bring up the microcontroller with a minimal firmware first (LED blink or simple GPIO toggle) before adding sensor or communication complexity.  
6. Double-check grounding between all subsystem boards, as communication issues were often traced back to incomplete ground references.  
7. Test the switching regulator output under load conditions early, not just at no-load voltage measurements.  
8. Use test points extensively during debugging to isolate power, communication, and signal issues.  
9. Label all connectors clearly in both schematics and physical hardware to avoid wiring mistakes during integration.  
10. Incrementally integrate subsystems rather than assembling the full system at once.

---

## Lessons Learned

1. Power system design is one of the most critical aspects of embedded hardware development, and small mistakes can cascade into system-wide failures.  
2. Switching regulators provide significantly better thermal performance than linear regulators when stepping down from higher voltages like 9V.  
3. Choosing simpler sensors (such as the TC74) can dramatically reduce firmware complexity and integration risk.  
4. Proper I2C design requires careful attention to pull-up resistors, addressing, and bus stability.  
5. PCB layout decisions directly affect debugging difficulty, especially for power and communication lines.  
6. Modular design greatly improves debugging and system integration compared to tightly coupled designs.  
7. Early testing of subsystems individually prevents compounding issues during final integration.  
8. Communication between boards requires strict agreement on voltage levels, timing, and protocol structure.  
9. Documentation during development is essential, as many design decisions are difficult to reconstruct later.  
10. Time management is as important as technical design, especially when multiple subsystems must integrate at the end of the project.

---

## Recommendations for Future Students

1. Begin by fully validating your power system before connecting any sensitive components, as this will prevent damage and reduce debugging time later.  
2. Design and test each subsystem independently before attempting full system integration to isolate issues more effectively.  
3. Use simple and well-documented sensors whenever possible, especially when learning embedded system integration for the first time.  
4. Maintain consistent labeling and documentation across schematics, PCBs, and wiring diagrams to avoid integration errors.  
5. Start firmware development early with minimal working examples so that hardware and software debugging can progress in parallel.

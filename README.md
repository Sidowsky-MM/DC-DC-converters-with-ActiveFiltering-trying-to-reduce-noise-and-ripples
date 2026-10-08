# DC-DC Converters with Active Filtering (Noise & Ripple Suppression)

<p align="center">
  <img width="49%" alt="Boost 3D Board" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/PCB_MAIN/BOOST/boost3D.png" />
  <img width="49%" alt="Buck 3D Board" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/PCB_MAIN/BUCK/buck3D.png" />
</p>
<p align="center">
  <img width="49%" alt="SEPIC 3D Board" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/PCB_MAIN/SEPIC/Sepic3dpers.png" />
  <img width="49%" alt="ZETA 3D Board" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/PCB_MAIN/ZETA/Zeta3dpersp.png" />
</p>

## Project Overview

Goal: Analyze, implement, and verify dedicated active and passive filtering topologies (ARF, AEF, Pi, CM/DM) across multiple DC-DC converter topologies (Synchronous SEPIC, Synchronous ZETA, Buck, and Asynchronous Boost) to minimize output voltage ripple (Vp-p) and switching noise while characterizing efficiency penalties and loop stability trade-offs.

Project Scope:
* Custom Multi-Topology PCB: Designed and routed in KiCad, integrating synchronous SEPIC, synchronous ZETA, Buck, and asynchronous Boost configurations alongside dedicated modular filters.
* Filter Implementations: Active Ripple Filters (ARF), Active EMI Filters (AEF), Common-Mode/Differential-Mode (CMDM) networks, and passive multi-stage Pi filters.
* Simulation vs. Hardware Discrepancies: Identification of ESL and ESR parasitic impacts not captured in standard ideal simulation models.
* Comprehensive Laboratory Verification: Evaluation of Vp-p attenuation (tested up to 3 A and 4 A load currents), relative voltage drop, THD metrics, and overall electrical conversion efficiency.

---

## System Architecture

<img width="75%" alt="Main Controller Schematic" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/LT8711_main.png" />

The experimental platform is modular, structured into four core functional stages:

1. Converter Core: Reconfigurable power conversion stage supporting Synchronous SEPIC, Synchronous ZETA, Buck, and Asynchronous Boost topologies driven by dedicated switching controllers (LT8711).
2. Filter Topology Matrix: Interchangeable filter boards tailored to targeted frequency bands and load ranges (ARF for low frequencies/high currents, AEF for high frequencies, passive Pi, and CMDM).
3. Active Cancellation Loop: Operational amplifier stages injecting anti-phase signals to cancel AC voltage ripples directly at the power node.
4. Output Distribution & Sensing: Dedicated Kelvin measurement points designed for low-inductance ground-spring probe acquisition under dynamic load.

---

## Hardware Details & Schematics

### Input Conditioning & Filtering
<img width="49%" alt="Input Common Mode / Differential Mode Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/IN_CM_DM.png" />
<img width="49%" alt="Input Active EMI Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/IN_AEF.png" />

* Input Protection & Interconnection: Input terminals are equipped with transient protection networks and structured input filter connection headers.
* Input CMDM: Combined Common-Mode and Differential-Mode passive filter attenuating high-frequency conducted noise back to the primary supply.
* Input AEF: High-speed active filter stage suppressing higher-frequency switching transients before propagation into power leads.

### Output Stage & Active Ripple Suppression
<img width="49%" alt="Output Active Ripple Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/OUT_ARF.png" />
<img width="49%" alt="Output Pi Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/OUT_PI.png" />

* Output ARF: Active Ripple Filter optimized for lower frequency ripple cancellation under higher current loads (3 A to 4 A).
* Output Passive Filtering: Multi-stage LC and Pi filter options used to evaluate parasitic resonances against active loop performance.
* Filter Interconnection: Dedicated routing matrices allow testing converters with completely bypassed, purely passive, or actively assisted output filtering.

### Layout Sensitivity & PCB Parasitics
<p align="center">
  <img width="49%" alt="Input Connection Architecture" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/Filtry_sposob_polaczenia_wej.png" />
  <img width="49%" alt="Output Connection Architecture" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/Filtry_sposob_polaczenia_wyjsciowe.png" />
</p>

* Critical Switching Loops: Minimal loop areas for high di/dt paths are strictly enforced. Laboratory testing confirmed that small deviations, such as an extended return path or placing the controller IC several millimeters away from power inductors, lead to complete converter instability and loss of nominal output regulation.
* Parasitic ESL and ESR: Discrete component parasitics strongly alter loop response compared to ideal models, mandating empirical verification for ripple and transient behavior.

---

## Engineering Analysis & Experimental Insights

Hardware measurements revealed distinct trade-offs between topology selection, noise reduction, and power efficiency:

* Topology Behavior: Synchronous SEPIC and ZETA topologies achieve lower current ripple above 1 A load compared to the asynchronous Boost. However, due to an input-to-output voltage differential nearly seven times larger than in Boost mode, SEPIC and ZETA operate at significantly narrower duty cycles, which depresses overall conversion efficiency.
* Asynchronous Boost: The single-switch Boost does not provide noticeable EMC improvements over synchronous counterparts despite having fewer active switching transitions.
* Ripple vs. EMI Discrepancy: Suppressing peak-to-peak voltage ripple (Vp-p) and lowering THD below 2% does not guarantee a proportional reduction in radiated or conducted EMI emissions.
* Stability & Transfer Function Impacts: Active filters introduce additional poles, zeros, and spectral spikes into the loop. Under light-load conditions triggering hiccup mode, these can destabilize the compensation network and excite parasitic resonances.
* Efficiency Overhead: Active filtering stages are inherently lossy; operational amplifiers consume up to 100 mA quiescent/driving current to maintain anti-phase cancellation, yielding overall converter efficiency drops of up to 12%.
* Measurement Metrics: Relative voltage drop under load acts as the primary figure of merit for cross-topology comparison, normalizing differences in output voltage levels, inductor selection, parasitic capacitances, and switch Rdson.

---

## Future Improvements

* Adaptive Digital Control: Design and verification of adaptive active filters governed by a real-time digital signal processor (DSP).
* Formal Compliance Testing: Quantitative evaluation of active filter attenuation using a calibrated EMI receiver in accordance with the CISPR 16 standard.
* Power Budget Optimization: In-depth energy budget analysis tailored for spaceborne and industrial applications with critical mass and volume constraints.

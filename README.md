# DC-DC Converters with Active Filtering (Noise & Ripple Suppression)

<img width="75%" alt="Main DC-DC 3D Render" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/3D_mainDCDC.JPG" />

## Project Overview

Goal: Analyze, implement, and verify dedicated active and passive filtering topologies (ARF, AEF, Pi, CM/DM) across multiple DC-DC converter topologies (Synchronous SEPIC, Synchronous ZETA, and Asynchronous Boost) to minimize output voltage ripple (Vp-p) and switching noise while characterizing efficiency penalties and loop stability trade-offs.

Project Scope:
* Custom Multi-Topology PCB: Designed and routed in KiCad, integrating synchronous SEPIC, synchronous ZETA, and asynchronous Boost configurations alongside dedicated modular filters.
* Filter Implementations: Active Ripple Filters (ARF), Active EMI Filters (AEF), Common-Mode/Differential-Mode (CMDM) networks, and passive multi-stage Pi filters.
* Simulation vs. Hardware Discrepancies: Identification of ESL and ESR parasitic impacts not captured in standard ideal simulation models.
* Comprehensive Laboratory Verification: Evaluation of Vp-p attenuation (tested up to 3 A and 4 A load currents), relative voltage drop, THD metrics, and overall electrical conversion efficiency.

---

## System Architecture

<img width="75%" alt="Hardware Mode Selection" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/Changemode.JPG" />

The experimental platform is modular, structured into four core functional stages:

1. Converter Core: Reconfigurable power conversion stage supporting Synchronous SEPIC, Synchronous ZETA, and Asynchronous Boost topologies driven by dedicated switching controllers.
2. Filter Topology Matrix: Interchangeable filter boards tailored to targeted frequency bands and load ranges (ARF for low frequencies/high currents, AEF for high frequencies, passive Pi and CMDM).
3. Active Cancellation Loop: Operational amplifier stages injecting anti-phase signals to cancel AC voltage ripples directly at the output node.
4. Output Distribution & Sensing: Dedicated Kelvin measurement points designed for low-inductance ground-spring probe acquisition under dynamic load.

---

## Hardware Implementations

### Power Conversion Stage
<img width="49%" alt="Main DC-DC Controller IC" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/MainIC_DC_DC.JPG" />
<img width="49%" alt="Power Switching MOSFETs" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/KEY_MOSFETS.JPG" />

Key hardware layout observations derived from experimental testing:
* PCB Routing Sensitivity: Strict enforcement of minimal current loops for switching nodes is mandatory. Experimental data confirmed that routing discrepancies (e.g. extending return paths or placing the controller IC several millimeters away from inductors) introduce severe instability and collapse output regulation.
* Switch Selection: Low Rdson power MOSFETs combined with high-current inductors were used to isolate semiconductor conduction losses from filtering overhead.

### Active & Passive Filter Modules
<p align="center">
  <img width="49%" alt="ARF 3D Render" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/3D_ARF.JPG" />
  <img width="49%" alt="AEF 3D Render" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/3D_AEF.JPG" />
</p>
<p align="center">
  <img width="49%" alt="Passive Pi Filter 3D" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/3D_PI.JPG" />
  <img width="49%" alt="CMDM Filter 3D" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/3D_CMDM.JPG" />
</p>

* Active Ripple Filters (ARF): Optimized for lower frequency ripple suppression under elevated load conditions (3 A to 4 A).
* Active EMI Filters (AEF): Configured with fast op-amps for high-frequency switching transient attenuation.
* CMDM and Pi Filters: Evaluated as passive baselines to assess high-frequency parasitic resonances versus active cancellation.

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

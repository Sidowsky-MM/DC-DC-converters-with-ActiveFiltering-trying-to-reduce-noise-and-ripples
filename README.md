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

Goal: Analyze, design, and empirically evaluate active and passive ripple suppression methodologies across multiple switched-mode power supply topologies (Synchronous SEPIC, Synchronous ZETA, Buck, and Asynchronous Boost) to minimize output voltage ripple (Vp-p) and switching noise while quantifying efficiency penalties, stability boundaries, and parasitic influences.

Project Scope:
* Multi-Topology Hardware Design: Routing and manufacturing modular test platforms integrating synchronous SEPIC, synchronous ZETA, Buck, and asynchronous Boost configurations alongside dedicated analog filter blocks.
* Comprehensive Filter Implementations: Implementation of Active Ripple Filters (ARF), Active EMI Filters (AEF), Common-Mode/Differential-Mode (CMDM) chokes, and multi-stage passive Pi networks.
* Simulation vs. Hardware Discrepancies: Identification of parasitic inductance (ESL) and resistance (ESR) deviations that invalidate idealized small-signal SPICE models.
* Parametric Laboratory Characterization: In-depth measurement of peak-to-peak ripple attenuation across dynamic loads (up to 4 A), harmonic distortion, power dissipation of active stages, and relative voltage drop under heavy loading conditions.

---

## System Architecture

<img width="75%" alt="Main Switching Controller Schematic" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/LT8711_main.png" />

The experimental platform is structured into four cascaded stages:

1. Switching Core: Flexible power stage centered on the LT8711 controller, configured to drive Synchronous SEPIC, Synchronous ZETA, Buck, or Asynchronous Boost power trains.
2. Filter Topology Matrix: Independent, swappable filtering stages tailored to targeted frequency bands (ARF for lower frequency high-current ripple; AEF for high-frequency switching transients; passive Pi and CMDM chokes).
3. Active Cancellation Loop: Wideband operational amplifiers and discrete pass elements actively injecting anti-phase ripple cancellation currents directly at the power bus.
4. Acquisition & Diagnostics: Dedicated Kelvin sensing points and low-inductance connection terminals facilitating wideband oscilloscope probing and spectrum analysis.

---

## Hardware Details & Schematics

### Input Conditioning & Filtering
<img width="49%" alt="Input Common Mode / Differential Mode Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/IN_CM_DM.png" />
<img width="49%" alt="Input Active EMI Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/IN_AEF.png" />

* Input Protection & Isolation: Integrated TVS diodes, bulk capacitance, and isolation jumpers for input bus conditioning.
* Common-Mode & Differential-Mode (CMDM): Specialized dual-choke network attenuating differential switching spikes and common-mode noise directed back into the DC source.
* Input Active EMI Filter (AEF): Active sensing network using fast linear amplifiers to mitigate input-side conducted emissions in the higher frequency spectrum.

### Output Stage & Active Ripple Suppression
<img width="49%" alt="Output Active Ripple Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/OUT_ARF.png" />
<img width="49%" alt="Output Pi Filter" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/OUT_PI.png" />

* Output Active Ripple Filter (ARF): Optimized for high current rails (3 A to 4 A), providing deep low-frequency attenuation where purely passive inductors become excessively bulky.
* Output Pi Filter: Passive LC/Pi network serving as a benchmark to compare insertion losses and parasitic self-resonances against active feedback cancellation.
* Multi-Topology Interconnection Routing: Reconfigurable jumper matrix enabling instant A/B testing between unfiltered baseline, passive filtered, and actively filtered paths.

<p align="center">
  <img height="340" alt="Input Connection Architecture" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/Filtry_sposob_polaczenia_wej.png" />
  <img height="340" alt="Output Connection Architecture" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/PCB/Schematic_MAIN/Filtry_sposob_polaczenia_wyjsciowe.png" />
</p>

### PCB Physical Implementation
<p align="center">
  <img width="49%" alt="Controller IC Hardware Detail" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/MainIC_DC_DC.JPG" />
  <img width="49%" alt="Switching MOSFETs Hardware Detail" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/KEY_MOSFETS.JPG" />
</p>
<p align="center">
  <img width="49%" alt="Active Ripple Filter Circuit Assembly" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/ARF1.JPG" />
  <img width="49%" alt="Active EMI Filter Circuit Assembly" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Photos8k/AEF_IC.JPG" />
</p>

* High di/dt Current Loop Geometry: Experimental testing demonstrated that loop area minimization is paramount. Extending the ground return by small margins or displacing the controller IC away from the switching inductors generated substantial parasitic ringing, leading to duty cycle jitter and total loss of voltage regulation.
* Thermal & Copper Considerations: Active pass elements and high-current power inductors were thermally coupled with multi-via stitching to distribute dissipation during continuous 4 A load cycles.
* Hardware Errata & Component Omission: Due to an erroneous manufacturer-provided footprint, components L1E, L2M, and L1M had to be removed from the prototype boards upon fault identification. Consequently, all experimental characterization and bench measurements were acquired with these specific inductors unpopulated/bypassed.

---

## Experimental Results & Comparative Analysis

Empirical evaluation focused on time-domain ripple extraction, frequency spectrum harmonics, and relative stability limits across loads.

<img width="75%" alt="Vpp Summary Across Topologies" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Vpp_podsumowanie.png" />

### Ripple Suppression & Harmonic Analysis
<p align="center">
  <img width="49%" alt="SEPIC Vpp Filter Comparison" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_Wszystkie_Filtry_SEPIC.png" />
  <img width="49%" alt="ZETA Vpp Filter Comparison" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_Wszystkie_Filtry_ZETA.png" />
</p>

1. Topology Ripple Characteristics: Synchronous SEPIC and ZETA converters demonstrated noticeably lower output current ripple under loads exceeding 1 A compared to the asynchronous Boost. However, due to the severe input-to-output conversion step (nearly a sevenfold differential compared to Boost), SEPIC and ZETA operated at narrow duty cycles, causing higher root-mean-square current stress and lower net efficiency.
2. Asynchronous Boost Limitations: Despite utilizing a single switching element, the asynchronous Boost demonstrated no measurable EMC or switching noise advantage over synchronous designs.
3. Attenuation Limits: Output voltage ripple (Vp-p) was suppressed down to minimal levels at high loading (3 A to 4 A), yielding total harmonic distortion (THD) metrics below 2% across operational bands.

<p align="center">
  <img width="49%" alt="SEPIC 3A FFT Spectrum" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_FFT_SEPIC_3A_CH4.png" />
  <img width="49%" alt="SEPIC 4A FFT Spectrum" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_FFT_SEPIC_4A_CH4.png" />
</p>

### Cross-Topology Comparisons: SEPIC & ZETA vs. Boost
<p align="center">
  <img width="49%" alt="SEPIC vs Boost Channel 3" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_CH3_Wszystkie_Filtry_SEPIC_vs_BOOST.png" />
  <img width="49%" alt="ZETA vs Boost Channel 3" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_CH3_Wszystkie_Filtry_ZETA_vs_BOOST.png" />
</p>
<p align="center">
  <img width="49%" alt="SEPIC vs Boost Channel 4" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_CH4_Wszystkie_Filtry_SEPIC_vs_BOOST.png" />
  <img width="49%" alt="ZETA vs Boost Channel 4" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/Porownanie_Vpp_CH4_Wszystkie_Filtry_ZETA_vs_BOOST.png" />
</p>

4. Relative Voltage Drop as Key Metric: Given varying nominal outputs, different power inductors, switch Rdson, and circuit parasitics across topologies, relative voltage error under varying load represents the most consistent figure of merit for cross-topology comparisons.
5. Vp-p vs. Radiated EMI Trade-off: Minimizing time-domain Vp-p does not correspond to an equivalent attenuation of high-frequency EMI emissions. Active filtering alters spectral distribution, often shifting energy into higher harmonic bands.
6. Transfer Function Alterations & Instability: Integrating active filter loops introduces additional poles, zeros, and spectral components into the overall loop transfer function. Under low-load conditions inducing burst or hiccup modes, these networks can degrade compensation margins, triggering parasitic resonances (particularly evident in passive Pi stages at high frequencies) and system instability.
7. Active Stage Power Dissipation: The active cancellation amplifiers drew static and dynamic currents reaching up to 100 mA to maintain anti-phase cancellation drive, resulting in conversion efficiency degradation of up to 12%.

<img width="75%" alt="Pi Filter Simulation vs Measurement" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/Conclusions/PI_porownanie_symulacji.png" />

---

## Future Improvements

* PCB Footprint Correction: Re-spin the PCB layout to correct the manufacturer library footprint errors for inductors L1E, L2M, and L1M, enabling verification with the complete intended magnetic filtering network.
* Adaptive Digital Control: Design and implementation of active filter parameters dynamically tuned via a real-time Digital Signal Processor (DSP) based on instantaneous load current.
* Standardized EMI Validation: Execution of conducted and radiated emission characterization utilizing a calibrated EMI receiver strictly compliant with the CISPR 16 standard.
* Power Budget & Weight Optimization: Refinement of the active stage power envelope to assess feasibility in spaceborne, avionics, and high-density industrial systems where filtering volume and mass take precedence over baseline efficiency.

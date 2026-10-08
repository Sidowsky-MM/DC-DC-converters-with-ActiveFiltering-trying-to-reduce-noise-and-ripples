# DC-DC Converters with Active Ripple Filtering

<img width="75%" alt="Hardware Setup" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/hardware_setup.png" />

## Project Overview

Goal: Design, implement, and verify an active ripple filtering stage integrated with switched-mode DC-DC converters to suppress output voltage ripple and switching transients without excessively degrading efficiency.

Project Scope:
* Custom PCB & Power Stage: Designed and routed in KiCad, covering both switching topology and active analog filtering.
* Active Filter Topology: High-bandwidth analog stage based on an active ripple cancellation (bypassing/canceling) network.
* Simulation & Modeling: SPICE simulations verifying small-signal loop stability, PSRR, and transient response under step load.
* Laboratory Verification: Time-domain and frequency-domain verification using wideband oscilloscope probes and dynamic electronic load.

---

## System Architecture

<img width="75%" alt="System Architecture Diagram" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/system_architecture.png" />

The system is divided into four main functional blocks:

1. Switching Pre-regulator: A step-down (buck) or boost switching regulator converting bulk input power with high electrical efficiency.
2. Sense & Error Extraction Stage: AC-coupling network designed to extract the AC ripple and switching spike components while rejecting the large DC level.
3. Active Cancellation Loop: Wideband analog amplifier driving an active pass element (BJT/MOSFET) configured in an active ripple cancellation / active filtering topology.
4. Output Distribution & Decoupling: Low-ESR ceramic matrix delivering conditioned, low-noise power directly to the load terminals.

---

## Hardware Details

### The DC-DC Power Stage
<img width="49%" alt="DC-DC Schematic" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/dcdc_schematic.png" />
<img width="49%" alt="Power Stage PCB" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/power_stage_pcb.png" />

The primary power stage is optimized to minimize parasitics and high-frequency EMI generation:
* Input Filtering: High-frequency ceramic decoupling placed directly adjacent to the power switches to minimize the parasitic switching loop area.
* Inductor & Switch Selection: Shielded power inductor paired with synchronous or low forward-drop components to maintain thermal stability.
* Layout Partitioning: Strict separation of high di/dt switching current loops from sensitive analog feedback and active filter traces.

### The Active Filtering Network
<img width="49%" alt="Active Filter Schematic" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/active_filter_schematic.png" />
<img width="49%" alt="Filter Hardware Detail" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/docs/filter_detail.png" />

Unlike bulky, lossy multi-stage LC filters, the active filter uses dynamic feedback to suppress residual switching artifacts:
* AC Coupling & Detection: The output AC voltage component is extracted via a dedicated high-pass network, preserving phase margin at the converter switching frequency.
* Control Loop: An operational amplifier or discrete linear transistor circuit generates an anti-phase correction signal.
* Active Pass/Shunt Element: An active element injects an equal and opposite current (or varies series resistance dynamically) to attenuate ripple across the targeted frequency spectrum.

---

## Simulation & Analysis

Stability and frequency response were modeled in SPICE prior to hardware assembly:

<img width="75%" alt="SPICE Simulation Output" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/simulation/spice_transient_analysis.png" />

1. Small-Signal AC Analysis: Verification of loop gain, phase margin, and PSRR across the operational frequency band (10 kHz to 50 MHz).
2. Transient Load Step: Verification of recovery time and overshoot/undershoot under sudden load current variations.
3. Thermal Dissipation Estimate: Calculating the static and dynamic power loss in the active filtering element to balance PSRR against thermal design limits.

---

## Results & Verification

Measurements were conducted using low-inductance ground springs and a dynamic DC electronic load:

<img width="75%" alt="Raw Switching Ripple" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/measurements/raw_ripple_scope.png" />

1. Unfiltered Baseline: Measurement of output ripple directly at the switching stage output without the active cancellation network engaged.

<img width="75%" alt="Filtered Output Ripple" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/measurements/filtered_ripple_scope.png" />

2. Active Cancellation Verification: Time-domain capture showing substantial reduction in peak-to-peak voltage ripple and attenuation of high-frequency switching edges.

<p align="center">
  <img width="49%" alt="Transient Step Response" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/measurements/transient_step_response.png" />
  <img width="49%" alt="FFT Spectrum Comparison" src="https://raw.githubusercontent.com/Sidowsky-MM/DC-DC-converters-with-ActiveFiltering-trying-to-reduce-noise-and-ripples/main/measurements/fft_spectrum_comparison.png" />
</p>

3. Spectral Analysis: FFT measurement confirming targeted attenuation of the fundamental switching frequency and primary harmonic components.

---

## Future Improvements

* Dynamic Biasing: Implement variable drop-out voltage control over the active pass element to optimize efficiency across wide load currents.
* Bandwidth Extension: Utilize higher GBW operational amplifiers or discrete RF stages to improve attenuation of fast sub-nanosecond switching spikes.
* Integrated Current Sensing: Integrate a dedicated shunt/Hall sensing circuit to monitor load distribution and protect the active pass element under fault conditions.
* Four-Layer PCB Revision: Transition to a 4-layer stackup with continuous inner ground and power planes to further reduce stray inductance and radiated coupling.

---

## Expansion Potential

1. Standalone Bench Supply Module: Package the converter and active filter as a compact, ultra-low-noise front-end module for laboratory equipment.
2. Analog Front-End Supply: Optimize the solution specifically for noise-critical loads, such as wideband RF transceivers, high-speed ADCs, and precision instrumentation amplifiers.

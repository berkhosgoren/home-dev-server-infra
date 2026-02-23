# INCIDENT-003: BIOS Microcode and Idle Stability Tuning After Nouveau Removal

## Summary

Following the resolution of GPU driver instability in INCIDENT-002, the server entered a new phase of testing focused on long-term stability under headless operation.

While disabling the `nouveau` driver eliminated hard freezes, the system continued to experience intermittent reboots after extended uptime, typically between one and two hours during normal SSH usage.

Investigation shifted toward firmware behaviour, CPU idle states, and BIOS microcode differences across available motherboard firmware revisions.

Testing revealed that the newest beta BIOS provided the most stable operating baseline compared to earlier revisions, although occasional reboots still occurred.

## Impact

* Server achieved significantly longer uptime compared to previous incidents.
* Hard system freezes were replaced by controller reboots.
* Infrastructure deployment remained paused pending deeper stability confirmation.
* Extended monitoring cycles were required before continuing with server setup phases.

## Symptoms Observed

* Random reboots occuring after longer idle periods (~1-2 hours).
* System remained stable under CPU stress testing but failed during idle or SSH activity.
* Machine Check (MCE) messages referencing CPU Bank 5 appeared intermittently during boot.
* No consistent thermal or memory errors detected.
* Headless operation increased difficulty in identifying exact reboot triggers.

## Investigation Steps

### BIOS Version Comparison 
Multiple firmware revisions were tested to isolate microcode behavior:


| BIOS Version | Release Date | Type   | Key Changes                    | Test Result                                  |
|---|---|---|---|---|
| 7B87v1G  | 2022-08-18 | Stable | Baseline firmware              | Functional, lower stability after newer kernel |
| 7B87v1H1 | 2023-05-18 | Beta   | AGESA 1.2.0.A, TPM patch       | Functional, reduced uptime; with back to back reboots |
| 7B87v1H2 | 2024-08-09 | Beta   | AGESA 1.2.0.Ca, CVE fix        | Reduced uptime identical to H2|
| 7B87v1H3 | 2024-09-25 | Beta   | Sinkclose uCode fix             | Reduced uptime; freezes and reboots occured more quickly |
| 7B87v1H5 | 2025-09-23 | Beta   | AGESA 1.2.0.F, TPM update      | **Provided longer uptime, most consistent system behavior** |

Result:

**H5 BIOS selected as the baseline firmware for continued testing.**

### Hardware Validation
To eliminate physical causes:
* PSU replaced and verified functional.
* Tested with:

  * single RAM configuration
  * dual RAM configuration

* RAM operated at JEDEC base speed (2133MHz).
* PBO (Precision Boost Overdrive) disabled to reduce aggressive boost behavior.
* CPU stress testing using `stress-ng` completed successfully without errors.

Conclusion:

**Hardware load stability confirmed; failures appeared linked to idle-state transitions rather than compute load.**

## Kernel / Boot Configuration Cleanup

To avoid overlapping variables from earlier troubleshooting:

* Removed experimental GRUB parameters related to CPU idle states.
* Returned kernel boot configuration to a clean baseline after Nouveau removal.
* Verified SSH remained functional under multiple concurrent sessions.

## Root Cause

The instability observed during this phase is believed to be related to firmware-level power management behavior rather than driver or hardware faults.

Key indicators:

* Reboots occured primarily during idle or low-load conditions.
* Stress tests consistently passed.
* Stability varied significantly between BIOS microcode versions.
* H5 BIOS demonstrated improved but not perfect stability, suggesting power-state handling improvements comparet to earlier firmware.

This phase established that the issue had evolved from GPU driver instability (INCIDENT-002) into CPU idle-state or firmware power-management behavior.

## Fixes Applied
* Selected H5 BIOS as primary firmware baseline.
* Disabled PBO to prevent aggressive boost transitions.
* Standardized RAM configuration at base frequency.
* Maintained `nouveau` blacklist from INCIDENT-002.
* Returned GRUB configuration to a minimal, clean state.

## Current Assessment

* GPU driver instability resolved.
* Hardware components validated as functional under load.
* BIOS H5 provided the most stable environment among tested firmware versions.
* System achieved longer uptime windows, although intermittent reboots still occured after extended operation.

At this stage, the system was considered **partially stabilized** but required further electrical or firmware tuning to eliminate remaining reboot events.

## Next Actions (Transition to INCIDENT-004)

* Investigare CPU idle voltage behavior.
* Test controlled voltage offset adjustments.
* Monitor Machine Check (Bank 5) events.
* Confirm long-term uptime stability before resuming infrastructure deployment.

## Lessons Learned
* Removing one instability layer (GPU driver) can reveal deeper firmware-level behavior.
* BIOS microcode revisions can significantly impact idle stability on Ryzen platforms.
* Incremental, single-variable testing is essential when isolating firmware-related issues.

## Status

* Baseline stability achieved using H5 BIOS.
* Monitoring continued as investigation moved into CPU voltage and idle-state tuning (INCIDENT-004).


# INCIDENT-004: CPU Machine Check Errors and Long-Uptime Reboots Resolved via BIOS Voltage Offset

### Summary

After stability improvements achieved in INCIDENT-003, the server continued to experience occasional reboots accompaniced by Machine Check Exception (MCE) messages referencing CPU bank 5.

Multiple BIOS versions were tested to isolate firmware-related instability. While BIOS H5 proved to be the most stable baseline, intermittent reboots occured after extended uptime.

Further investigation identified aggressive automatic CPU voltage behavior as a likely trigger. Applying a negative CPU voltage offset in BIOS significantly improved system stability, allowing sustained long-duration uptime with multiple concurrent SSH users.

## Impact

* Random reboots after long uptime periods.
* Difficult to diagnose because system could remain stable for hours.
* Infrastructure setup paused to avoid data corruption during unexpected resets.
* SSH sessions dropped without warning.

## Symptoms Observed

* Kernel logs showing recurring entries:

  * `mce: [Hardware Error]: CPU X: Machine Check: 0 Bank 5`
* Reboots occuring during idle or light SSH activity rather than heavy load.
* No consistent high temperature or load spikes:

  * CPU temps ~35-47°C.
  * Load average near zero.
* Stress tesling and multi-user SSH sessions did not immediately trigger failure.
* BIOS reporting elevated automatic core voltage levels.

## Investigation Steps

Following stabilization work documented in INCIDENT-003, the investigation focused specifically on remaining Machine Check Expection (MCE) reboots.

### Baseline Context (From Previous Incidents)
Prior incidents had already validated:

* PSU replacement and power delivery stability.
* RAM tested in single and dual configurations.
* Nouveau driver removal and official NVIDIA driver installation.
* Kernel and GRUB power-state experiments.
* BIOS revisions tested with H5 identified as the most stable firmware baseline.

Because hardware and driver layers were already isolated, attention shifted toward CPU voltage behavior under newer BIOS firmware.

## Identifying Voltage-Related Instability

Observations leading to voltage tuning:

  * Persistent MCE errors referencing CPU cores (Bank 5).
  * Stable temperatures and low system load ruled out thermal or stress causes.
  * Reboots occured during idle or light SSH activity rather than heavy computation.
  * BIOS reported aggressive automatic core voltage behavior (~1.46V spikes).

These factors suggested transient voltage instability rather than hardware failure.

## Targeted BIOS Adjustment

Instead of further firmware or driver changes:

* Applied a negative CPU voltage offset in BIOS.
* Maintained:

  * PBO disabled
  * RAM at base frequency
  * Existing driver configuration unchanged

Result:
* Immediate improvement in uptime stability.
* Multi-user SSH sessions sustained for extended durations.
* No new MCE entries during long monitoring windows.

## Fixes Applied

* Selected BIOS H5 as operational firmware baseline.
* Applied negative CPU voltage offset via BIOS offset mode.
* Kept PBO disabled to avoid voltage spikes during testing.
* Continued monitoring with:

  * `watch -n 30 uptime`
  * periodic `dmesg` checks
  * background uptime logging 

## Current Assessment

* Voltage instability appears to have been the remaining trigger for MCE-related reboots.
* Server achieved extended uptime exceeding previous stability limits.
* System remained responsive under multiple concurrent SSH sessions.
* No additional kernel-level GPU or driver conflicts observed after NVIDIA driver installation.

## Next Actions

* Continue long-duration monitoring to confirm stability beyond 24-hour cycles.
* Gradually reintroduce performance features (e.g., PBO) only after baseline is confirmed.
* Resume infrastructure phases from stable hardware baseline starting with Storage Configuration.

## Lessons Learned

* BIOS auto-voltage behavior can introduce instability even when hardware is otherwise healthy.
* Firmware updates may require voltage tuning rather than reverting to older BIOS versions.
* Machine Check Exceptions under low load can indicate transient voltage issues rather than thermal or memory faults.
* Structured incident tracking helped isolate firmware vs. driver vs. hardware layers efficiently.

## Status

* **Stability significantly improved after voltage offset adjustment.**
* Monitoring ongoing to confirm long-term reliability.

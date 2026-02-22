# INCIDENT-002: System Freezes Under SSH Load After PSU Replacement

## Summary

After replacing the PSU following INCIDENT-001, the server continued to experience stability issues.
Instead of immediate power loss or reboots, the system began freezing during normal SSH usage or after 
extended uptime.

Investigation revealed that the instability was not caused by power delivery, RAM failure, or CPU load, but by a GPU 
driver conflict involving the open-source `nouveau` driver on a headless Ubuntu Server installation.

Disabling the `nouveau` driver restored long-term stability.

## Impact

* Server would freeze completely (no reboot, no SSH response).
* Headless operation made failures difficult to detect remotely.
* Uptime rarely exceeded ~60-70 minutes before lockups.
* Infrastructure phases were paused until stability was confirmed.

## Symptoms Observed

* Freeze without reboot; system became unresponsive.
* SSH connections triggered freezer more reliably than CPU stress.
* CPU stress testing (`stress-ng`) completed successfully, suggesting load stability.
* Kernel boot messages occasionally referenced CPU bank/MCE warnings but without consistent fatal errors.
* System could run longer with temporary GRUB parameters but instability persisted.
* Stability dramatically improved when booted with:

  * `nomodeset`

## Investigation Steps

## Hardware Isolation

* PSU replaced with new unit.
* Tested with:

  * Single RAM stick
  * Dual RAM configuration
* Disabled PBO (Precision Boost Overdrive).
* Reflashed BIOS and updated to newer beta version.
* Verified RAM speeds (2133 MHz base).

## Kernel / Power State Testing
* Removed custom GRUB CPU power-state parameters.
* Tested:

  * `idle=nomwait`
  * C-state related BIOS changes

These increased uptime but did not eliminate freezes.

## Driver / Graphics Stack Investigation

Because freezes occurred during idle/SSH rather than under load:

* Suspected GPU console driver interaction.
* Booted with temporary GRUB edit:

  * `nomodeset`
    
Result:

* System ran significantly longer without freeze.
* Confirmed GPU driver stack involvement.

## Root Cause

Open-source NVIDIA driver (`nouveau`) caused kernel-level instability on this headless Ryzen + RTX 2060 Super configuration.

Likely Factors:

* Headless Ubuntu Server install using framebuffer console.
* RTX GPU initialized without proper headless driver stack.
* Kernel modesetting leading to deadlock during console redraw or network activity.

Explains why:

* Stress testing passed.
* Freezes occurred during SSH/network interaction.

## Fix Applied

## Permanently Disable Nouveau

Created blacklist file: 

  * `/etc/modprobe.d/blacklist-nouveau.conf` 

Contents: 

  *
    ```bash
     blacklist nouveau
     options nouveau modeset=0
    ```

Rebuilt initramfs:

  * `sudo update-initramsfs -u`

Update GRUB configuration:

  * `sudo update-grub`

Removed temporary `nomodeset` parameter after confirming stability.

## Current Assessment

* Hardware (PSU, CPU, RAM) appears stable.
* Primary instability source identified as GPU driver stack.
* Server now operates in GPU-neutral headless mode pending official NVIDIA headless driver installation.

## Next Actions

* Observe extended uptime to confirm stability baseline.
* Install official NIVIDA headless driver (no GUI).
* Continue infrastructure phases:

  * Storage setup
  * Docker installation
  * SQL Server container

## Lessons Learned

* GPU drivers can introduce instability even on headless servers.
* Stress tests alone do not guarantee kernel stability.
* Incremental single-variable testing (BIOS, GRUB, RAM, drivers) was critical to isolating root cause.

## Status

* Stable baseline achieved.
* Monitoring uptime before proceeding with infra setup.




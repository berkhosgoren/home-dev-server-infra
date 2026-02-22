# INCIDENT-001: Random Reboots + SSH Unreachable

## Summary

The server experienced intermittent unexpected reboots. When this occurred, SSH connectivity from the main PC would fail (timeout/refused).
This incident was investigated while preparing the server for headless operation.

## Impact

* SSH sessions dropped unexpectedly.
* Server became unreachable from LAN during/after reboot events.
* Progress on storage setup was blocked until stability and access were restored.

## Symptoms Observed

* Random reboots without obvious external indication (headless).
* `ssh ...` returned `connection refused` or `connection timed out`.
* At times, firewall/network profile changes affected ping/SSH.
* Server occasionally had both a static IP and a DHCP secondary IP.

## Investigation Steps

* Verified SSH service state:

  * `systemctl status ssh`
* Verified sshd listening on port 22:

  * `sudo ss -tulpn | grep 22`
* Checked IP addressing and routes:

  * `ip a`
  * `ip route`
* Reviewed boot/journal logs around reboot events:

  * `journalctl -b -1`
  * `journalctl -k`
* Ran CPU stress tests to reproduce:

  * `stress-ng ...`
* Checked for machine check / hardware signals:

  * `journalctl -k --grep="MCE|Hardware Error|WHEA|EDAC|Machine check"`

## What Helped / Fixes Applied

* Restored SSH availability by ensuring service was enabled/started:

  * `sudo systemctl enable --now ssh`
* Firewall issue confirmed: disabling firewall allowed SSH to reconnect.

  * Later plan: keep firewall ON, explicitly allow SSH (port 22) from LAN.
* Network profile change (on client side) impacted reachability; switching to Private improved ping/SSH behavior.

## Current Assesment

* Network/firewall configuration can block SSH and mimic server downtime.
* Unexpected reboots appear consistent with a power stability issue (suspected PSU/power strip path), especially given the intermittent nature.

## Next Actions

* Replace PSU (planned).
* After hardware stability improves:

  * Re-enable firewall with explicit rules for SSH (LAN only).
  * Continue with storage configuration for SMB shares.

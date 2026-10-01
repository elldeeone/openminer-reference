# BM2382 unknowns

This reference is a work in progress. An unknown is not a zero value, a safe
default or a borrowed specification from another chip.

## Required for a new miner board

| Unknown | Why it is required | Evidence that can close it |
| --- | --- | --- |
| Package and land pattern | Fit and assembly | Verified package measurements and footprint |
| Pin and rail map | Correct connectivity | Applicable specification or measured stock-board mapping |
| Rail voltage/current/sequence | Regulator and startup design | Measured domains, current and sequencing under stated conditions |
| IO levels and clock | Safe interface design | Verified signal levels, loading and reference clock |
| One-chip/short-chain startup | Independent board control | Qualified startup, work, valid PoW and restart on that topology |
| Thermal/mechanical integration | Cooling and reliability | Verified contact, mounting and temperature response |

These prevent a complete independent hardware reference. They do not prevent
publishing or implementing the documented stock-board interface.

## Remaining driver qualifications

| Unknown | Current boundary |
| --- | --- |
| Other board counts and chain lengths | Runtime was tested with two 92-position chains on a three-hashboard KS5 Pro; the site power limit prevented three-hashboard mining |
| Model-specific operating settings | Clock, power and thermal settings were measured on the KS5 Pro. Differences between miner models were not compared. |
| Other pool job/target formats | Current adapter uses four words, timestamp and positive integer difficulty |
| Other baud/frequency recipes | Preserve the complete tested recipe |
| Long unattended operation | Evidence covers bounded sessions and recovery |
| Cutoff after Linux, SoC or safety-owner failure | Included physical tests cover pool EOF and normal Stop. They do not establish a shutdown mechanism for all controller failures. |
| Rail discharge after cutoff | Low output-enable and reset GPIOs are confirmed. Chip rail discharge time was not measured. |
| Absolute sensor accuracy | Stock conversion is available; external calibration is absent |

## Optional internal research

The complete register field map, full reset-state table, counter width and
counting event, and general diagnostic read effects remain open. Physical lane
count, general chip/core attribution for arbitrary work, internal queue order
and the higher-clock fixed-job discrepancy also remain open.

Selected work images have bounded causal chip/core attribution and tested
output serialization. These results do not establish a general ownership law.
See [research notes](research-notes.md).

A driver must validate PoW, bind results to current work and retain tested
safety controls. It does not require a full silicon explanation of these
optional questions.

## Add evidence

Close only the claim that an experiment tests. State the conditions and the
result. Preserve failures and competing explanations. Use
[contribution guidance](../../../CONTRIBUTING.md).

[BM2382 overview](README.md) · [Evidence](evidence.md) · [Unknowns](unknowns.md)

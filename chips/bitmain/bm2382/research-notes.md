# BM2382 research notes

These findings preserve the deeper characterization. A driver can use the
supported stock recipe without solving the remaining internal models.
Each result applies only to its stated work, configuration and collection
window. The [evidence catalog](evidence.md) identifies the retained sources.

## Fixed-job prediction

`proven`: a 300 MHz reference model predicted 6,358 candidates across eight
40 s windows. Scores, framing, prefixes and the complete output buffers agreed.
No unexplained output bytes remained in those windows.

`strongly indicated`: prefix, seed and generation changes control repeatable
search assignments in the tested state. Held-out contrasts support the model.
Observed recurrence intervals include 13.7439616 s in the earlier clock context
and 28.6332736 s at 300 MHz.

`unknown`: unique silicon anatomy. Equivalent 126-lane and 128-lane allocation
models survive. Returned chip/core coordinates overlap the nonce. Frame bytes
alone do not prove a unique chip owner. Finite matched windows do not prove
indefinite wrap behaviour.

## Causal chip and core attribution

`proven`, bounded: seven chip masks assigned all 160 timed qualified result
opportunities to the 92 chip positions in the tested chain. Each chip owned at
least one opportunity. Three selected core changes removed their predicted
occurrences. Exact restoration returned those occurrences. The returned core
field's low seven bits agreed with the sampled core predictions.

The 160 opportunities contained six different complete frames. One identical
frame came from 34 physical chips. Chip attribution used the work image,
occurrence time and causal controls. It did not use a unique chip ID in the
frame.

A subsequent 300 MHz test used three held-out work images and fresh work IDs.
All 23 comparison windows matched the predictions for result family, time,
core field and selected chip/core omissions. All 4,746 captured nonce frames
passed the work-image qualification check. No per-run model adjustment was
used.

**Acceptance limit:** the first chip-mask run failed its complete nonce gate.
Four other CRC-valid frames failed stock-hash qualification. The 160 qualified
opportunities support the causal map. They do not make the complete run pass.
The subsequent held-out test passed its bounded physical acceptance gate.

**Unknown:** physical lane count and general attribution for arbitrary work.
These tests do not distinguish the equivalent 126-lane and 128-lane models.

Sources: `causal-chip-ownership` and `held-out-allocation` in the
[evidence catalog](evidence.md).

## Duplicate attribution

`proven`, bounded: two occurrences of one identical complete result frame came
from two distinct chips and their selected core-105 positions. Each selected
core change removed only its own timed occurrence. A sibling-core control
removed neither occurrence. Exact restoration returned both. A paired chip
change removed both occurrences.

This establishes the cause of that duplicate class. It does not establish the
cause of every repeated nonce or the behaviour of simultaneous internal results.
A driver must deduplicate nonces in software. It must not depend on unique
physical owner attribution.

Source: `duplicate-causal-attribution` in the [evidence catalog](evidence.md).

## Selected output contention

`proven`, bounded: both results from a selected source pair survived slow-UART
contention in A-before-B order. Separate source controls and repeated combined
controls agreed. A held-out source pair also retained both results in that order.

| Observation | Selected main pair | Held-out pair |
| --- | ---: | ---: |
| Fast-UART result separation | 145.52 µs | 926.40 µs |
| Slow-UART result separation | 1.44412 ms | 1.44624 ms |
| Fast separation after restoration | 145.48 µs | Not stated here |

All 28 sampled chip-counter intervals matched the attributed output counts.
The tests establish selected serialization and retention. They do not establish
simultaneous internal generation, queue capacity or a general absence of drops.

**Acceptance limit:** the original epoch gate failed because it assigned a
boundary result by parser time. A separate recheck used physical wire arrival
and passed all 7,353 frame qualifications. The original failure remains.
Two unrelated qualified frames had owner-model timing errors of 37–45 ms,
outside the frozen 5 ms tolerance. Their delays remain unexplained.

Source: `selected-contention-and-active-state` in the
[evidence catalog](evidence.md).

## Active core-0a state

`proven`, bounded: selected active cores returned `000010` and accepted a
directed clear to `000000`. Qualified target output continued while bracketing
reads returned zero. The state later returned to `000010`.

A 300 MHz repeat tested chip position 37/core 29 and chip position 91/core 103.
Their target frames appeared 14 times and three times, respectively, between
bracketing zero reads. Sibling and other-chip controls retained `000010`.
Core register `02` retained `000025` on both targets.

Reassertion followed the original work phase rather than the elapsed time since
clear. An earlier independent pair gave the same bounded clear/output/reassert
behaviour. These observations are separate from the idle/reset `1→0` test in
[core register 0a](core-registers.md#0x0a-writable-core-state).

`unknown`: the internal trigger and meaning of bit 4. The firmware word
“overflow” does not establish an overflow, saturation or general recovery law.
The stock driver recipe does not require this diagnostic write.

Sources: `selected-contention-and-active-state` and
`active-state-and-read-contrast` in the [evidence catalog](evidence.md).

## Counters and read effects

`proven`: register `3c` can be cleared on one selected ASIC without clearing
held-out selectors. Values 65535→65536→65537 establish at least 17 observed
counter bits. They do not establish the complete width or wrap point.

**Acceptance limit:** the boundary run failed its original wrapper and affine
clock gates. A separate physical wire-order synthesis established the counter
observation. It required the complete 65,610-packet physical TX sequence, exact
native/RX prefix and all 65,537 expected replay pairs. This is independent
counter evidence. It is not ordinary complete-run acceptance.

Directed reads at the selected 300 MHz reference point had at least 347.52 µs
from the selected reply to the first nonce. Matched trials had no count loss.
This is a bounded observed policy, not a universal safe-read interval.

Broadcast tests on a selected ASIC returned two or three results while the
counter increased by one. The deficit persisted in late reads. A qualifying
nonce began 262.4 µs before the selected reply ended in the correlated test.
Separate selected-ASIC tests counted four outputs. The complete internal
count/read/output order remains unknown.

Earlier repeated tests compared broadcast reads of `3c`, broadcast reads of
`1c` and no broadcast read. Both broadcast registers gave one increment for
two attributed outputs on the selected chip. No-read controls gave two
increments. This supports a broadcast-read interaction in those trials. It
does not establish a defect specific to `3c`.

Do not use this diagnostic counter for share credit. Do not infer that all
broadcast reads are unsafe. Stock temperature service remains part of the
supported controller recipe.

Sources: `counter-seventeen-bit-boundary` and `active-state-and-read-contrast`
in the [evidence catalog](evidence.md). The directed-read policy has its own
bounded source record.

## Output-filter contrast

`proven`: the core-02 sequence `25→27→25` excluded score-38 opportunities in
selected tests while higher-score output continued. Count/output deltas agreed
in the matched windows. The tested pairs gave 6/6→12/12→3/3 and
5/5→10/10→3/3. Window lengths differed; these are not rate comparisons.

`unknown`: a universal register-to-score equation, queue order and the exact
counter event. Repeated surviving results were counted. This does not establish
one unique internal filtering stage.

## Higher-clock discrepancy

Earlier higher-clock fixed-job tests contained 38 below-filter-score results.
The cause remains unknown. The 300 MHz matched reference cannot be extended
to claim lossless prediction for that higher-clock context.

## Historical stock observations

The earlier observations below used the stock controller. They do not extend
the operating qualifications of the open controller.

| Observation period | Observed stock behaviour | Limit |
| --- | --- | --- |
| March 2026 | One board reached address initialization on connectors 0 and 2. Two fitted chains mined. | This does not demonstrate one-chip mining or connector-1 operation. |
| April 2026 | Cold startup, steady changing work, temperature service, adaptive bias writes and warm service recovery. | Observed stock branches. They do not qualify arbitrary open-controller settings. |
| June 2026 | High-speed stock work and result traffic returned after a power cycle. | The electrical cause and a universal recovery rule were not established. |
| Later degraded stock observation | Chain counts were 88 and 92. Four fans, work/results and 54 complete temperature cycles were recorded. | The cause of the missing positions remains unknown. Later 92/92 boots do not identify that cause. |

Source: `historical-stock-observations` in the [evidence catalog](evidence.md).

## What this establishes

The protocol and supported stock-board control do not depend on a unique
lane model, complete counter width or hidden FIFO explanation. Keep these
questions open. New evidence must state the exact workload, configuration,
read policy, collection interval and software-validation method.

[BM2382 overview](README.md) · [Evidence](evidence.md) · [Unknowns](unknowns.md)

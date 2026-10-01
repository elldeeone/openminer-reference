# BM2382 evidence

The reference combines several months of research. The source snapshot is
`7bf903ca7ba8f75c7b31fbeaf1888d236ddf892c`. The latest included physical results
are dated 1 October 2026. The later seed-binding correction is an offline
reduction. Its source revision is recorded in the binding-correction asset.

The miner owner identified the test unit as a KS5 Pro. It has three
hashboards. Tests used two mining hashboards because of the site power limit.
The research records use `KS5` as a family label.

BITMAIN lists separate [KS5 Standard specifications](https://support.bitmain.com/hc/en-us/articles/29471533336345-KS5-Specifications)
and [KS5 Pro specifications](https://support.bitmain.com/hc/en-us/articles/29937567040153-KS5-Pro-Specifications).
These are miner-level specifications. The operating settings and measurements
in this reference come from the KS5 Pro test setup.

The research source is private. Retained evidence includes decoded captures,
run records and selected raw archives. Some raw samples were deleted under the
retention policy. This reference includes selected static extracts. A source
hash identifies an original asset. It does not make that original available
or reproduce its experiment.

The [source catalog](evidence/source-catalog.json) gives descriptive source IDs,
record hashes, source version dates, conditions and evidence availability.
A source version date identifies the saved document version. It is not an
experiment timestamp unless the record states that timestamp.

## Public assets

| Asset | Contents | What it establishes |
| --- | --- | --- |
| [Initialization TSV](evidence/initialization.tsv) | 340 ordered packets, waits and stages | Exact UART recipe data for each chain; use the documented two-chain orchestration |
| [Address responses](evidence/address-responses.hex) | 276 ten-byte frames in wire order | Exact address-response gate, including CRC and selector order |
| [Mining example](evidence/mining-example.json) | Original job words, timestamp, work/result frames, nonce, digest and target | One captured, software-valid, pool-accepted share |
| [Register observations](evidence/register-observations.json) | 14 ASIC and four core targets | Scoped observation statements and explicit unknowns |
| [Validation summary](evidence/validation-summary.json) | Fresh-job, recovery and fixed-job results | Reported run outcomes and their boundaries; not a complete waveform archive |
| [Source catalog](evidence/source-catalog.json) | Descriptive source IDs, hashes, conditions and retention | Links a claim to a saved source record; does not publish private originals |
| [Manifest](evidence/manifest.json) | SHA-256 hashes and original-source identities | Integrity of the selected extracts |

The initialization file removes private collection filenames and packet labels.
Packet bytes, row order and waits are unchanged. The address-response file
converts original bytes to hexadecimal lines without changing frame bytes.
The share example replaces the worker label and removes private collection
metadata. All values needed for packet and PoW replay remain unchanged.

## Claim and source map

| Claim group | Source type | Public representation | Limit |
| --- | --- | --- | --- |
| Frame layouts and CRC | Firmware transport plus primary UART capture | Protocol tables, CRC examples and static frames | One historical low-speed frame failed CRC |
| Startup | Frozen Rust initializer plus physical fresh-job runs | TSV, response fixture and orchestration instructions | Stock topology, power/reset integration required |
| Register/reset behaviour | Directed physical comparisons with held-out selectors and restoration | Register tables and observation JSON | No target has a complete field or reset definition |
| Fixed-job search | Eight complete physical output windows, matched offline prediction | Research notes and validation summary | 300 MHz context; non-unique lane models remain |
| Counter/read/filter | Correlated physical read and nonce windows | Research notes | Selected access policy and workloads only |
| Fresh jobs and pool credit | Physical UART, controller events and pool replies | Validation summary and exact share example | Only a subset of accepted results is directly present on the tap |
| Fault and recovery | Pool EOF, terminal GPIO, fresh-owner and restoration records | Operation and validation summary | Separate final-park closeout required |
| Chip electrical design | No verified package/rail/clock specification in the included evidence | Hardware and unknowns | New-board requirements remain open |

## Historical primary capture

The original reference preserved these checks: 3,227/3,227 work CRCs,
1,276/1,276 control CRCs, 24,196/24,196 fast status CRCs,
1,747/1,748 low-speed status CRCs and 13,747/13,747 result CRCs.
These counts are historical summaries. This change does not publish the full
original capture. They are not counts from the October fresh-job runs.

## Selected findings and failed gates

A selected physical finding can remain supported when another gate in the
same run fails. The source catalog states these limits. Do not treat a
successful offline recheck as acceptance of the original complete run.

- The causal ownership result covers all 92 positions in its fixed-job test.
  Four other CRC-valid frames failed the original nonce qualification gate.
- The selected output-contention result passed a separate physical recheck.
  The original epoch gate failed, and two owner-timing exceptions remain.
- The counter progressed through 65535, 65536 and 65537. A separate
  wire-order synthesis supports this boundary. The original wrapper and
  affine timing gates failed.
- Fresh-job run A failed its EOF closeout. Run B demonstrated EOF, a fresh
  owner and normal Stop, but failed its final stock park. A separate closeout
  completed that park.

## Offline result binding correction

The original fresh-job reducer checked the work ID, prefix and age. It omitted
one production check: the returned seed. Run A had no seed mismatches. Run B
had three. None was a pool-accepted share.

The [validation summary](evidence/validation-summary.json) preserves the
original joined counts and gives the seed-qualified counts. The
[binding correction](evidence/binding-correction.json) identifies the three
frames, the source hashes and the corrected reduction hashes. The corrected
join still includes some results from retired jobs. It is not a complete
production candidate inventory. Production applies its own current-job and
session policy.

All work and result counts in these fresh-job captures describe chain 2.
During sustained native mining, the reported ASIC temperature extrema also
describe chain 2. Startup temperature gates cover both chains. Pool accepted
share totals cover the selected controller sessions.

## Review checks

The preparation checks validate all 340 initialization packet CRCs, all 276
address frame CRCs, fixture selectors, the share packet fields and the asset
hashes. The actual production Rust PoW verifier reproduces the share digest
and target decision. Link, anchor and SVG checks cover the handbook files.

The diagram check reads the existing SVGs and metadata. It checks metadata, field coverage and existing hashes without changing
the repository or manifest. Rendering checks use a separate temporary directory.
Field-layout checks verify widths and offsets. Packet-example checks verify
length and CRC coverage. The SVG files are the editable figure sources. JSON
files contain supplementary metadata; see the [figure workflow](images/README.md).

The register claims were checked against the original command order. Reset
retention applies only to the marker or restored word named in each section.
These are offline checks of the reference. They do not constitute another
powered test. Complete independent physical auditing still needs the original
run evidence or a new controlled replication.

[BM2382 overview](README.md) · [Evidence](evidence.md) · [Unknowns](unknowns.md)

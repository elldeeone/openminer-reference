# Contribute evidence

Use short, direct sentences. Use one term for each item. Define an abbreviation
at first use. Keep an instruction separate from its explanation.

## Evidence labels

| Label | Meaning |
| --- | --- |
| `proven` | A physical observation supports the stated claim under stated conditions |
| `strongly indicated` | Multiple observations support the claim; a material alternative remains |
| `hypothesis` | A proposed explanation that needs a discriminating test |
| `unknown` | The available evidence does not establish an answer |

A firmware constant proves a firmware value. A software test proves software
behaviour. Neither alone proves physical chip behaviour. State that distinction.

## Submit a change

1. State the chip, board, controller, topology, software revision and conditions.
2. State the observation, its evidence and the claim it supports.
3. State the limits and the explanations that remain possible.
4. Include a small reproducible packet, table or measurement extract. Record
   source and derivative hashes. Preserve a failed result when it affects scope.
5. Check links, tables, units, checksums and examples before submitting a change.

This repository contains documentation and static evidence. Keep executable
drivers, decoders, bench-control scripts and deployment tools in their own
repositories. Link a public implementation when its access and license permit it.

## Diagram changes

Edit the SVG with a standard SVG editor or XML editor. Update its supplementary
JSON metadata in the same change. Check offsets, widths, checksum coverage,
labels and rendering. Keep unknown fields explicit. See the
[diagram editing guide](chips/bitmain/bm2382/images/README.md).

## Public evidence

Remove credentials, worker/account identifiers, private network addresses and
bench access details. Describe redactions. Keep hash-replay inputs unchanged
when publishing a PoW example. Obtain the rights needed to share each asset.
Do not copy firmware binaries or decompiler output into this repository.

Submit original documentation, diagrams and evidence descriptions under
CC BY 4.0. Identify third-party material and its terms separately. See the
[license scope](LICENSING.md).

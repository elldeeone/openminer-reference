![Open Miner](./openminer-ansi.png#gh-light-mode-only)
![Open Miner](./openminer-ansi-white.png#gh-dark-mode-only)

<p align="right"><strong>“If you can’t open it, you don’t own it.”</strong></p>

---

# Open Miner ASIC Reference

Chip and hardware reference documentation for Open Miner.

Open Miner is an open platform for mining hardware, firmware and control.
This repository documents ASIC protocols, register observations, initialization
sequences, test evidence and remaining unknowns.

> [!NOTE]
> This is community research, not manufacturer documentation. Each claim states
> its evidence and tested conditions. Unknown fields remain explicit.

## Chip references

| Chip | Tested hardware | Current progress |
| --- | --- | --- |
| [Bitmain BM2382](chips/bitmain/bm2382/README.md) | KS5 Pro; two mining hashboards used because of the site power limit | Stock-hashboard control demonstrated. Chip electrical mapping and independent miner-board validation remain open. |
| [Goldshell IEN616](chips/goldshell/ien616/README.md) | KA Box. One stock hashboard with firmware 2.2.2. | Stock-hashboard control is proven after stock preparation. Chip electrical mapping, independent cold boot and independent miner-board validation are not complete. |

## Contribute

See [CONTRIBUTING](CONTRIBUTING.md) for evidence labels and submission guidance.
Contributions can add electrical measurements, validate another topology or
correct a documented observation.

This repository contains documentation and static evidence. Executable drivers
and bench tools belong in their own repositories.

## License

The original documentation, technical diagrams and evidence descriptions are
licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
The Open Miner name and logos are excluded. The README quotation and identified
third-party material are also excluded.

See the [full license](LICENSE), [license scope](LICENSING.md) and
[branding provenance](PROVENANCE.md).

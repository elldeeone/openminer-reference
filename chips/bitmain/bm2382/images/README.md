# Edit the diagrams

SVG is the editable source and the displayed image. Use a standard SVG editor,
such as [Inkscape](https://inkscape-manuals.readthedocs.io/en/1.3/saving.html),
or edit the SVG XML directly. Save the result as plain SVG.

The JSON files are supplementary metadata. They record field layouts, scope
notes and checksum coverage. They use the custom `openminer-diagram-v1` format.
They are not WaveDrom or Bitfield input. No private renderer is required to
edit the published SVGs.

## Field metadata

Register and packet definitions contain:

| Key | Meaning |
| --- | --- |
| `kind` | `register` or `packet` |
| `width` | Total bits or bytes, as specified by `unit` |
| `fields` | Field offset, width, label and display classification |
| `notes` | Scope, observation limits and checksum explanation |
| `checksum` | Packet checksum kind and exact coverage, when applicable |

A register offset is the least-significant bit position. A packet offset is
the zero-based byte position in wire order. Fields must cover the complete
access view without gaps or overlaps. A transport width does not prove the
physical width or function of an internal register.

Display classifications are `known`, `tested` and `unknown`. `known` identifies
a supported layout or value. `tested` identifies a bounded observed contrast.
`unknown` leaves the internal meaning unnamed. A question mark labels an
unknown field that is too narrow for the full word. These classifications
do not replace the handbook's evidence labels.

The [JSON schema](schema/openminer-diagram-v1.schema.json) defines the metadata
structure. Field coverage and checksum coverage require additional checks;
the schema alone does not validate those relationships.

## Drawing metadata

Combined layouts, the initialization sequence and the hardware block diagram
use `kind: drawing`. Their `svg` object records the SVG root and child nodes.
Each node has a tag, attributes, text and child nodes. This is an SVG-tree
snapshot. It is not a semantic sequence or circuit model.

## Change a diagram

1. Check the supporting observation before changing a field or label.
2. Edit the SVG. Preserve the numbered fields, scope notes and unknown fields.
3. Update the matching JSON metadata. Keep offsets, widths and units explicit.
   For a drawing, update its SVG-tree snapshot to match the edited image.
4. Check field coverage, checksum coverage, text fit and the rendered image.
5. Update the SVG and JSON hashes in [the manifest](../evidence/manifest.json).
6. Submit the SVG and JSON together.

SVG controls the appearance. JSON records the described data. If they disagree,
correct the difference before submitting the change. Do not use a generated
image to overwrite the image under review during a verification check.

## Layout conventions

The existing field diagrams have a 1,000-unit view width. Their field area
extends from x=52 to x=948. Register bit b has its left edge at
`x=52 + (width-b-1) * 896/width`.

Packet offsets increase from left to right. Packet rows contain up to 16 bytes.
Long packets wrap after each group of 16. Bit numbers decrease from left to
right. Keep unknown fields hatched. Use amber for tested contrasts and blue
for supported layouts.

These conventions describe the current images. A new image can use a different
size if its numbered fields and scope remain clear.

[BM2382 overview](../README.md) · [Contribution guidance](../../../../CONTRIBUTING.md)

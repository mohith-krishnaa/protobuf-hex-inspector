# Protobuf Hex Inspector

A lightweight, client-side developer utility for inspecting **schema-less Protocol Buffers (Protobuf) HEX payloads** when the original `.proto` schema is unavailable.

**[Live Demo](https://mohith-krishnaa.github.io/protobuf-hex-inspector/)** · **[Source](https://github.com/mohith-krishnaa/protobuf-hex-inspector)**

## Interface

![Protobuf Hex Inspector overview](./assets/protobuf-overview.jpg)

Protobuf Hex Inspector is designed for quickly understanding the **wire-level structure** of a payload without pretending to know application-specific schema semantics.

## What it does

- Accepts raw Protobuf HEX input
- Validates and normalizes HEX
- Parses field keys and wire types
- Reads 64-bit-safe varints with JavaScript `BigInt`
- Shows unsigned, signed 64-bit and ZigZag interpretations for varints
- Handles length-delimited fields
- Detects printable UTF-8 text
- Attempts recursive nested-message inspection
- Exposes fixed32/fixed64 raw bytes and numeric interpretations
- Shows field-level metadata and raw field bytes
- Groups repeated fields in the JSON view
- Measures payload size, field count and decode time
- Copies decoded JSON to the clipboard
- Saves decoded JSON locally
- Runs entirely in the browser

## Decoded output

![Decoded Protobuf output](./assets/protobuf-decoded-output.jpg)

The field inspector separates the human-readable JSON view from the underlying wire-level information, making it easier to debug malformed or unfamiliar payloads.

## How it works

```text
HEX input
   ↓
HEX validation / normalization
   ↓
bytes
   ↓
field key
   ↓
field number + wire type
   ↓
wire-specific decoding
   ↓
field metadata + interpreted value
   ↓
JSON / inspector table
```

## Supported wire types

| Wire type | Meaning | Support |
|---:|---|---|
| `0` | Varint | ✅ unsigned / signed / ZigZag views |
| `1` | 64-bit | ✅ raw HEX / uint64 / float64 |
| `2` | Length-delimited | ✅ text / bytes / nested-message attempt |
| `3` | Start group | ❌ unsupported |
| `4` | End group | ❌ unsupported |
| `5` | 32-bit | ✅ raw HEX / uint32 / int32 / float32 |

### Important schema limitation

Without the original `.proto` schema, the inspector **cannot know the intended semantic type** of a field. For example, a wire-type `0` value could represent a `uint64`, `int64`, `sint64`, boolean, enum, or another application-defined value.

The numeric interpretations shown by the tool are therefore **wire-level possibilities, not guaranteed semantic types**.

## Usage

1. Open the [live demo](https://mohith-krishnaa.github.io/protobuf-hex-inspector/).
2. Paste a Protobuf HEX payload.
3. Select **Decode** or press `Ctrl/Cmd + Enter`.
4. Inspect the JSON output.
5. Use the field inspector to examine wire type, raw bytes and interpretations.
6. Copy or save the decoded JSON when needed.

## Privacy

The decoder processes payloads in the browser and does not require a project backend, database or upload endpoint.

For highly sensitive payloads, run the repository locally rather than relying on a hosted page.

## Limitations

- No `.proto` schema awareness
- Field names cannot be recovered without schema information
- Length-delimited data is inherently ambiguous between text, nested messages and arbitrary bytes
- Groups (wire types `3` and `4`) are intentionally unsupported
- Numeric interpretations cannot establish the original application-level type

This is a **wire-level inspection/debugging utility**, not a replacement for a schema-aware generated Protobuf parser.

## Project structure

```text
.
├── assets/
│   ├── protobuf-overview.jpg
│   └── protobuf-decoded-output.jpg
├── index.html
├── README.md
└── LICENSE
```

## Tech stack

- HTML
- CSS
- Vanilla JavaScript
- JavaScript `BigInt`
- `TextDecoder`
- `DataView`
- Clipboard API
- Browser File / Blob APIs

No framework or build system is required.

## License

MIT License. See `LICENSE` for details.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

# Protobuf Hex Inspector

A lightweight, client-side tool for inspecting **schema-less Protocol Buffers (Protobuf) HEX payloads** when the original `.proto` schema is unavailable.

**Live demo:** https://mohith-krishnaa.github.io/protobuf-hex-inspector/

## What it does

- Accepts raw Protobuf HEX input
- Validates HEX before decoding
- Parses Protobuf field keys and wire types
- Reads 64-bit-safe varints using JavaScript `BigInt`
- Handles length-delimited fields
- Detects printable UTF-8 text
- Recursively inspects nested length-delimited messages
- Decodes fixed64 and fixed32 fields as raw hexadecimal bytes
- Groups repeated fields and simplifies single-value fields
- Displays decoded data as formatted JSON
- Copies decoded output to the clipboard
- Runs entirely in the browser

## How it works

```text
HEX input
   ↓
HEX → bytes
   ↓
Protobuf field key
   ↓
field number + wire type
   ↓
value decoding
   ↓
JSON output
```

The implementation is intentionally schema-less, so field numbers are shown as numeric keys rather than meaningful `.proto` field names.

## Current decoder scope

The current implementation handles:

| Wire type | Meaning | Support |
|---:|---|---|
| `0` | Varint | ✅ |
| `1` | 64-bit | ✅ Raw HEX |
| `2` | Length-delimited | ✅ |
| `3` | Start group | ❌ Explicitly unsupported |
| `4` | End group | ❌ Explicitly unsupported |
| `5` | 32-bit | ✅ Raw HEX |

For wire type `2`, the decoder first attempts strict UTF-8 detection. If the value is not printable text, it attempts recursive message decoding and falls back to raw HEX when the payload cannot be interpreted as a nested message.

## Usage

1. Open the live demo or `index.html` locally.
2. Paste a Protobuf HEX payload into the input box.
3. Select **Decode**.
4. Copy the resulting JSON if needed.

## Privacy

The decoder itself processes payloads in the browser and does not require a project backend, file upload, or application database. The hosted page can still load through normal browser/network infrastructure, so sensitive payloads should be inspected locally if privacy is important.

## Limitations

Schema-less decoding cannot recover the original field names, schema semantics, or application-specific meaning. Length-delimited binary values can also be ambiguous between text, nested messages, and arbitrary bytes.

The tool does not currently interpret fields using a `.proto` schema, and fixed32/fixed64 values are exposed as raw hexadecimal rather than being converted into application-specific numeric types.

This is an inspection/debugging utility, not a full replacement for a generated Protobuf parser with the original schema.

## Project structure

```text
.
├── index.html
├── README.md
└── LICENSE
```

## Tech stack

- HTML
- CSS
- Vanilla JavaScript
- JavaScript `BigInt`
- Browser `TextDecoder`
- Clipboard API

No framework or build system is required.

## License

MIT License. See `LICENSE` for details.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

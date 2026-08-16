# Protobuf Hex Inspector

A lightweight, client-side tool for inspecting **schema-less Protocol Buffers (Protobuf) HEX payloads** when the original `.proto` schema is unavailable.

**Live demo:** https://mohith-krishnaa.github.io/protobuf-hex-inspector/

## What it does

- Accepts raw Protobuf HEX input
- Parses Protobuf field keys and wire types currently implemented by the decoder
- Reads varints
- Handles length-delimited fields
- Attempts UTF-8 text detection
- Recursively inspects length-delimited data as nested messages
- Groups repeated fields and simplifies single-value fields
- Displays decoded data as formatted JSON
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

The current implementation is intentionally schema-less, so field numbers are shown as numeric keys rather than meaningful `.proto` field names.

## Current decoder scope

The current implementation explicitly handles:

- Wire type `0` — varint
- Wire type `2` — length-delimited values
- Printable UTF-8 detection for text values
- Recursive inspection of nested length-delimited payloads

Other Protobuf wire types are not currently decoded by the implementation and may cause a payload to be reported as unsupported.

## Usage

1. Open the live demo or `index.html` locally.
2. Paste a Protobuf HEX payload into the input box.
3. Select **Decode**.
4. Copy the resulting JSON if needed.

## Privacy

The application is client-side only. Payloads are processed in the browser; the project does not require a backend, file upload, or application database.

Do not treat this as a guarantee that a hosted browser environment is private if you use it with sensitive data. For sensitive payloads, review the deployed source and run the application locally.

## Limitations

Schema-less decoding cannot recover the original field names, schema semantics, or application-specific meaning. Binary length-delimited values can also be ambiguous between text, nested messages, and arbitrary bytes.

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
- Browser `TextDecoder` / Clipboard APIs

No framework or build system is required.

## License

MIT License. See `LICENSE` for details.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

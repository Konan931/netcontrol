# Repository structure

NetControl is a small, dependency-free Python command-line tool for Linux network diagnostics. The original code was migrated out of the `Konan931/Konan931` GitHub profile repository.

```text
netcontrol/
├── LICENSE                         MIT license
├── README.md                       Usage, requirements, safety and tests
├── structure.md                    Repository map and extension notes
├── .gitignore                      Local environment and generated files
└── scripts/
    ├── netcontrol.py               CLI entry point and diagnostics
    ├── netcontrol.toml.example     Optional, non-secret configuration example
    └── tests/
        └── test_netcontrol.py      Python unittest coverage
```

## Runtime and supported platforms

- Python 3.11+ is recommended for optional TOML configuration (`tomllib`).
- Interface, routing and socket diagnostics read Linux `/proc` and `/sys` pseudo-filesystems.
- DNS and TCP probes can run on other operating systems where Python networking is available.
- The `ping` subcommand delegates to the installed system `ping` program; platform behavior may vary.
- The CLI does not modify network configuration, routes or firewall settings.

## Command anatomy

`scripts/netcontrol.py` contains:

- `interfaces`, `routes`, `connections`: local Linux diagnostics.
- `dns_lookup`, `tcp_check`, `ping`: explicit host checks.
- `snapshot`, `watch`: current status and successive interface-counter samples.
- `load_config`: optional TOML defaults.
- `render`, `parser`, `main`: output, command parsing and dispatch.

Use `--json` before a subcommand to request machine-readable output.

## Development and verification

```bash
python3 -m py_compile scripts/netcontrol.py
python3 -m unittest discover -s scripts/tests -v
python3 scripts/netcontrol.py --help
python3 scripts/netcontrol.py --json interfaces
```

Tests cover proc-file parsing, IPv4 route decoding, loopback TCP checks, connection summaries and invalid-port handling. Running tests may open a short-lived local listener on `127.0.0.1`.

## Extension conventions

- Keep the diagnostic core importable and dependency-light.
- Add test fixtures for new proc/sys parsers and unusual input conditions.
- Validate network targets and bounded timeouts; avoid implicit scans.
- Document user-visible output and exit-code changes.
- Do not add persistent monitoring, network mutation or background services without explicit scope and security review.

## Migration note

The source lived under `tools/netcontrol/` in the profile repository. Its independent repository retains the author's `scripts/` directory and existing commit history. Changes here should not require edits to the GitHub profile README except for links or brief project descriptions.

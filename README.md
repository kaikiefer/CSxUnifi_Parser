# UniFi CEF parser for CrowdStrike Falcon Next-Gen SIEM

Independent custom parser and candidate searches for UniFi Network, Protect and OS CEF exports. Not an official CrowdStrike or Ubiquiti product. Targets CPS 1.2.0 and ECS 9.5.0.

## Contents
- Import parsers/ubiquiti-unifi.yaml, or use parsers/ubiquiti-unifi.cql in the parser editor.
- searches/ contains 13 candidate rules and three investigation queries. Review searches/README.md and detection-notes.md before deployment.
- tests/fixtures.json contains 30 entirely generated synthetic examples. No captured user logs are included. Addresses use RFC 5737/3849 documentation ranges, hosts use .example, and the test MAC is locally administered.
- scripts/build.py regenerates the import YAML from CQL and fixtures.

## Installation
Create a custom parser named ubiquiti-unifi in your Falcon/LogScale tenant, import the YAML and run its testCases before production ingestion. CQL compilation and tenant execution are NOT validated by the offline tests. Forward one raw CEF/syslog event per input; JSON wrappers and raw daemon logs require separate parsing.

When using script-only import, tag: Cps.version, Vendor, ecs.version, event.dataset, event.kind, event.module, event.outcome, observer.type (without a leading #).
Configure your collector separately. For yearless local timestamps provide the actual source timezone via @timezone rather than guessing.

## Validation
Python is a development dependency, not part of tenant ingestion.

    python3 -m venv .venv
    .venv/bin/python -m pip install -r requirements-dev.txt
    .venv/bin/python scripts/build.py
    .venv/bin/python scripts/build.py --check
    .venv/bin/python -m unittest discover -s tests -v

For schema validation set UNIFI_PARSER_SCHEMA to a local copy of the official https://schemas.humio.com/parser/v0.3.0 schema. Without it that test explicitly skips. The schema is not bundled.

Tests check export schema (when supplied), YAML/source consistency, fixture structure and selected regex extraction only. They do not emulate parseCEF or the full CQL engine. Validate firmware-specific exports in your tenant.

## Fixtures and licensing

All example events are synthetic. No captured logs are included.
No license is granted.

## References
- https://developer.crowdstrike.com/ngsiem/cps/compliant-parsers/
- https://developer.crowdstrike.com/ngsiem/cps/parser-guidelines/
- https://schemas.humio.com/parser/v0.3.0
- https://help.ui.com/hc/en-us/articles/33349041044119-UniFi-System-Logs-SIEM-Integration

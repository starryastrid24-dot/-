# LxBox rules and DNS artifacts

This repository contains public match-only sing-box SRS/JSON extracted from the user's authorized Tianyue rules. No proxy nodes, subscription URLs, passwords, UUIDs, API tokens, or device backups belong here. Never copy the original subscription YAML into this repository.

## Interfaces and provenance

`route-manifest.json` schema 1 defines ordered action segments, category, source line numbers and SHA256 of every JSON/SRS. SRS contains predicates only; outbound and DNS selection must be configured in LxBox. Preserve cross-action ordering. Same-action grouping does not prove that changing proxy direction or DNS has identical effects.

Files are served by GitHub raw HTTPS at the committed branch or immutable commit. LxBox v2.25.18 downloads them into its internal cache; posting a rule without checking cache and running config is not acceptance. This repository runs no service, automation or Node process. Builds were performed with official sing-box-lx 1.14.4-lx.25; current build tools and protected evidence remain in Personal Archive's task pa-8pi, not in this public repository.

`dns-split.json` and `dns-original.json`, when present, are storage-1.0 user DNS records with identical managed tags. Apply by merging their managed records into the current DNS list, retaining unrelated records; the API replaces the whole list. The split profile sends foreign DNS through corresponding enabled directions. Original uses the source's Ali/Tencent resolvers directly. Direction tags are references, never node credentials. Inspect and adapt direction references before reuse on a different installation.

## Validation and maintenance

Before publishing, check JSON key allowlists, source mapping, hashes and absence of credentials. Compile SRS offline with the stated core, then verify device download, generated/running config and real DNS/route paths. Anthropic rules are static-only: do not actively test Anthropic domains without new explicit authorization.

GEOIP CN uses the upstream URL documented in README; LAN and final MATCH require explicit application rules. The source's no-resolve flags remain in the manifest because headless SRS cannot encode them; do not claim exact equivalence without reviewing resolution behavior. Desktop process names do not identify Android packages.

On failure, retain the last accepted commit/config and report the failing stage. Do not force-push or delete evidence. Repeated downloads can replace application caches but must retain rule IDs; avoid duplicate rule creation. No local daemon or temporary runtime file is created by merely using this repository. Close network/file handles after manual tooling; preserve published hashes and validation records. Work remains recorded in the original Beads task, with real device checks distinguished from offline compilation.

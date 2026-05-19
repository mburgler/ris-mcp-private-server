# ris-mcp-standalone

A Claude Code marketplace shipping the **RIS Legal** plugin — search and
citation over the Austrian **Rechtsinformationssystem** (RIS): federal, state,
district and municipal law, gazettes (Bundesgesetzblatt, Landesgesetzblätter),
and case law from the **VfGH, VwGH, OGH, BVwG, LVwG**, Datenschutzbehörde, and
a dozen further specialised tribunals.

Read-only. Public data, CC BY 4.0 (Bundeskanzleramt, Republik Österreich). No
write tools, no irreversible operations.

## Install

In Claude Code:

```text
/plugin marketplace add https://github.com/mburgler/ris-mcp-standalone
/plugin install ris-legal@ris-mcp-standalone
```

That registers the marketplace and installs the `ris-legal` plugin. The plugin
ships a pre-wired MCP connector pointing at the hosted server at
`https://rismcp.mburgler.com/mcp` — no local server, no API key.

## What you get

Eight tools and one skill:

| Tool | Purpose |
|---|---|
| `ris_about` | Server identity, sources, license, disclaimer |
| `ris_list_scopes` | Valid enum values for a field (Bundesland, Fachgebiet, …) |
| `ris_search` | Universal search across 38 RIS applications, eight corpora |
| `ris_get_document` | Full metadata for one record |
| `ris_get_section` | Full text of one paragraph / article / Rechtssatz |
| `ris_get_related` | Amendments, predecessors, cited norms, citing decisions |
| `ris_verify` | In-force check on a date against consolidated law |
| `ris_get_history` | Change feed for an Anwendung between two dates |

The bundled `ris-research` skill teaches the agent verbatim-citation policy,
authentic-vs-informational handling, inline tag vocabulary, and prompt-injection
defence over retrieved content.

Every response carries a `ris_citation` (RIS internal id + ELI or
Geschäftszahl + `dokument_url` + `retrieved_at`) and an `is_authentic` flag —
`true` only for applications that carry legal force (the gazette and a handful
of authentic registers), `false` for consolidated text (`BrKons`, `LrKons`) and
informational records.

## Sanity check

```bash
curl https://rismcp.mburgler.com/health
# → { "status": "ok", "ris_reachable": true, ... }
```

## What's inside this repo

```
.claude-plugin/
  marketplace.json        # marketplace manifest
ris-legal/                # the plugin itself
  .claude-plugin/plugin.json
  .mcp.json               # connector configuration → hosted endpoint
  README.md               # plugin-level docs
  skills/ris-research/
    SKILL.md              # the agent skill
LICENSE                   # Apache-2.0 (plugin + skill content)
README.md                 # this file
```

The MCP server code (the thing that runs at `rismcp.mburgler.com`) is
maintained separately and is not part of this repository.

## License

- **Plugin / skill content (this repo):** Apache-2.0 — see [`LICENSE`](./LICENSE).
- **RIS data delivered by the server:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.de) — credit *Bundeskanzleramt, Republik Österreich*.

## Disclaimer

Only specific RIS applications carry legal force. Consolidated text is
informational only — always cite the gazette for legally binding propositions.
**Not legal advice.**

## Contact

- Plugin maintainer: Matteo Bürgler · matteo.buergler@gmail.com
- Upstream support / mass-download coordination: `ris.it@bka.gv.at` (Bundeskanzleramt Abt. VII/6 — E-Government Bund/Verwaltung)

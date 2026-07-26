# JARVIS Module Feed

**Generiertes Repository — bitte nicht von Hand bearbeiten.**

Dies ist der öffentliche Auslieferungspunkt der
[JARVIS Module Registry](https://github.com/HeavyG2005/JARVIS-Module-Registry).
Alles hier wird von deren Publish-Workflow erzeugt; Änderungen von Hand werden beim
nächsten Lauf überschrieben.

## Inhalt

| Pfad | Was |
|---|---|
| `index.json` | Der signierte Feed nach Vertrag v1 §2 |
| `keys/registry-public.ed25519` | Öffentlicher Signierschlüssel (base64, 32 Byte roh) |
| Releases | Je ein Release `{id}-{version}` mit dem Modul-ZIP, `.sha256` und `.sig` |

## Verifikation

Der Feed ist Ed25519-signiert. Die Top-Level-`signature` deckt die JCS-kanonisierte
Serialisierung (RFC 8785) des `modules`-Blocks ab; jede Version trägt zusätzlich eine
Signatur über den kanonischen String `"{id}\n{version}\n{sha256}"`.

Von Hand prüfbar mit dem Tooling der Registry:

```bash
pip install -e ./tools/publish          # im Registry-Repo
curl -sSLO https://raw.githubusercontent.com/HeavyG2005/JARVIS-Module-Feed/master/index.json
jarvis-registry verify --index index.json --public-key keys/registry-public.ed25519
```

## Was hier *nicht* liegt

Kein Quellcode — weder der von Modulen noch der des Tooling. Modul-Pakete sind gebaute
Artefakte (`manifest.json` + Binaries); ihr Quellcode liegt bei den jeweiligen Einreichern.

# JARVIS Module Feed

Der öffentliche Auslieferungspunkt der
[JARVIS Module Registry](https://github.com/HeavyG2005/JARVIS-Module-Registry) — **und der Ort,
an dem Module eingereicht werden**.

> **Module einreichen: [CONTRIBUTING.md](CONTRIBUTING.md)** — ein PR mit einer einzigen Datei
> unter `submissions/`.

`index.json`, `keys/` und die Releases sind **generiert**: sie entstehen im Publish-Workflow der
Registry, Änderungen von Hand daran werden beim nächsten Lauf überschrieben. Von Hand bearbeitet
wird hier ausschließlich `submissions/`.

## Stabile URLs

- Feed: `https://heavyg2005.github.io/JARVIS-Module-Feed/index.json`
- Public Key: `https://heavyg2005.github.io/JARVIS-Module-Feed/keys/registry-public.ed25519`

Diese URLs sind der Bezugspunkt für den JCC-Hub. Pages liefert vom Branch `main`.

## Inhalt

| Pfad | Was | Erzeugt von |
|---|---|---|
| `submissions/{id}/{version}.json` | Einreichungen — URL + Hash je Modulversion | **Menschen**, per PR |
| `index.json` | Der signierte Feed nach Vertrag v1 §2 | Publish-Workflow |
| `keys/registry-public.ed25519` | Öffentlicher Signierschlüssel (base64, 32 Byte roh) | Publish-Workflow |
| Releases `{id}-{version}` | Modul-ZIP, `.sha256` und `.sig` | Publish-Workflow |
| Release `tooling-v*` | Rad-Datei des Publish-Tooling, damit die CI hier prüfen kann | Publish-Workflow |

## Verifikation

Der Feed ist Ed25519-signiert. Die Top-Level-`signature` deckt die JCS-kanonisierte
Serialisierung (RFC 8785) des `modules`-Blocks ab; jede Version trägt zusätzlich eine
Signatur über den kanonischen String `"{id}\n{version}\n{sha256}"`.

Von Hand prüfbar — ohne Zugriff auf das Registry-Repository, das Tooling liegt hier als
Rad-Datei bereit:

```bash
pip install https://github.com/HeavyG2005/JARVIS-Module-Feed/releases/download/tooling-v0.1.0/jarvis_registry-0.1.0-py3-none-any.whl
curl -sSLO https://heavyg2005.github.io/JARVIS-Module-Feed/index.json
jarvis-registry verify --index index.json --public-key keys/registry-public.ed25519
```

## Was hier *nicht* liegt

Kein Quellcode — weder der von Modulen noch der des Tooling. Modul-Pakete sind gebaute
Artefakte (`manifest.json` + Binaries); ihr Quellcode liegt bei den jeweiligen Einreichern.
Der Signierschlüssel liegt im privaten Registry-Repo und verlässt es nie: die Registry signiert
auch fremde Module, ihr Schlüssel ist die Trust-Wurzel des Systems.

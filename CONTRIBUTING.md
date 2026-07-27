# Ein Modul einreichen

Eine Einreichung ist ein Pull Request mit **einer einzigen neuen Datei**:

```
submissions/{id}/{version}.json
```

```json
{
  "id": "jarvis.hue",
  "version": "1.2.0",
  "sourceUrl": "https://example.com/hue-1.2.0.zip",
  "sha256": "<sha256 des ZIP, lowercase hex>",
  "yanked": false
}
```

Das war die ganze Datei. Anzeigename, Icon, `coreApi` und `discoverySignatures` stehen bewusst
**nicht** darin — die liest die Registry aus der `manifest.json` in deinem ZIP. So gibt es genau
eine Wahrheit und keine Drift zwischen Einreichung und Paket.

## Was mit deinem PR passiert

1. **`validate-submissions`** lädt dein Paket von `sourceUrl`, prüft den `sha256` und liest das
   Manifest. Stimmt etwas nicht, steht der Grund im CI-Log.
2. **`submission-scope`** prüft, dass dein PR ausschließlich `submissions/` anfasst — siehe unten.
3. **Ein Mensch schaut drauf.** Das ist kein Formalismus, sondern die Sicherheitsgrenze des
   Systems (siehe „Warum der Review zählt").
4. Nach dem Merge veröffentlicht die Registry innerhalb einer Stunde: sie signiert dein Paket,
   legt es als Release-Asset an und schreibt `index.json` neu. Ab dann sieht jeder JCC-Hub es.

## Dein PR darf nur `submissions/` ändern

Ändert er daneben noch etwas — die CI, den Feed, den Public Key —, schlägt `submission-scope`
fehl. Das ist kein Misstrauen gegen dich persönlich: eine Einreichung ist ein winziges
JSON-Diff, und genau daneben würde eine Änderung an der Prüfmechanik am wenigsten auffallen.
Willst du etwas am Repo selbst verbessern, mach dafür bitte einen eigenen PR auf — der ist
willkommen, er ist nur kein Einreichungs-PR.

## Warum der Review zählt

Die Registry ist alleiniger Herausgeber und signiert **auch fremde Module** mit ihrem Schlüssel.
Ein Modul, das im Manifest `trust: trusted` deklariert, wird durch diese Signatur im Hub
tatsächlich Trusted — es bekommt also echte Rechte auf dem Gerät des Nutzers. Der Merge deines
PRs ist der Moment, in dem diese Zusage gemacht wird. Deshalb wird jede Einreichung von Hand
angesehen, und deshalb kann es dauern.

Was **nicht** passiert: dein Quellcode wird nicht verlangt und nicht angefasst. Die Einreichung
referenziert nur eine URL und einen Hash; die Registry sieht ausschließlich das gebaute ZIP.
Eine automatische Prüfung der Binaries (Malware-Scan, reproduzierbare Builds) gibt es bewusst
noch nicht — der menschliche Review ist derzeit der einzige inhaltliche Filter.

## Vorher selbst prüfen

Das Tooling der Registry liegt hier als Rad-Datei bereit, du brauchst keinen Zugriff auf das
Registry-Repository:

```bash
pip install https://github.com/HeavyG2005/JARVIS-Module-Feed/releases/download/tooling-v0.1.0/jarvis_registry-0.1.0-py3-none-any.whl

# sha256 deines Pakets bestimmen
jarvis-registry pack ./mein-modul --out mein-modul.zip

# Einreichung gegen den veroeffentlichten Feed pruefen — genau das tut auch die CI
jarvis-registry validate --submissions submissions --index index.json
```

## Ein Modul zurückziehen

`"yanked": true` in der betreffenden Datei setzen und mergen lassen. Die Version verschwindet
nicht aus dem Feed — Hubs, die sie installiert haben, sollen sie weiter verifizieren können —,
wird aber nicht mehr als installierbar angeboten.

## Versionen

SemVer. Eine bereits veröffentlichte `{id}/{version}` ist unveränderlich: ein geänderter
`sha256` auf einer publizierten Version wird von `validate-submissions` abgelehnt. Neuer Inhalt
heißt neue Version.

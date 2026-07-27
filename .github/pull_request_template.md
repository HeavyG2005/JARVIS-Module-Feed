<!--
Reichst du ein Modul ein? Dann sollte dieser PR genau eine neue Datei enthalten:
submissions/{id}/{version}.json — und sonst nichts. Details: CONTRIBUTING.md

Verbesserst du etwas am Repo selbst? Dann loesch diese Vorlage einfach raus und
beschreib dein Anliegen normal.
-->

## Modul

- **id:**
- **version:**
- **Was das Modul tut:**

## Herkunft

- **Quellcode:** <!-- URL, falls oeffentlich — freiwillig, wird nicht vorausgesetzt -->
- **Wer baut das Paket:** <!-- du selbst, eine CI, ein Release-Workflow? -->

## Selbst geprüft

- [ ] `sourceUrl` ist dauerhaft erreichbar und liefert genau das ZIP, dessen `sha256` hier steht
- [ ] `manifest.json` im ZIP nennt dieselbe `id` und `version` wie diese Einreichung
- [ ] `jarvis-registry validate` lief lokal durch (siehe CONTRIBUTING.md)
- [ ] Dieser PR ändert **ausschliesslich** `submissions/`

## Zum Trust-Level

<!--
Deklariert dein manifest.json `trust: trusted`? Dann begruende hier bitte, warum das Modul
erhoehte Rechte auf dem Geraet des Nutzers braucht. Die Signatur der Registry macht diese
Deklaration wirksam — das ist die Entscheidung, die im Review getroffen wird.
-->

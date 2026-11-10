# Fahrplan – als App aufs Handy bringen

Die App besteht aus diesen Dateien. Sie müssen immer zusammen bleiben:

```
index.html            die App selbst
manifest.webmanifest  Name, Farben und Symbol der App
sw.js                 sorgt dafür, dass die App offline läuft
icons/                App-Symbole
```

## 1. Einmal hochladen (kostenlos)

Handys installieren eine Web-App nur von einer https-Adresse. Die Dateien müssen also einmal ins Internet.
Danach läuft die App offline, und alle Einträge bleiben auf dem jeweiligen Handy.

**Variante A: Netlify Drop (am einfachsten)**
1. Im Browser am PC https://app.netlify.com/drop öffnen.
2. Kostenloses Konto anlegen bzw. anmelden.
3. Den ganzen Ordner `Fahrgemeinschaft` in das Feld ziehen.
4. Du bekommst eine Adresse wie `https://irgendwas.netlify.app`. Die kannst du in den Site-Einstellungen umbenennen.

**Variante B: GitHub Pages**
1. Auf github.com ein neues öffentliches Repository anlegen, z. B. `fahrplan`.
2. Alle Dateien hochladen („Add file“ → „Upload files“, den Ordner `icons` mit hochladen).
3. Unter Settings → Pages bei „Branch“ `main` auswählen und speichern.
4. Nach etwa einer Minute ist die App unter `https://DEINNAME.github.io/fahrplan/` erreichbar.

## 2. Auf dem Handy installieren

**Android (Chrome):** Adresse öffnen → Menü ⋮ → „App installieren“ bzw. „Zum Startbildschirm hinzufügen“.

**iPhone (Safari):** Adresse in Safari öffnen → Teilen-Symbol (Quadrat mit Pfeil) → „Zum Home-Bildschirm“.

Danach hat jedes Handy ein eigenes „Fahrplan“-Symbol. Die App öffnet sich im Vollbild und braucht kein Internet mehr.

## Wichtig zu wissen

- **Jedes Handy hat seinen eigenen Plan.** Trägt Anna auf ihrem Handy „krank“ ein, sieht Mehmet das nicht automatisch.
- **Updates:** Wenn du später etwas an der App änderst, lädst du die Dateien erneut hoch und erhöhst in `sw.js` die Zahl bei `VERSION` (z. B. `fahrplan-v2`). Die Handys holen sich die neue Version, sobald sie wieder online sind.
- Die Einträge sind im Browser-Speicher des Handys. Wenn man im Browser „Websitedaten löschen“ wählt, sind sie weg.

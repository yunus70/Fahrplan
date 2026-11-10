# Fahrplan – Anleitung

## Was ist was?

```
index.html            die App selbst (Aussehen, Kalender, Logik)
firebase-config.js    Zugangsdaten für das gemeinsame Speichern
sw.js                 Service Worker: hält die App offline verfügbar
manifest.webmanifest  Name, Farben und Symbol der App
icons/                App-Symbole
```

Die App liegt bei **GitHub Pages**. Die gemeinsamen Einträge liegen bei **Google Firebase**.
Du brauchst beides.

---

## Teil 1: Gemeinsam speichern mit Firebase (einmalig, ca. 10 Minuten)

1. **Projekt anlegen**
   https://console.firebase.google.com öffnen, mit deinem Google-Konto anmelden →
   „Projekt erstellen“ → Name z. B. `fahrplan` → Google Analytics **ausschalten** → erstellen.

2. **Anonyme Anmeldung einschalten**
   Links „Build“ → **Authentication** → „Jetzt starten“ → Reiter „Anmeldemethode“ →
   **Anonym** → aktivieren → speichern.
   (Niemand muss sich mit Namen oder Passwort anmelden. Jedes Handy bekommt automatisch eine unsichtbare Kennung.)

3. **Erlaubte Adresse eintragen**
   Immer noch in Authentication → Reiter „Einstellungen“ → **Autorisierte Domains** →
   „Domain hinzufügen“ → `DEINNAME.github.io` (deine GitHub-Pages-Adresse ohne `https://` und ohne `/fahrplan`).

4. **Datenbank anlegen**
   Links „Build“ → **Firestore Database** → „Datenbank erstellen“ →
   Standort **europe-west3 (Frankfurt)** → „Im Produktionsmodus starten“ → erstellen.

5. **Zugriffsregeln setzen**
   In Firestore den Reiter **Regeln** öffnen, alles darin löschen, diesen Text einfügen und auf **Veröffentlichen** klicken:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /gruppen/{gruppe} {
         allow get, create, update: if request.auth != null && gruppe.size() >= 16;
         allow list, delete: if false;
       }
     }
   }
   ```

   Das heißt: Wer den geheimen Gruppen-Code aus dem Einladungslink kennt, kann den Plan lesen und eintragen.
   Niemand kann sich eine Liste aller Gruppen anzeigen lassen oder Gruppen löschen.

6. **Zugangsdaten holen**
   Oben links das Zahnrad → **Projekteinstellungen** → ganz unten „Meine Apps“ → Symbol **`</>`** (Web) →
   Name `Fahrplan` → „App registrieren“ (Firebase Hosting **nicht** anhaken).
   Es erscheint ein Block wie dieser:

   ```
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "fahrplan-xxxx.firebaseapp.com",
     projectId: "fahrplan-xxxx",
     ...
   };
   ```

   Diesen Block an Claude schicken, oder selbst in `firebase-config.js` eintragen:
   `window.FAHRPLAN_FIREBASE = null;` ersetzen durch `window.FAHRPLAN_FIREBASE = { ...die Werte... };`
   Diese Werte sind kein Passwort. Sie dürfen öffentlich auf GitHub liegen, der Schutz kommt aus den Regeln in Schritt 5.

---

## Teil 2: Auf GitHub hochladen

1. In deinem Repository auf **Add file → Upload files**.
2. Den **Inhalt** des Ordners `Fahrgemeinschaft` hineinziehen (alle Dateien und den Ordner `icons`).
   Vorhandene Dateien werden dabei ersetzt.
3. **Commit changes** klicken. Nach ein bis zwei Minuten ist die neue Version online.

**Bei jeder späteren Änderung:** in `sw.js` die Zahl bei `VERSION` hochzählen (`fahrplan-v3`, `-v4` …), dann erneut hochladen.
So holen sich die Handys die neue Version.

---

## Teil 3: Benutzen

1. **Du** öffnest die App und legst den Plan an. Hast du schon einen Plan auf dem Handy, gehst du auf „Gruppe“ → **„Plan mit anderen teilen“**.
2. Unter **Gruppe → Einladungslink** auf „Link teilen“ tippen und ihn per WhatsApp o. Ä. an deine Mitfahrer schicken.
3. Die Mitfahrer öffnen den Link und installieren die App:
   - **Android:** Browser-Menü → „App installieren“ / „Zum Startbildschirm hinzufügen“
     (Chrome, Samsung Internet, Firefox, Edge … alle gehen)
   - **iPhone:** Teilen-Symbol □↑ → „Zum Home-Bildschirm“
4. Ab jetzt sieht jeder sofort, was die anderen eintragen.
   **Ohne Internet** kann man trotzdem eintragen. Die Einträge werden abgeglichen, sobald das Handy wieder online ist.

Unten in der App steht immer, wie der Stand ist:
- „Synchronisiert“: alles abgeglichen
- „Offline gespeichert · wird abgeglichen …“: Eintrag wartet auf Internet
- „Nur auf diesem Gerät gespeichert“: noch keine Gruppe, der Plan ist nicht geteilt

**Wichtig:** Der Einladungslink ist der Schlüssel zu eurem Plan. Teile ihn nur mit deinen Mitfahrern.

# Techno Bavaria – iOS-App (Hülle um techno-bavaria.de)

Native iOS-App (Capacitor), die https://techno-bavaria.de lädt. Alle Änderungen an der Website sind sofort in der App,
ohne neue Apple-Freigabe. Gebaut und zu TestFlight hochgeladen wird per GitHub Actions auf einem Cloud-Mac (kein eigener Mac nötig).

## Einmalige Einrichtung

1. **Bundle-ID der alten App:** `de.n8tixx.app` (ist als Standard eingetragen; nur über die Variable `IOS_BUNDLE_ID` änderbar).
2. **Team-ID**: developer.apple.com → Account → Mitgliedschaft → „Team-ID".
3. **API-Schlüssel**: App Store Connect → Benutzer und Zugriff → Integrationen → App-Store-Connect-API → Team-Schlüssel → „+".
   Name z. B. „GitHub Build", Zugriff **App-Manager** (oder Admin). Die `.p8`-Datei lässt sich nur EINMAL laden. Notiere Schlüssel-ID und Issuer-ID.
4. **GitHub** → Repo Rhino87x/technobavaria → Settings → Secrets and variables → Actions:
   - Secrets: `APP_STORE_CONNECT_KEY_ID`, `APP_STORE_CONNECT_ISSUER_ID`, `APP_STORE_CONNECT_KEY_P8` (kompletter Inhalt der .p8-Datei), `APPLE_TEAM_ID`
   - Variables: `IOS_BUNDLE_ID` (Bundle-ID der alten App), optional `IOS_MARKETING_VERSION` (Standard 2.0; muss höher sein als die zuletzt veröffentlichte Version)
   - Werte ohne Leerzeichen am Anfang/Ende einfügen.

## Build starten

GitHub → Actions → „iOS → TestFlight" → Run workflow. Nach ca. 10–20 Minuten erscheint der Build in App Store Connect → TestFlight
(nach Apples automatischer Verarbeitung, ca. 10 Minuten).

## App umbenennen

App Store Connect → App → App-Informationen → Name: „Techno Bavaria". Dazu Beschreibung, Screenshots, Datenschutz-URL
(https://techno-bavaria.de/legal/privacy_policy), Support-URL, Altersfreigabe und „App-Datenschutz" anpassen.

## Hinweise

- Google-/Facebook-Login sind in der App ausgeblendet (Google blockiert Logins in eingebetteten Browsern; Apple verlangt dann „Mit Apple anmelden"). In der App gilt E-Mail-Login.
- Der Browser-User-Agent enthält `TechnoBavariaApp`; die Website erkennt daran die App.
- Konto-Löschung: Apple verlangt sie in der App. Aktuell läuft sie über Datenschutz-Center → Anfrage.

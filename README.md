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
- Konto-Löschung: direkt in der App unter Profil → „Datenschutz & Konto löschen“ (sofortige Löschung mit Bestätigung „LÖSCHEN“).
- Social-Logins werden nur angezeigt, wenn der Anbieter in Supabase aktiviert ist (aktuell keiner aktiv → nur E-Mail-Login).
- App-Icon: `ios/App/App/Assets.xcassets/AppIcon.appiconset/AppIcon-512@2x.png` (1024×1024, ohne Transparenz; Quelle: technobavaria/mobile/resources/app-icon-1024.png).

- Spracheingabe (KI-Berater): native iOS-Spracherkennung über `@capacitor-community/speech-recognition`; die Website nutzt sie automatisch, wenn sie in der App läuft. Fragt beim ersten Tippen auf das Mikrofon nach Mikrofon- und Spracherkennungs-Erlaubnis.

---

# Techno Bavaria – Android-App (Google Play)

Gleiche Hülle wie iOS (Capacitor, lädt https://techno-bavaria.de), Projekt im Ordner `android/`.
Paketname (dauerhaft, nicht mehr änderbar nach dem ersten Upload): **`de.technobavaria.app`**. Ziel-API 36.

## Einmalige Einrichtung

1. **Google Play Console** (play.google.com/console) als **Organisation** registrieren (einmalig 25 US-$).
   Für Organisationen verlangt Google eine **D-U-N-S-Nummer** der Universal Event GmbH (kostenlos bei Dun & Bradstreet, dauert einige Tage).
   Vorteil Organisation: kein Pflicht-Test mit 12 Testern über 14 Tage (gilt nur für private Konten).
2. **GitHub** → Repo Rhino87x/technobavaria-ios → Settings → Secrets and variables → Actions → Secret
   `ANDROID_KEYSTORE_PASSWORD` = selbst ausgedachtes, langes Passwort (gut aufbewahren!).
3. **Erster Build**: Actions → „Android → Google Play (AAB)" → Run workflow.
   Beim ersten Lauf wird ein **Upload-Schlüssel** erzeugt (Artefakt „upload-schluessel", 3 Tage verfügbar):
   herunterladen, sicher speichern und den Inhalt von `upload-keystore.base64.txt` als Secret `ANDROID_KEYSTORE_BASE64` eintragen.
   Ohne diesen Schritt entsteht beim nächsten Lauf ein anderer Schlüssel und Google lehnt Updates ab.
4. **Play Console** → App erstellen („Techno Bavaria", Deutsch, App, kostenlos) → Release → Produktion (oder zuerst „Interner Test")
   → Artefakt „techno-bavaria-aab" (`app-release.aab`) hochladen. „Play App Signing" aktiviert lassen.

## Updates

Workflow erneut starten → neues AAB hochladen. Die Versionsnummer (versionCode) zählt automatisch hoch.
Inhaltliche Änderungen an der Website brauchen kein App-Update.

## Hinweise

- Push-Benachrichtigungen sind auf Android noch aus (brauchen Firebase Cloud Messaging; später nachrüstbar).
- Google-/Facebook-Login bleiben in der App ausgeblendet (Google blockiert Logins in eingebetteten WebViews).
- Store-Grafiken (Icon 512×512, Feature-Grafik 1024×500, Screenshots 1080×1920) werden separat bereitgestellt.

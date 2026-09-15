# PadelLive – Entwicklungshistorie (v1.2.0 → v2.23.0)

Stand: 2026-09-15. Zusammengestellt aus `AGENTS.md`, den Versionskommentaren in
`MainActivity.java`, den Claude-Projektdokumenten ("Padel-Live Courtfinder"),
`review/REVIEW_TODO_v2.11.0.md`, den IONOS-`README.txt`-Ständen der archivierten
ZIPs sowie Diffs zwischen den archivierten `app.js`/`api.php`/`index.html`-Ständen
(`old version/PadelLive-IONOS-v*.zip`). Es existiert kein Git-Verlauf für
`padel_live_android` bzw. `padel_live_ionos` – diese Historie ist daher aus den
genannten Sekundärquellen und Datei-Zeitstempeln rekonstruiert. Lücken sind unten
explizit als solche markiert.

## Architektur-Meilensteine im Überblick

- **v1.2.0–v2.3.0**: native Android-Einzel-Provider-App (nur Playtomic), fest codiert auf
  zwei Clubs (Just Padel Ihringen, Tuniberg Padel), keine Server-Komponente.
- **v2.4.0**: erstmals parallele Web-Versionen – PWA (Next.js, `padel_live_pwa/`) und
  eine IONOS-Webspace-Variante (PHP-Proxy + Vanilla-JS, `padel_live_ionos/`) – Playtomic-only,
  zwei hartkodierte Clubs, ein Tag, ein Sport.
- **v2.5.0/v2.6.0**: lokaler SQLite/Room-Cache ("stale-while-revalidate") für die Android-App,
  siehe `codex-prompt-local-cache.md`. Gleichzeitig Abzweigung eines separaten Workspace
  `APK_Multi_Padel Buchung` auf Basis v2.6.0 für eine größere Multi-Provider-/Multi-Sport-Roadmap
  (Playtomic, MATCHi, Ten'Up, eBuSy, GotCourts, CourtReserve, Anybuddy, ClubSpark) – dieser
  Workspace ersetzt **nicht** die hier dokumentierte Haupt-App.
- **v2.7.0–v2.10.0**: Ausbau von Einzel- auf Multi-Provider (TC Mengen, dann Anybuddy) und von
  einem auf drei Sportarten; Feature-Parität der Web-Version zur App wird ab v2.10.0/v2.11.0
  aktiv nachgezogen.
- **v2.11.0–v2.18.0**: laufende Verfeinerung (Splashscreen, GUI-Review-Paket, Zeitraster,
  Suchzeiträume) – jeweils gespiegelt in Android **und** PWA/IONOS.
- **v2.19.0–v2.23.0**: ausschließlich Android-seitige Bumps am 15.09.2026, ohne begleitende
  PWA-/IONOS-Version und ohne auffindbare schriftliche Dokumentation (siehe Abschnitt unten).

## Versionsübersicht

| Version | Datum/Uhrzeit | Plattform(en) | Kurzbeschreibung |
|---|---|---|---|
| v1.2.0 | 08.09. 05:31 | Android | Frühstadium, nur Playtomic (keine Detaildoku) |
| v1.3.0 | 08.09. 06:02 | Android | „ |
| v1.4.0 | 08.09. 06:28 | Android | „ |
| v1.5.0 | 08.09. 06:59 | Android | „ |
| v1.6.0 | 08.09. 07:05 | Android | „ |
| v1.7.0 | 08.09. 11:00 | Android | „ |
| v1.8.0 | 09.09. 12:03 | Android | „ |
| v1.9.0 | 09.09. 12:20 | Android | „ |
| v2.0.0 | 09.09. 12:37 | Android | „ |
| v2.0.1 | 09.09. 12:41 | Android | „ |
| v2.1.0 | 09.09. 13:14 | Android | „ |
| v2.2.0 | 09.09. 13:48 | Android (+ .aab) | „ |
| v2.2.1 | 09.09. 14:09 | Android (+ .aab) | „ |
| v2.3.0 | 09.09. 14:17 | Android | „ |
| v2.4.0 | 10.09. 06:42–07:36 | Android, PWA, IONOS | Erste parallele Web-Versionen (Playtomic-only) |
| v2.5.0 | 10.09. 10:55 | Android | Vorstufe zum lokalen Cache |
| v2.6.0 | 11.09. 08:01 | Android | Lokaler Slot-Cache (stale-while-revalidate); Basis für separaten Multi-Provider-Workspace |
| v2.7.0 | *nicht separat archiviert* | Android | Provider als Enum refaktoriert (Vorbereitung Multi-Provider); Cache-Key um Provider/Sport erweitert |
| v2.8.0 | *nicht separat archiviert* | Android | TC Mengen: öffentliche Verfügbarkeit ohne Login |
| v2.8.1 | *nicht separat archiviert* | Android | TC Mengen: Login-Grundlage (Firebase Auth, AES-256-verschlüsselt) |
| v2.9.0 | *nicht separat archiviert* | Android | Anybuddy-Zwischenlösung „Club-Link manuell einfügen" (später verworfen) |
| v2.10.0 | 11.09. (vor 12:58) | Android, dann IONOS 12:58 | Anybuddy vollautomatisch ohne manuelle Eingabe; IONOS auf Feature-Parität gehoben |
| v2.11.0 | 11.09. 12:54 / IONOS 13:06 | Android, PWA/IONOS | Splashscreen/Ladeansicht + Versionsanzeige |
| v2.12.0 | 12.09. 08:42/08:43 | Android, PWA | Beginn großes GUI-/Review-Paket (Padel/Tennis-Tabs, Farben) |
| v2.12.1 | 12.09. 15:48/15:51 | Android, PWA | Fortsetzung Review-Paket |
| v2.12.2 | 13.09. 07:29/07:30 | Android, PWA | TC-Mengen-Login getrennt (Login-Test / Speichern) |
| v2.13.0 | *übersprungen* | – | Keine Version v2.13.0 auffindbar (Nummernsprung 2.12.2 → 2.14.0) |
| v2.14.0 | 13.09. 08:08 (Android) / 15:56 (IONOS/PWA) | Android, PWA, IONOS | Ladeansicht in PWA nachgezogen; Padel/Tennis getrennte Clubauswahl; Suchzeitraum auf 2 Wochen begrenzt |
| v2.14.1 | 14.09. 05:40/05:41 | Android, PWA, IONOS | TC-Mengen-Buchung öffnet exakte Tagesansicht zum gewählten Datum |
| v2.15.0 | 14.09. 11:15/11:16 | Android, PWA, IONOS | Zeitfelder (Von/Bis) runden auf 30-Minuten-Raster; TC-Mengen-Buchungslink korrigiert |
| v2.16.0 | 15.09. 04:57 | Android, PWA, IONOS | Neuer Suchzeitraum „4 Wochen" (28 Tage) |
| v2.17.0 | 15.09. 04:59/05:00 | Android, PWA, IONOS | Zeitraster sportabhängig (Tennis 60 Min, Padel 30 Min) |
| v2.18.0 | 15.09. 05:37 | Android, PWA, IONOS | Reiner Versionsbump – keine funktionale Änderung in `app.js`/`api.php` feststellbar |
| v2.19.0 | 15.09. 06:07 | **nur Android** | Keine Dokumentation auffindbar |
| v2.20.0 | 15.09. 10:21 | **nur Android** | Keine Dokumentation auffindbar |
| v2.21.0 | 15.09. 10:43 | **nur Android** | Keine Dokumentation auffindbar |
| v2.22.0 | 15.09. 10:46 | **nur Android** | Keine Dokumentation auffindbar |
| v2.23.0 | 15.09. 10:49 | **nur Android** | Keine Dokumentation auffindbar (aktueller Stand: `versionCode 42`, `versionName '2.23.0'`) |

## Details zu den dokumentierten Versionen

### v2.4.0 – Erste Web-Versionen
PWA (`padel_live_pwa/`, Next.js) und IONOS-Variante (`padel_live_ionos/`, PHP-Proxy +
Vanilla-JS) entstehen parallel zur Android-App. Funktionsumfang zu diesem Zeitpunkt: nur
Playtomic, zwei hartkodierte Clubs, ein Tag, ein Sport – deutlich hinter dem späteren
Android-Funktionsumfang.

### v2.5.0/v2.6.0 – Lokaler Cache
Auftrag an Codex (`codex-prompt-local-cache.md`): ein lokaler SQLite/Room-Cache, der
zuletzt geladene Slots sofort beim App-Start/Bildschirmwechsel anzeigt, während im
Hintergrund ein nicht-blockierendes Live-Update läuft ("stale-while-revalidate", kein
periodischer Hintergrundjob, kein WorkManager/Foreground-Service). Nur aktive/ausgewählte
Provider werden aktualisiert.

Parallel dazu wird v2.6.0 als Ausgangsbasis für einen **separaten** Workspace
`APK_Multi_Padel Buchung` gewählt (`APK_Multi_Padel_Workspace_Setup.md`), der eine größere
Multi-Provider-/Multi-Sport-Architektur (`ProviderAdapter` → `SearchProfile` →
`QueryPlanner`) nach den CourtFinder-Roadmap-Dokumenten aufbauen soll. Dieser Workspace ist
ein eigenständiges Nebenprojekt und nicht Teil der hier dokumentierten Versionskette.

### v2.7.0 – Provider-Refactoring
Provider (Playtomic, Anybuddy, TC Mengen) werden als `enum Provider` modelliert, sodass
weitere Provider künftig als Ein-Zeiler ergänzt werden können. Der Cache-Schlüssel wird um
Provider und Sport erweitert (`provider, sport, club_id, day, resource_id, start_time,
duration`), damit dieselbe Anlage pro Provider/Sport getrennt gecacht werden kann
(Schema-Änderung → DB-Version hochgezählt, alte Cache-Daten werden beim Upgrade verworfen).

### v2.8.0/v2.8.1 – TC Mengen
v2.8.0: Anbindung von TC Mengen (`platzbuchung.de`) – öffentliche Verfügbarkeit ohne Login
über einen unauthentifizierten REST-Endpunkt, live gegen die Demo-Instanz des Anbieters
verifiziert.
v2.8.1: Login-Grundlage für TC Mengen (Benutzername → Firebase-E-Mail-Auflösung → Firebase
Identity Toolkit v3 `verifyPassword`). Zugangsdaten werden ausschließlich in
`EncryptedSharedPreferences` (AES-256) gespeichert, nie in den normalen `SharedPreferences`.
Die eigentliche Buchung bleibt vorerst bei `res.tc-mengen.de` (Mitspieler-Erfassung dort
nicht automatisierbar). Bei dieser Version wurde vergessen, `android.useAndroidX=true` in
`gradle.properties` zu setzen (Konfigurationsfehler, von Codex bei v2.10.0 gefunden und
behoben).

### v2.9.0 – Anybuddy-Zwischenlösung (verworfen)
Erste, bereits ausgelieferte Anybuddy-Integration über manuelles Einfügen eines
Club-Links durch den Nutzer. Diese Lösung wurde von Thomas am 11.09. explizit verworfen
("user soll entlastet werden, es werden keine Links von User eingebunden") und durch die
vollautomatische Lösung in v2.10.0 ersetzt.

### v2.10.0 – Anybuddy vollautomatisch + Web-Parität
Anybuddy wird ohne jede manuelle Eingabe abgefragt: Die App leitet aus einer bereits
bekannten Adresse (GPS oder erster gespeicherter Club) per On-Device-Geocoding einen
Anybuddy-„citySlug" her (`{ort}-{plz}-{land}`, z. B. `ihringen-79241-de`) und fragt die
serverseitig gerenderte Anybuddy-Stadtseite ab (kein Login, kein API-Key). Land/Kontinent
werden nirgends gefiltert; ein 404 gilt als normales „kein Treffer", nicht als Fehler.
Mehrere Anybuddy-Venues pro Stadt werden gleichzeitig gescannt und im Cache über einen
venue-spezifischen `resourceId` auseinandergehalten.

Gleichzeitig wird die IONOS-Version (`padel_live_ionos/`) von v2.4.0 auf denselben
Funktionsumfang wie Android v2.10.0 gehoben (Playtomic frei suchbar, TC Mengen ohne Login,
Anybuddy automatisch, GPS-Suche, 1 Tag–4 Wochen, DE/EN/FR-UI). Bekannte Vereinfachungen der
Web-Version gegenüber der App: gröberes Cache-/Fehlerraster pro Woche statt pro Tag, keine
Kachelfarben-Einstellung, TC-Mengen-Passwort bei „merken" unverschlüsselt im
Browser-`localStorage` (kein Web-Äquivalent zu `EncryptedSharedPreferences`), Anybuddy-Ortsableitung
über OpenStreetMap Nominatim statt Android-Geocoder. Eine Live-Verifikation gegen die echten
Endpunkte (Playtomic, Anybuddy, TC-Mengen-Cloud-Function) war aus der Cloud-Sandbox heraus
nicht möglich (Egress-Policy) – nur unit-getestet.

### v2.11.0 – Splashscreen/Ladeansicht
Android erhält einen animierten Splashscreen (Playtomic-/Anybuddy-Claim, alle drei
Sportarten, orangefarbene Ladeanzeige, Versionsbadge), sichtbar bis zur ersten
Datenabfrage. Die PWA/IONOS-Version zieht ein entsprechendes Web-Overlay nach (eigenständig
umgesetzt, kein 1:1-Abbild, da der aktuelle `MainActivity.java`-Stand in der Session nicht
eingelesen werden konnte). Versionsnummer zusätzlich im Header/Footer sichtbar.

### v2.12.0–v2.12.2 – GUI-/Funktions-Reviewpaket
Umfangreiches, in `review/REVIEW_TODO_v2.11.0.md` dokumentiertes Review- und Änderungspaket
(größtenteils umgesetzt bis Stand 12.09.2026):

- Badminton entfernt, Fokus auf Padel und Tennis; Clubverwaltung und Suchbutton in Tabs
  „Padel"/„Tennis" aufgeteilt, Tab-Auswahl priorisiert gegenüber der globalen Einstellung.
- Ergebniskacheln providerspezifisch gestaltet (Playtomic grün, Anybuddy dunkelgrün, TC
  Mengen rot), korrekt mit Buchungsseite des jeweiligen Providers verknüpft.
- Neuer Informationsbereich links der ersten Ergebniskachel (Datum/Uhrzeit/Dauer,
  dreizeilig), responsive 1–5 Kacheln pro Reihe je nach Bildschirmbreite.
- Startmodi eingeführt: „Manuell", „Mit GPS ohne Suche", „Mit GPS und Suche"; Zeitfenster
  wahlweise nur für den ersten Tag oder alle Tage, Standard 08:00–22:00 Uhr.
- Diverse gemeldete Fehler behoben: GPS-Auswahl wurde bei der Anbieterzählung/Suchfreigabe
  nicht berücksichtigt, fehlende TC-Mengen-Buchungskacheln, fehlerhafte Anybuddy-Orts-/Auftragszuordnung.
- v2.12.2 (13.09.): TC-Mengen-Login-Button aufgeteilt in „LOGIN TESTEN" (prüft, speichert
  nicht) und „SPEICHERN" (verschlüsselt, getrennt); Login-Request an den aktuellen
  `platzbuchung.de`-Client angepasst (Mandant `tcmengen` in Header, URL-Pfad, Origin/Referer).
- Offen geblieben (laut Review-Doku, praxisabhängig): Playtomic-Ergebnisse für alle Länder,
  echter TC-Mengen-Login-Test mit realen Zugangsdaten, vollständige manuelle Prüfung aller
  Start-/Zeitvarianten, visuelle Prüfung auf echtem Android-Gerät/Tablet.

### v2.14.0/v2.14.1 – IONOS-Nachzug + TC-Mengen-Buchungslink
v2.14.0: PWA/IONOS erhält die Ladeansicht, getrennte Padel-/Tennis-Clubauswahl (TC Mengen
nur im Tennis-Bereich) und einen auf zwei Wochen begrenzten Suchzeitraum.
v2.14.1: TC-Mengen-Buchungen öffnen jetzt die öffentliche Tagesansicht exakt am gewählten
Datum, sodass der Termin beim Wechsel zur TC-Mengen-Seite nicht mehr verloren geht.

### v2.15.0–v2.18.0 – Zeitraster und Suchzeiträume (nur per Code-Diff rekonstruiert)
Für diese Versionen existiert kein Freitext-Änderungsprotokoll; die folgenden Punkte
stammen aus dem Vergleich der archivierten `app.js`/`api.php`-Stände:

- **v2.15.0**: Die Zeitfelder „Von"/„Bis" runden Eingaben automatisch auf ein
  30-Minuten-Raster; der TC-Mengen-Buchungslink wurde von einem falschen Tages-Pfadsegment
  (`/1`) auf das korrekte (`/7`) korrigiert.
- **v2.16.0**: Neuer Suchzeitraum „4 Wochen" (28 Tage) zusätzlich zu 1 Tag/2 Tage/1 Woche/2 Wochen.
- **v2.17.0**: Das Zeitraster wird sportabhängig – Tennis in 60-Minuten-, Padel weiterhin in
  30-Minuten-Schritten; beim Sportwechsel werden Zeitfeld-Schrittweite und -Werte
  automatisch angepasst.
- **v2.18.0**: Versionsnummer in `app.js`/`api.php`/`index.html` hochgezählt, aber keine
  sonstige Code-Änderung feststellbar – vermutlich reiner Parität-/Release-Bump oder eine
  ausschließlich Android-seitige Änderung ohne PWA-Gegenstück.

### v2.19.0–v2.23.0 – Undokumentierte Android-Bumps (15.09.2026)
Für diese fünf Versionen ließ sich **keine** Beschreibung finden: kein Eintrag in
`README.txt`, keinem Projekt-Dokument, keinem `review/`- oder `Claude outputs/`-Ordner,
keine versionsspezifischen Code-Kommentare in `MainActivity.java` (das Muster `V2.x.x:`
endet bei v2.10.0). Nachweisbar ist nur:

- `build.gradle` steht aktuell auf `versionCode 42`, `versionName '2.23.0'`.
- Alle fünf APKs wurden am 15.09.2026 zwischen 06:07 und 10:49 Uhr gebaut, davon vier
  (v2.20.0–v2.23.0) in nur 28 Minuten zwischen 10:21 und 10:49 Uhr – ein Muster, das eher zu
  einer kurzen Fix-/Test-Iteration passt als zu fünf eigenständigen Feature-Releases.
- Es existiert **keine** begleitende PWA-/IONOS-ZIP-Version für v2.19.0–v2.23.0 (letzter
  Web-Stand ist weiterhin v2.18.0) – die in `AGENTS.md` geforderte Parität zwischen Android
  und PWA ist für diese fünf Versionen derzeit nicht gegeben.

Falls diese Versionen inhaltlich nachvollzogen werden sollen, wäre ein Blick in Codex' eigene
Sitzungs-/Chatprotokolle nötig (falls vorhanden) – aus den auf diesem Rechner abgelegten
Dateien allein ist der Inhalt nicht rekonstruierbar.

## Bekannte Lücken dieser Nachverfolgung

- Kein Git-Repository mit echter Commit-Historie für `padel_live_android`/`padel_live_ionos`
  (einzig `padel_live_pwa/` ist ein Git-Repo, aber mit nur einem einzigen Commit
  „Create PadelLive PWA").
- v1.2.0–v2.3.0 sowie v2.7.0–v2.9.0 sind nur über Dateizeitstempel bzw. beiläufige Erwähnung
  in späteren Dokumenten datiert/belegt, nicht inhaltlich dokumentiert (außer den oben
  zitierten Code-Kommentaren zu v2.7.0–v2.9.0).
- v2.13.0 fehlt komplett in der Versionsfolge (Sprung von v2.12.2 auf v2.14.0).
- v2.19.0–v2.23.0 sind inhaltlich nicht rekonstruierbar (siehe oben).

## Quellen

- `AGENTS.md` – Versionierungsregeln
- `padel_live_android/app/src/main/java/de/padel/live/MainActivity.java` – Code-Kommentare
  `V2.7.0`–`V2.10.0`, aktueller `build.gradle`-Stand
- Claude-Projekt „Padel-Live Courtfinder": `pwa-ionos-v2.10.0-parity.md`,
  `anybuddy-auto-integration-v2.10.0.md`, `APK_Multi_Padel_Workspace_Setup.md`,
  `codex-prompt-local-cache.md`, `Android-Build-Umgebung.md`
- `review/REVIEW_TODO_v2.11.0.md`
- `old version/PadelLive-IONOS-v*.zip` (README.txt-Stände v2.11.0/v2.14.0/v2.14.1, sowie
  Diffs der `app.js`/`api.php`/`index.html`-Dateien v2.14.1 → v2.18.0)
- Datei-Zeitstempel aller `PadelLive*.apk`/`.aab`/`.zip`-Archive in `Padel Buchung/` und
  `Padel Buchung/old version/`

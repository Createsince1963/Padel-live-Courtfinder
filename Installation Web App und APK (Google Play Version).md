# Installation

Diese Anleitung beschreibt die Installation als PWA oder APK auf Android.

## 01. PWA installieren

Eine Progressive Web App (PWA) wird über den Browser installiert. Es ist keine APK-Datei und kein App Store erforderlich.

### Installation

1. Den bereitgestellten PWA-Link in **Google Chrome** öffnen.
2. Rechts oben auf das Menü mit den drei Punkten tippen.
3. **Zum Startbildschirm hinzufügen** auswählen.
4. **Installieren** antippen.
5. Die Bildschirmanweisungen abschließen.
6. Die App anschließend über das neue Symbol auf dem Startbildschirm oder im App-Menü öffnen.

> Hinweis: Je nach Browser und Android-Version kann der Menüpunkt **App installieren**, **Installieren** oder **Zum Startbildschirm hinzufügen** heißen. Einige Funktionen benötigen weiterhin eine Internetverbindung.

### PWA aktualisieren

Die aktuelle Version wird normalerweise beim Öffnen geladen. Bei Problemen die App vollständig schließen und neu starten.

### PWA deinstallieren

1. **Einstellungen > Apps > Alle Apps anzeigen** öffnen.
2. Die PWA auswählen.
3. **Deinstallieren** antippen.

## 02. APK herunterladen und installieren

Eine APK ist eine Android-Installationsdatei. Diese Methode installiert die App außerhalb des Google Play Store.

### Sicherheitshinweise

- Die APK nur aus dem offiziellen Projekt-Repository oder einer ausdrücklich vertrauenswürdigen Quelle laden.
- Keine APK aus unbekannten E-Mails, Chats oder Download-Portalen installieren.
- Die Berechtigung für unbekannte Apps nach der Installation wieder deaktivieren.

### APK herunterladen

1. Die GitHub-Seite des Projekts öffnen.
2. Unter **Releases** die aktuelle Version auswählen.
3. Unter **Assets** die Datei mit der Endung `.apk` herunterladen.

### Installation über den Dateimanager

1. Den Android-Dateimanager öffnen.
2. Den Ordner **Downloads** öffnen.
3. Die heruntergeladene `.apk`-Datei antippen.
4. Falls Android die Installation blockiert, **Einstellungen** öffnen.
5. Für den verwendeten Dateimanager **Dieser Quelle vertrauen** oder **Aus dieser Quelle zulassen** aktivieren.
6. Zur APK zurückkehren und erneut antippen.
7. **Installieren** auswählen.
8. Nach Abschluss **Öffnen** oder **Fertig** wählen.

### Android-Paket-Installer

Beim Antippen übergibt der Dateimanager die APK an den Paket-Installer. Meldungen prüfen, **Installieren** wählen und die App danach öffnen.

> Falls **App nicht installiert** erscheint, können eine inkompatible Android-Version, eine falsche Prozessorvariante, eine beschädigte Datei oder eine bereits installierte Version mit anderer Signatur die Ursache sein.

### Berechtigung wieder deaktivieren

Der Menüpfad kann je nach Hersteller abweichen:

1. **Einstellungen > Apps > Spezieller App-Zugriff** öffnen.
2. **Unbekannte Apps installieren** auswählen.
3. Den zuvor verwendeten Browser oder Dateimanager auswählen.
4. **Aus dieser Quelle zulassen** wieder deaktivieren.

### APK aktualisieren

1. Neue APK aus der offiziellen Quelle laden.
2. APK öffnen und **Aktualisieren** wählen.
3. Nicht vorher deinstallieren, wenn lokale App-Daten erhalten bleiben sollen.

## Probleme

- **Download nicht auffindbar:** Im Dateimanager den Ordner **Downloads** prüfen.
- **Installation blockiert:** Freigabe muss für genau die App erteilt werden, welche die APK öffnet.
- **Paket ungültig:** APK erneut aus der offiziellen Quelle herunterladen.
- **App nicht kompatibel:** Android-Mindestversion und Gerätearchitektur in den Release-Hinweisen prüfen.
- **Arbeitsgerät:** Unternehmensrichtlinien oder Mobile Device Management können externe Installationen sperren.

# User Stories

## Feature 1: Benutzerverwaltung & Sicherheit

### Authentifizierung

- Als Benutzer/Admin möchte ich mich bei Snowbridge mit E-Mail/Benutzername und Passkey anmelden können.
- Bei meiner Anmeldung sollen meine Zuständigkeitsbereiche per Rolle erkannt werden.
- Als Benutzer möchte ich meine registrierten Passkeys verwalten können (anzeigen, umbenennen, löschen).
- Als Benutzer möchte ich einen neuen Passkey hinzufügen können, falls ich keinen Zugang mehr habe (z. B. neues Gerät).
- Als Benutzer möchte ich die Zwei-Faktor-Authentifizierung aktivieren können, um mein Konto besser abzusichern.

### Benutzerverwaltung (Admin)

- Als Admin möchte ich neue Benutzer anlegen und ihnen Rollen zuweisen können.
- Als Admin möchte ich bestehende Benutzer bearbeiten oder löschen können.
- Als Admin möchte ich eine Übersicht aller registrierten Benutzer und ihrer Rollen sehen.
- Als Admin möchte ich einen Benutzer sperren oder entsperren können.

### Berechtigungen & Sichtbarkeit

- Als Benutzer möchte ich nur die ETL-Jobs sehen und bearbeiten können, für die ich als zuständig eingetragen bin.
- Als Admin möchte ich festlegen können, welche Benutzer oder Rollen auf welche Jobs zugreifen dürfen.

### Audit & Compliance

- Als Admin möchte ich ein Audit-Log einsehen können, das protokolliert, wer wann welchen Job erstellt, geändert oder gestartet hat.
- Als Admin möchte ich Audit-Einträge filtern und exportieren können (z. B. für Compliance-Nachweise).
- (Zertifikat und Datenschutz)

---

## Feature 2: ETL-Kernfunktionen

### Datenquellen (Extract)

- Als Benutzer möchte ich neue Datenquellen (z. B. Datenbanken, APIs, CSV-Dateien) anlegen und konfigurieren können.
- Als Benutzer möchte ich bestehende Datenquellen bearbeiten oder entfernen können.
- Als Benutzer möchte ich die Verbindung zu einer Datenquelle testen können, bevor ich sie verwende.

### Transformationen (Transform)

- Als Benutzer möchte ich Transformationsregeln (Feldmappings, Filter, Berechnungen) separat definieren und in mehreren Jobs wiederverwenden können.
- Als Benutzer möchte ich eine Transformation vor der Ausführung mit Beispieldaten testen können (Dry-Run / Preview), um Fehler frühzeitig zu erkennen.

### Datenziel (Load)

- Als Benutzer möchte ich Datenziele (z. B. Datenbanken, Dateisystem, APIs) anlegen und konfigurieren können.
- Als Benutzer möchte ich die Verbindung zu einem Datenziel testen können, bevor ich es verwende.

### ETL-Jobs

- Als Benutzer möchte ich einen neuen ETL-Job erstellen und dabei Quelle, Transformation und Ziel definieren können.
- Als Benutzer möchte ich einen ETL-Job manuell starten können.
- Als Benutzer möchte ich einen ETL-Job deaktivieren oder löschen können.
- Als Benutzer möchte ich den Fortschritt eines laufenden ETL-Jobs in Echtzeit verfolgen können.

### Datenpipeline & Abhängigkeiten

- Als Benutzer möchte ich Jobs verketten können, sodass ein Folge-Job automatisch startet, wenn der vorherige erfolgreich abgeschlossen wurde.
- Als Benutzer möchte ich den Status einer Pipeline (Kette von Jobs) als Ganzes überwachen können.

---

## Feature 3: Betrieb & Monitoring

### Scheduling

- Als Benutzer möchte ich ETL-Jobs zeitgesteuert (z. B. täglich, stündlich) ausführen lassen können.
- Als Benutzer möchte ich den Zeitplan eines Jobs jederzeit ändern oder deaktivieren können.

### Monitoring & Logs

- Als Benutzer möchte ich die Ausführungshistorie eines ETL-Jobs einsehen können (Start, Ende, Status, Fehler).
- Als Benutzer möchte ich bei einem fehlgeschlagenen Job eine Benachrichtigung erhalten.
- Als Admin möchte ich systemweite Logs einsehen können, um Fehler zu diagnostizieren.

### Fehlerbehandlung

- Als Benutzer möchte ich bei einem fehlgeschlagenen Job eine verständliche Fehlermeldung sehen.
- Als Benutzer möchte ich einen fehlgeschlagenen Job nach Behebung des Fehlers erneut starten können.
- Als Benutzer möchte ich festlegen können, wie oft ein Job bei einem Fehler automatisch wiederholt werden soll (Retry-Strategie).

---

## Feature 4: Oberfläche & Bedienung

### Navigation & Dashboard

- Als Benutzer/Admin möchte ich per Navigationsmenü zwischen den verschiedenen Seiten navigieren können.
- Als Benutzer möchte ich auf einer Startseite (Dashboard) einen Überblick über den aktuellen Status aller ETL-Jobs sehen.

### Import / Export

- Als Benutzer möchte ich eine Job-Konfiguration als Datei exportieren können, um sie in einer anderen Umgebung (z. B. Dev → Prod) zu importieren.
- Als Benutzer möchte ich mehrere Jobs auf einmal exportieren oder importieren können.

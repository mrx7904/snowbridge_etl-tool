# Issues – Snowbridge ETL-Tool

> Generiert aus `Planning/UserStories.md`. Issues sind nach Unterbereich gruppiert.
> Je ein Issue pro Sub-Feature (~38 User Stories → 14 Issues).

---

## Feature 1: Benutzerverwaltung & Sicherheit

---

### Issue #1: Passkey-Authentifizierung und Anmeldung implementieren

**Labels:** `feature` `auth` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer/Admin möchte ich mich bei Snowbridge mit E-Mail/Benutzername und Passkey anmelden können.
- Bei meiner Anmeldung sollen meine Zuständigkeitsbereiche per Rolle erkannt werden.
- Als Benutzer möchte ich meine registrierten Passkeys verwalten können (anzeigen, umbenennen, löschen).
- Als Benutzer möchte ich einen neuen Passkey hinzufügen können, falls ich keinen Zugang mehr habe (z. B. neues Gerät).
- Als Benutzer möchte ich die Zwei-Faktor-Authentifizierung aktivieren können, um mein Konto besser abzusichern.

### Beschreibung
Snowbridge benötigt eine passwortlose Anmeldung via WebAuthn/Passkey. Nach erfolgreicher Anmeldung wird die Benutzerrolle ausgelesen und für die rollenbasierte Zugriffskontrolle im gesamten System gesetzt. Benutzer können ihre Passkeys selbst verwalten und bei Bedarf neue hinzufügen.

### Akzeptanzkriterien
- [ ] Anmeldung mit E-Mail/Benutzername und Passkey funktioniert
- [ ] Anmeldung schlägt fehl bei ungültigen Credentials (Fehlermeldung wird angezeigt)
- [ ] Benutzerrolle wird nach Anmeldung korrekt erkannt und als Claim gesetzt
- [ ] Benutzer kann registrierte Passkeys anzeigen, umbenennen und löschen
- [ ] Benutzer kann einen neuen Passkey hinzufügen (z. B. für ein neues Gerät)
- [ ] Zwei-Faktor-Authentifizierung kann aktiviert werden
- [ ] Session endet nach definierter Inaktivitätszeit

### Technische Hinweise
- Blazor Server: Login-Seite und Passkey-Verwaltungsseite als Razor Components
- C#: WebAuthn-Bibliothek (z. B. `Fido2NetLib`), ASP.NET Core Identity für Rollenverwaltung
- Rollen als Claims in der Blazor-Session
- Konfiguration der Rollen über `appsettings.json` oder MariaDB

---

### Issue #2: Benutzerverwaltung für Admins implementieren

**Labels:** `feature` `auth` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Admin möchte ich neue Benutzer anlegen und ihnen Rollen zuweisen können.
- Als Admin möchte ich bestehende Benutzer bearbeiten oder löschen können.
- Als Admin möchte ich eine Übersicht aller registrierten Benutzer und ihrer Rollen sehen.
- Als Admin möchte ich einen Benutzer sperren oder entsperren können.

### Beschreibung
Admins benötigen eine Verwaltungsoberfläche, über die sie Benutzerkonten anlegen, bearbeiten, löschen, sperren und Rollen zuweisen können. Die Übersicht aller Benutzer muss über eine geschützte Admin-Seite erreichbar sein.

### Akzeptanzkriterien
- [ ] Admin kann neue Benutzer anlegen und ihnen eine Rolle zuweisen
- [ ] Admin kann Benutzerdaten (Name, E-Mail, Rolle) bearbeiten
- [ ] Admin kann einen Benutzer löschen
- [ ] Admin sieht eine Übersicht aller Benutzer mit Rolle und Status
- [ ] Admin kann einen Benutzer sperren (Login wird verweigert) und entsperren
- [ ] Seite ist nur für Admins erreichbar (`AuthorizeView Roles="Admin"`)

### Technische Hinweise
- Blazor Server: Admin-Bereich als geschützter Seitenbereich
- C#: CRUD-Operationen über ASP.NET Core Identity
- MariaDB: Benutzertabelle mit Rollenzuweisung und `IsLocked`-Flag

---

### Issue #3: Rollenbasierte Berechtigungen und Sichtbarkeit umsetzen

**Labels:** `feature` `auth` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich nur die ETL-Jobs sehen und bearbeiten können, für die ich als zuständig eingetragen bin.
- Als Admin möchte ich festlegen können, welche Benutzer oder Rollen auf welche Jobs zugreifen dürfen.

### Beschreibung
ETL-Jobs müssen Benutzern oder Rollen zugeordnet werden können. Die Oberfläche zeigt jedem Benutzer nur die für ihn freigegebenen Jobs an. Admins verwalten die Zugriffskontrolle über eine Berechtigungsmaske pro Job.

### Akzeptanzkriterien
- [ ] Benutzer sieht nur Jobs, denen er oder seine Rolle zugeordnet ist
- [ ] Admin kann einem Job beliebige Benutzer oder Rollen zuweisen
- [ ] Zugriff auf nicht freigegebene Jobs wird serverseitig verweigert (kein reines UI-Hide)
- [ ] Berechtigungsänderungen wirken sich ohne Neustart der App aus

### Technische Hinweise
- C#: Policy-basierte Autorisierung in ASP.NET Core
- Datenmodell: `JobPermission`-Tabelle (JobId, UserId/RoleId)
- Blazor: `AuthorizeView` mit dynamischen Policies

---

### Issue #4: Audit-Log für sicherheitsrelevante Aktionen implementieren

**Labels:** `feature` `auth` `backend`
**Milestone:** `MVP`
**Priorität:** `Medium`

### User Stories
- Als Admin möchte ich ein Audit-Log einsehen können, das protokolliert, wer wann welchen Job erstellt, geändert oder gestartet hat.
- Als Admin möchte ich Audit-Einträge filtern und exportieren können (z. B. für Compliance-Nachweise).

### Beschreibung
Alle sicherheitsrelevanten und betrieblichen Aktionen (Job erstellen, ändern, starten, löschen; Benutzer sperren) werden in einem Audit-Log persistiert. Admins können das Log filtern und als CSV exportieren.

### Akzeptanzkriterien
- [ ] Jede Aktion (Job-CRUD, Job-Start, Benutzer-Sperre) erzeugt einen Audit-Eintrag mit Zeitstempel, Benutzer und Aktion
- [ ] Admin kann das Audit-Log nach Datum, Benutzer und Aktionstyp filtern
- [ ] Admin kann gefilterte Einträge als CSV exportieren
- [ ] Audit-Log ist schreibgeschützt (keine Einträge können gelöscht werden)

### Technische Hinweise
- C#: Audit-Service als Singleton, der von allen relevanten Services aufgerufen wird
- MariaDB: `AuditLog`-Tabelle (Id, Timestamp, UserId, Action, EntityType, EntityId)
- CSV-Export über `CsvHelper` oder eigene Implementierung

---

## Feature 2: ETL-Kernfunktionen

---

### Issue #5: Datenquellen anlegen, konfigurieren und testen

**Labels:** `feature` `backend` `frontend` `py`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich neue Datenquellen (z. B. Datenbanken, APIs, CSV-Dateien) anlegen und konfigurieren können.
- Als Benutzer möchte ich bestehende Datenquellen bearbeiten oder entfernen können.
- Als Benutzer möchte ich die Verbindung zu einer Datenquelle testen können, bevor ich sie verwende.

### Beschreibung
Benutzer verwalten Datenquellen über eine CRUD-Oberfläche. Verbindungsparameter werden verschlüsselt gespeichert. Ein Verbindungstest prüft die Erreichbarkeit der Quelle, bevor sie in einem Job verwendet wird.

### Akzeptanzkriterien
- [ ] Benutzer kann eine neue Datenquelle anlegen (Name, Typ, Verbindungsparameter)
- [ ] Benutzer kann eine bestehende Datenquelle bearbeiten
- [ ] Benutzer kann eine Datenquelle löschen (nur wenn sie keinem aktiven Job zugeordnet ist)
- [ ] Verbindungstest gibt Erfolg oder verständliche Fehlermeldung zurück
- [ ] Zugangsdaten werden nicht im Klartext gespeichert

### Technische Hinweise
- Blazor: Formular mit dynamischen Feldern je nach Quellentyp
- C#: `ConnectionService` delegiert Verbindungstest an Python-Worker
- Python: Verbindungstest-Skript je Quellentyp (z. B. `test_connection.py`)
- Secrets in `.env`-Datei, nicht in der Datenbank

---

### Issue #6: Transformationsregeln definieren und per Dry-Run testen

**Labels:** `feature` `backend` `frontend` `py`
**Milestone:** `MVP`
**Priorität:** `Medium`

### User Stories
- Als Benutzer möchte ich Transformationsregeln (Feldmappings, Filter, Berechnungen) separat definieren und in mehreren Jobs wiederverwenden können.
- Als Benutzer möchte ich eine Transformation vor der Ausführung mit Beispieldaten testen können (Dry-Run / Preview), um Fehler frühzeitig zu erkennen.

### Beschreibung
Transformationen sind wiederverwendbare Konfigurationsbausteine, die unabhängig von einzelnen Jobs definiert werden. Ein Dry-Run führt die Transformation auf einer kleinen Stichprobe aus und zeigt das Ergebnis als Vorschau an, ohne Daten zu schreiben.

### Akzeptanzkriterien
- [ ] Benutzer kann eine Transformation mit Feldmappings, Filtern und Berechnungen anlegen
- [ ] Eine Transformation kann in mehreren Jobs referenziert werden
- [ ] Dry-Run führt die Transformation auf Beispieldaten aus und zeigt das Ergebnis an
- [ ] Dry-Run schreibt keine Daten in das Zielsystem
- [ ] Fehler im Dry-Run werden verständlich angezeigt

### Technische Hinweise
- Konfiguration als JSON gespeichert (erweiterbar ohne Schema-Migration)
- Python: Dry-Run-Skript verarbeitet Stichprobe und gibt Vorschau-Daten zurück
- C#: Ergebnis des Dry-Run wird als Datentabelle in der Blazor-UI gerendert

---

### Issue #7: Datenziele anlegen, konfigurieren und testen

**Labels:** `feature` `backend` `frontend` `py`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich Datenziele (z. B. Datenbanken, Dateisystem, APIs) anlegen und konfigurieren können.
- Als Benutzer möchte ich die Verbindung zu einem Datenziel testen können, bevor ich es verwende.

### Beschreibung
Analog zu Datenquellen verwalten Benutzer Datenziele über eine CRUD-Oberfläche. Snowflake ist das primäre Datenziel. Ein Verbindungstest prüft die Erreichbarkeit vor der ersten Verwendung.

### Akzeptanzkriterien
- [ ] Benutzer kann ein neues Datenziel anlegen (Name, Typ, Verbindungsparameter)
- [ ] Benutzer kann ein bestehendes Datenziel bearbeiten
- [ ] Benutzer kann ein Datenziel löschen (nur wenn es keinem aktiven Job zugeordnet ist)
- [ ] Verbindungstest gibt Erfolg oder verständliche Fehlermeldung zurück
- [ ] Snowflake wird als primärer Zieltyp unterstützt

### Technische Hinweise
- Python: Snowflake-Connector (`snowflake-connector-python`) für Verbindungstest und Load
- C#: Einheitliches Interface für Datenquellen und Datenziele (`IConnector`)
- Secrets via `.env`

---

### Issue #8: ETL-Jobs erstellen, verwalten und manuell ausführen

**Labels:** `feature` `backend` `frontend` `py`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich einen neuen ETL-Job erstellen und dabei Quelle, Transformation und Ziel definieren können.
- Als Benutzer möchte ich einen ETL-Job manuell starten können.
- Als Benutzer möchte ich einen ETL-Job deaktivieren oder löschen können.
- Als Benutzer möchte ich den Fortschritt eines laufenden ETL-Jobs in Echtzeit verfolgen können.

### Beschreibung
ETL-Jobs sind das zentrale Artefakt der Anwendung. Ein Job verknüpft Quelle, Transformation und Ziel. Jobs können manuell gestartet, deaktiviert oder gelöscht werden. Der Fortschritt wird in Echtzeit über Blazor Server's SignalR-Verbindung in der UI aktualisiert.

### Akzeptanzkriterien
- [ ] Benutzer kann einen neuen Job anlegen und Quelle, Transformation und Ziel zuweisen
- [ ] Job kann manuell gestartet werden
- [ ] Laufender Job zeigt Echtzeit-Fortschritt in der UI (verarbeitete Zeilen, Status)
- [ ] Job kann deaktiviert werden (kein manueller Start, kein Scheduling)
- [ ] Job kann gelöscht werden (mit Bestätigungsdialog)
- [ ] Fehlgeschlagener Job zeigt Status `FAILED` mit Fehlermeldung

### Technische Hinweise
- C#: `JobRunner`-Service startet Python-Worker via `Process.Start`
- Python: ETL-Skript gibt Fortschrittszeilen via stdout aus (z. B. `PROGRESS:50`)
- Blazor: Echtzeit-Update via `InvokeAsync(StateHasChanged)` aus dem Job-Service
- MariaDB: `JobRun`-Tabelle (JobId, StartTime, EndTime, Status, Log)

---

### Issue #9: Datenpipelines aus verketteten Jobs aufbauen

**Labels:** `feature` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `Medium`

### User Stories
- Als Benutzer möchte ich Jobs verketten können, sodass ein Folge-Job automatisch startet, wenn der vorherige erfolgreich abgeschlossen wurde.
- Als Benutzer möchte ich den Status einer Pipeline (Kette von Jobs) als Ganzes überwachen können.

### Beschreibung
Mehrere ETL-Jobs können zu einer Pipeline zusammengefasst werden. Ein Folge-Job startet automatisch, sobald der vorherige erfolgreich abgeschlossen wurde. Der Gesamtstatus der Pipeline ist auf einer Übersichtsseite sichtbar.

### Akzeptanzkriterien
- [ ] Benutzer kann Jobs zu einer Pipeline verketten (Reihenfolge definieren)
- [ ] Folge-Job startet automatisch nach erfolgreichem Abschluss des Vorgängers
- [ ] Bei Fehler eines Jobs wird die Pipeline gestoppt (kein Folge-Job startet)
- [ ] Pipeline-Übersicht zeigt Status aller enthaltenen Jobs und den Gesamtstatus
- [ ] Pipeline kann manuell gestartet werden

### Technische Hinweise
- C#: `PipelineOrchestrator`-Service überwacht Job-Abschlüsse und startet Folge-Jobs
- MariaDB: `Pipeline`- und `PipelineStep`-Tabelle
- Blazor: Pipeline-Übersichtsseite mit Status-Badges je Job

---

## Feature 3: Betrieb & Monitoring

---

### Issue #10: Zeitgesteuerte Job-Ausführung (Scheduling) implementieren

**Labels:** `feature` `backend` `sch`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich ETL-Jobs zeitgesteuert (z. B. täglich, stündlich) ausführen lassen können.
- Als Benutzer möchte ich den Zeitplan eines Jobs jederzeit ändern oder deaktivieren können.

### Beschreibung
Jeder ETL-Job kann mit einem Cron-basierten Zeitplan versehen werden. Der integrierte Scheduler prüft in regelmäßigen Abständen fällige Jobs und startet sie automatisch. Zeitpläne können jederzeit geändert oder deaktiviert werden, ohne den Job selbst zu löschen.

### Akzeptanzkriterien
- [ ] Benutzer kann einem Job einen Cron-Ausdruck als Zeitplan zuweisen
- [ ] Scheduler führt fällige Jobs automatisch aus
- [ ] Benutzer kann den Zeitplan eines Jobs ändern oder deaktivieren
- [ ] Deaktivierter Zeitplan verhindert automatische Ausführung (manueller Start bleibt möglich)
- [ ] Nächste geplante Ausführung wird in der Job-Übersicht angezeigt

### Technische Hinweise
- C#: Hosted Service (`IHostedService`) als Scheduler mit Cron-Auswertung (z. B. `Cronos`-Bibliothek)
- MariaDB: `Schedule`-Tabelle (JobId, CronExpression, IsActive, NextRun)

---

### Issue #11: Ausführungshistorie und Benachrichtigungen für ETL-Jobs

**Labels:** `feature` `backend` `frontend` `log`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer möchte ich die Ausführungshistorie eines ETL-Jobs einsehen können (Start, Ende, Status, Fehler).
- Als Benutzer möchte ich bei einem fehlgeschlagenen Job eine Benachrichtigung erhalten.
- Als Admin möchte ich systemweite Logs einsehen können, um Fehler zu diagnostizieren.

### Beschreibung
Jeder Job-Run wird mit Zeitstempel, Dauer, Status und Log persistiert. Benutzer erhalten bei fehlgeschlagenen Jobs eine Benachrichtigung (z. B. E-Mail oder In-App). Admins haben Zugriff auf systemweite Logs über eine separate Ansicht.

### Akzeptanzkriterien
- [ ] Jeder Job-Run erscheint in der Ausführungshistorie mit Start, Ende, Status und Fehlertext
- [ ] Log-Ausgabe (stdout/stderr des Python-Workers) wird je Run gespeichert und anzeigbar
- [ ] Benutzer erhält eine Benachrichtigung bei fehlgeschlagenem Job (Kanal konfigurierbar)
- [ ] Admin-Seite zeigt systemweite Logs aller Jobs aller Benutzer
- [ ] Logs können nach Datum, Job und Status gefiltert werden

### Technische Hinweise
- C#: Log-Parsing aus stdout/stderr des Python-Prozesses, Speicherung in `JobRun`-Tabelle
- Benachrichtigung: E-Mail via SMTP oder In-App-Notification (Blazor Toast)
- Admin-Seite: gefilterte Abfrage über alle `JobRun`-Einträge

---

### Issue #12: Fehlerbehandlung und Retry-Strategie für Jobs implementieren

**Labels:** `feature` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `Medium`

### User Stories
- Als Benutzer möchte ich bei einem fehlgeschlagenen Job eine verständliche Fehlermeldung sehen.
- Als Benutzer möchte ich einen fehlgeschlagenen Job nach Behebung des Fehlers erneut starten können.
- Als Benutzer möchte ich festlegen können, wie oft ein Job bei einem Fehler automatisch wiederholt werden soll (Retry-Strategie).

### Beschreibung
Fehlgeschlagene Jobs zeigen eine aufbereitete Fehlermeldung aus dem Python-Log. Benutzer können einen fehlgeschlagenen Job manuell neu starten. Pro Job kann eine Retry-Strategie (Anzahl Versuche, Wartezeit) konfiguriert werden.

### Akzeptanzkriterien
- [ ] Fehlermeldung aus dem Python-Worker wird verständlich aufbereitet und angezeigt
- [ ] Fehlgeschlagener Job kann mit einem Klick neu gestartet werden
- [ ] Benutzer kann Retry-Anzahl und Wartezeit zwischen Versuchen konfigurieren
- [ ] Automatische Retries werden in der Ausführungshistorie als separate Einträge geführt
- [ ] Nach Ausschöpfung aller Retries wird der Job als `FAILED` markiert und Benachrichtigung ausgelöst

### Technische Hinweise
- C#: `JobRunner` implementiert Retry-Logik mit konfigurierbarem Backoff
- Python: Exit-Codes und strukturierte Fehlermeldungen auf stderr
- MariaDB: `retry_count` und `max_retries`-Felder in der `JobRun`-Tabelle

---

## Feature 4: Oberfläche & Bedienung

---

### Issue #13: Navigationsmenü und Dashboard implementieren

**Labels:** `feature` `frontend`
**Milestone:** `MVP`
**Priorität:** `High`

### User Stories
- Als Benutzer/Admin möchte ich per Navigationsmenü zwischen den verschiedenen Seiten navigieren können.
- Als Benutzer möchte ich auf einer Startseite (Dashboard) einen Überblick über den aktuellen Status aller ETL-Jobs sehen.

### Beschreibung
Die Anwendung erhält ein persistentes Navigationsmenü, das rollenbasiert unterschiedliche Menüpunkte einblendet. Das Dashboard zeigt auf einen Blick den Status aller dem Benutzer zugeordneten ETL-Jobs (aktiv, fehlgeschlagen, geplant).

### Akzeptanzkriterien
- [ ] Navigationsmenü ist auf allen authentifizierten Seiten sichtbar
- [ ] Aktive Seite wird im Menü hervorgehoben
- [ ] Menüpunkte werden rollenbasiert ein-/ausgeblendet
- [ ] Navigation erfolgt ohne vollständigen Seitenreload (Blazor-Routing)
- [ ] Dashboard zeigt Status-Übersicht aller zugeordneten Jobs (Kacheln oder Tabelle)
- [ ] Dashboard aktualisiert sich bei laufenden Jobs automatisch

### Technische Hinweise
- Blazor: `NavMenu.razor` in `Shared/`, `MainLayout.razor` als Seitenrahmen
- Rollenbasierte Sichtbarkeit via `<AuthorizeView Roles="Admin">`
- Dashboard: Blazor-Komponente mit `@implements IDisposable` für automatisches Update

---

### Issue #14: Job-Konfigurationen exportieren und importieren

**Labels:** `feature` `backend` `frontend`
**Milestone:** `MVP`
**Priorität:** `Low`

### User Stories
- Als Benutzer möchte ich eine Job-Konfiguration als Datei exportieren können, um sie in einer anderen Umgebung (z. B. Dev → Prod) zu importieren.
- Als Benutzer möchte ich mehrere Jobs auf einmal exportieren oder importieren können.

### Beschreibung
Job-Konfigurationen (Quelle, Transformation, Ziel, Zeitplan) können als JSON-Dateien exportiert und in einer anderen Snowbridge-Instanz importiert werden. Der Import prüft auf Konflikte (z. B. bereits vorhandene Job-Namen) und erlaubt dem Benutzer, diese aufzulösen.

### Akzeptanzkriterien
- [ ] Einzelner Job kann als JSON-Datei exportiert werden
- [ ] Mehrere Jobs können als gebündelte JSON-Datei exportiert werden
- [ ] JSON-Datei kann über die UI importiert werden
- [ ] Import prüft auf Namenskonflikte und zeigt diese dem Benutzer an
- [ ] Secrets (Verbindungspasswörter) werden beim Export nicht im Klartext ausgegeben

### Technische Hinweise
- C#: Serialisierung der Job-Konfiguration via `System.Text.Json`
- Secrets beim Export durch Platzhalter ersetzen (`***REDACTED***`)
- Blazor: Datei-Download via `IJSRuntime` und Upload via `InputFile`-Komponente

---

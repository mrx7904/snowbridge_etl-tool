---
title:   Snowbridge ETL-Tool
author:  Marc Schubert
subject: Technische Dokumentation / Softwareentwicklung
version: v0.1
date:    20. Juli 2026
company: Alsco Berufskleidungs-Service GmbH · Datenmanagement
toc:     true
---

# Snowbridge – Technische Dokumentation

## 1. Überblick

Snowbridge ist ein leichtgewichtiges ETL-Werkzeug (Extract, Transform, Load), das im Rahmen einer Bachelorarbeit mittels Agent-Driven Software Engineering (ADSE) entwickelt wird. Ziel ist die Realisierung einer individuellen, wartbaren und erweiterbaren Datenpipeline-Lösung als Alternative zu kommerziellen SaaS-ETL-Plattformen wie Fivetran oder Airbyte.

Das Tool ist auf die Anbindung von **Snowflake** als Cloud-Data-Warehouse ausgerichtet und ermöglicht die Definition, Ausführung und Überwachung von ETL-Workflows über eine webbasierte Benutzeroberfläche.

### 1.1 Systemübersicht

| Komponente       | Technologie              | Version    | Beschreibung                              |
|------------------|--------------------------|------------|-------------------------------------------|
| Frontend & Backend | Blazor Server (.NET)   | .NET 10 LTS | UI und Anwendungslogik (C#)             |
| ETL-Workflows    | Python                   | 3.10+      | Datenverarbeitung und DB-Verbindungen     |
| Ziel-DWH         | Snowflake                | –          | Cloud-Data-Warehouse                      |
| Konfiguration    | JSON / .env              | –          | Verbindungsparameter, Secrets             |

### 1.2 Architekturmuster: C# als Command Center, Python als Worker

Snowbridge folgt einem klaren Prozess-Isolations-Muster: Die C#/.NET-Schicht übernimmt die Orchestrierung, Scheduling, Logging und die Bereitstellung der Benutzeroberfläche. Die eigentliche Datenverarbeitung wird an Python-Skripte delegiert, die als separate Prozesse gestartet werden.

```
┌──────────────────────────────────────────┐
│           Blazor Server (C#/.NET)        │
│  UI · Scheduling · Logging · Auth        │
│                  │                       │
│       ConnectionService / Runner         │
└──────────────────┬───────────────────────┘
                   │ Prozess-Start (Process.Start)
                   ▼
┌──────────────────────────────────────────┐
│         Python ETL-Worker                │
│  Extraktion · Transformation · Laden     │
│  Snowflake-Connector · .env-Config       │
└──────────────────────────────────────────┘
```

> [!NOTE] Die Prozess-Isolation gewährleistet, dass ein fehlerhafter ETL-Lauf nicht den Blazor-Serverprozess destabilisiert. Logs werden über stdout/stderr des Python-Prozesses gesammelt und im C#-Layer geparst.

### 1.3 Minimum Viable Feature Set (MVP)

| Feature           | Status        | Beschreibung                                      |
|-------------------|---------------|---------------------------------------------------|
| Authentifizierung | 🔲 Geplant    | E-Mail/Benutzername + Passkey, rollenbasiert      |
| Navigation        | 🔲 Geplant    | Seitenübergreifendes Navigationsmenü              |
| Scheduling        | 🔲 Geplant    | Zeitgesteuerte Ausführung von ETL-Workflows       |
| Logging           | 🔲 Geplant    | Ausführungsprotokoll je Workflow-Run              |
| Data Preview      | 🔲 Geplant    | Vorschau der verarbeiteten Daten im Browser       |
| Snowflake-Anbindung| 🔲 Geplant   | Verbindung und Daten-Load via Python-Connector    |

---

## 2. Architektur & Tech-Stack

### 2.1 Tech-Stack-Begründung

_Dieser Abschnitt wird während der Umsetzung befüllt._

### 2.2 Projektstruktur

_Dieser Abschnitt wird nach Initialisierung des Projekts befüllt._

### 2.3 Konfigurationsmanagement

Verbindungsparameter und Secrets werden ausschließlich über `.env`-Dateien und `.json`-Konfigurationsdateien verwaltet. Keine Credentials im Source Code.

> [!WARNING] `.env`-Dateien dürfen nicht in das Git-Repository eingecheckt werden. Eine `.env.example` mit Platzhaltern wird zur Dokumentation der benötigten Variablen gepflegt.

---

## 3. Frontend (Blazor Server UI)

### 3.1 Seitenstruktur

_Dieser Abschnitt wird mit Fortschritt der UI-Entwicklung befüllt._

### 3.2 Authentifizierung

_Dieser Abschnitt wird nach Implementierung der Auth-Komponente befüllt._

---

## 4. Backend (C# / .NET)

### 4.1 ConnectionService

_Dieser Abschnitt wird nach Implementierung des ConnectionService befüllt._

### 4.2 Scheduling

_Dieser Abschnitt wird nach Implementierung des Schedulers befüllt._

### 4.3 Logging & Monitoring

_Dieser Abschnitt wird nach Implementierung des Log-Systems befüllt._

---

## 5. Python ETL-Workflows

### 5.1 Snowflake-Anbindung

_Dieser Abschnitt wird nach Implementierung der Snowflake-Verbindung befüllt._

### 5.2 Parameterübergabe C# → Python

_Dieser Abschnitt wird nach Implementierung der Interop-Schnittstelle befüllt._

### 5.3 Fehlerbehandlung & Logging

_Dieser Abschnitt wird nach Implementierung der Fehlerbehandlung befüllt._

---

## 6. Deployment & Betrieb

_Dieser Abschnitt wird nach Abschluss der Implementierung befüllt._

---

## 7. Glossar

| Begriff  | Beschreibung                                                            |
|----------|-------------------------------------------------------------------------|
| ADSE     | Agent-Driven Software Engineering – KI-gestützte Softwareentwicklung   |
| ETL      | Extract, Transform, Load – Datenpipeline-Paradigma                      |
| Blazor   | Microsoft-Framework für webbasierte .NET-Anwendungen                    |
| Snowflake| Cloud-Data-Warehouse-Plattform                                          |
| MVP      | Minimum Viable Product – minimaler, funktionstüchtiger Funktionsumfang  |
| CAPEX    | Capital Expenditure – einmalige Investitionskosten                      |
| OPEX     | Operational Expenditure – laufende Betriebskosten                      |
| TCO      | Total Cost of Ownership – Gesamtkostenbetrachtung                       |

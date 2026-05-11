# Gliederung der Bachelorarbeit

**Arbeitstitel:** Potenziale von Agent-Driven Software Engineering zur Realisierung von ETL-Systemen: Ein Kosten-Nutzen-Vergleich zwischen Eigenentwicklung und Standardsoftware  
**Verfasser:** Marc Schubert  
**Zielumfang:** ca. 40 Textseiten (± 10%)

---

## 1. Einleitung (ca. 3–4 Seiten)
*Ziel: Hinführung zum Thema, Relevanz verdeutlichen und Forschungsrahmen abstecken.*

* **1.1 Motivation:** ADSE als neues Paradigma – Chancen für die kosteneffiziente Realisierung individueller Datenpipelines.
* **1.2 Problemstellung:** Fehlende empirische Erkenntnisse zu Potenzialen und Grenzen von ADSE im Kontext von ETL-Systemen.
* **1.3 Zielsetzung und Forschungsfrage:** Welche Potenziale bietet ADSE bei der Entwicklung von ETL-Werkzeugen, und wie verhält sich der Kosten-Nutzen-Saldo im Vergleich zu Standardsoftware?
* **1.4 Aufbau der Arbeit:** Kurzer Überblick über die Struktur.

## 2. Theoretische Grundlagen und Stand der Technik (ca. 7–9 Seiten)
*Ziel: Notwendiges Begriffsverständnis schaffen, ohne unnötige Breite.*

* **2.1 Agent-Driven Software Engineering (ADSE):**
    * Paradigmenwechsel: Vom Coding zur Orchestrierung von KI-Agenten.
    * Potenziale: Produktivitätssteigerung, Zugänglichkeit, Time-to-Market.
    * Aktuelle Grenzen: Halluzinationen, Kontextgrenzen, Qualitätssicherung.
* **2.2 ETL-Architekturen und der Modern Data Stack:**
    * Herausforderungen bei Cloud-Data-Warehouses (Snowflake).
    * Hybride Anwendungsarchitekturen (.NET für UI/Logik vs. Python für Data Processing).
* **2.3 Make-or-Buy Entscheidungsmodelle in der IT:**
    * Total Cost of Ownership (TCO) Betrachtung (CAPEX vs. OPEX).
    * Kriterien für Nischen-Software (Vendor Lock-in, Anpassbarkeit, Time-to-Market).

## 3. Konzeption der Fallstudie (ca. 5–6 Seiten)
*Ziel: Definition des Untersuchungsrahmens, des Artefakts und des Vergleichsmaßstabs.*

* **3.1 Methodik:** Design Science Research (DSR) Ansatz (Fokus auf Artefakt und Evaluation).
* **3.2 Vorstellung des Untersuchungsobjekts: ETL-Werkzeug „Snowbridge":**
    * Einordnung als repräsentatives Beispiel für domänenspezifische ETL-Eigenentwicklungen.
    * Anforderungsanalyse (Minimum Viable Feature Set): Scheduling, Logging, Data Preview, Snowflake-Anbindung.
    * Nicht-funktionale Anforderungen: Usability (Blazor UI), Erweiterbarkeit (Python Skripte).
* **3.3 Definition des Referenz-Szenarios (Benchmark):**
    * Auswahl der Vergleichs-SaaS (z.B. Fivetran/Airbyte Standard Tier).
    * Modellierung der Lizenzkosten über 3 Jahre.

## 4. Realisierung des ETL-Systems mittels ADSE (ca. 11–13 Seiten)
*Ziel: Detaillierte Beschreibung der technischen Umsetzung und empirische Analyse der ADSE-Potenziale.*

* **4.1 Architekturentwurf und Tech-Stack-Evaluation:**
    * Auswahl und Begründung: .NET 10, Python 3.10+, MariaDB.
    * Das Prozess-Isolations-Pattern: C# als „Command Center", Python als „Worker".
* **4.2 Implementierung der Kernkomponenten:**
    * Frontend: Generierung der Blazor Server UI, Dashboards und Komponenten.
    * Backend: Entwicklung des `ConnectionService` und Konfigurationsmanagement (`appsettings.json`).
    * Schnittstelle: Parameterübergabe, Prozesssteuerung und Log-Parsing (Tracebacks in UI).
* **4.3 Empirische Analyse der ADSE-Potenziale:**
    * Produktivitätspotenziale: Schnelle Generierung von Boilerplate-Code, Architekturvorschläge, Dokumentation.
    * Qualitätspotenziale: Code-Review durch KI, konsistente Namensgebung, automatisierte Testfälle.
    * Identifizierte Grenzen: Debugging komplexer Interop-Fehler, Halluzinationen bei neuen Framework-Versionen (.NET 10).

## 5. Kosten-Nutzen-Analyse und Diskussion (ca. 7–9 Seiten)
*Ziel: Beantwortung der Forschungsfrage durch Zahlen und qualitative Argumente.*

* **5.1 Aufwandsmessung der ADSE-gestützten Entwicklung:**
    * Erfasste Netto-Entwicklungszeit (Menschliche Arbeitszeit + KI-Interaktion).
    * Monetäre Bewertung der Eigenleistung (kalkulatorischer Stundensatz).
* **5.2 TCO-Vergleichsrechnung (Make vs. Buy):**
    * Szenario A: SaaS-Abonnement-Kosten über 36 Monate.
    * Szenario B: Snowbridge-Entwicklung via ADSE (CAPEX) + Wartung/Hosting (OPEX).
    * Ermittlung des Break-Even-Points und Sensitivitätsanalyse.
* **5.3 Qualitative Nutzenbewertung:**
    * Strategischer Nutzen: Datenschutz (Data Residency), Unabhängigkeit, individuelle Anpassbarkeit.
    * Implikationen für den Einsatz von ADSE in vergleichbaren ETL-Projekten.

## 6. Fazit und Ausblick (ca. 2–3 Seiten)
*Ziel: Zusammenfassung und Blick in die Zukunft.*

* **6.1 Zusammenfassung der Ergebnisse:** Welche Potenziale bietet ADSE für ETL-Eigenentwicklungen, und wann lohnt sich der Make-Ansatz?
* **6.2 Kritische Reflexion:** Grenzen der Studie und Übertragbarkeit der Ergebnisse.
* **6.3 Ausblick:** Weiterentwicklung von Snowbridge und zukünftige Forschungsfragen zu ADSE im Data Engineering.

# Winterdienst-Tool – Projektübersicht

## 1. Zweck

Das Tool organisiert den Winterdienst für den Königreichssaal an der **Hirblinger Str. 99, Augsburg**.

Ziele:

- offene Winterdienst-Tage sichtbar machen
- Verantwortliche und Helfer koordinieren
- bei Schnee/Eis schnell erkennen, welche Bereiche bereits abgedeckt sind
- Helfern eine einfache mobile Ansicht bieten
- Mehrsprachigkeit für die beteiligten Versammlungen
- Wetterlage und Einsatzplanung zusammenführen
- private Kalenderübernahme ermöglichen
- Admin-Verwaltung direkt im Tool ermöglichen

Das Tool ist **mobile-first** aufgebaut und wird als einzelne HTML-Datei über GitHub Pages bereitgestellt.

---

## 2. Aktueller Stand

**Aktuelle Frontend-Version:** `v0.10.4`

Aktuelle Datei:

```text
winterdienst-v.0.10.4.html
```

Die Versionsnummer wird im Footer angezeigt.  
Der Seitentitel selbst bleibt versionsfrei.

---

## 3. Technische Architektur

### Frontend

- Single-File-HTML
- kein Build-Prozess erforderlich
- für GitHub Pages geeignet
- responsives Layout
- lokale Speicherung der Sitzung und der eigenen Person auf dem Gerät

### Backend

Backend und Datenhaltung laufen über **Supabase**.

Projekt:

```text
kaffeetrinker80
Project Ref: avuimpwjslrgpdahyloa
Region: eu-central-1
```

Supabase URL:

```text
https://avuimpwjslrgpdahyloa.supabase.co
```

API / Edge Function:

```text
https://avuimpwjslrgpdahyloa.supabase.co/functions/v1/winter-service-api
```

Aktuelle Edge-Function-Version: **v6**

Die Browser-App greift **nicht direkt auf die Tabellen** zu.  
Alle relevanten Aktionen laufen über die Edge Function.

---

## 4. Sicherheit und Login

Es gibt zwei Zugriffsebenen:

- normaler Nutzer / Helfer
- Admin

Die Anmeldung erfolgt über einen gemeinsamen Zugang bzw. Admin-Zugang.

Wichtig:

- Zugangsdaten liegen nicht als Klartext im Frontend
- Geheimnisse werden serverseitig geprüft
- Speicherung in Supabase erfolgt als PBKDF2-SHA256-Hash
- Sitzungen werden als eigene Session-Tokens verwaltet
- Session-Laufzeit derzeit: 14 Tage
- Row Level Security ist auf den Winterdienst-Tabellen aktiviert
- Browser-Zugriff auf die Tabellen selbst ist entzogen
- Zugriff erfolgt über die Edge Function mit Server-Berechtigung

Aus Sicherheitsgründen gehören die konkreten PIN-/Passwortwerte **nicht in diese Projektdokumentation**.

---

## 5. Sprachen

Das Tool unterstützt derzeit:

- Deutsch
- Englisch
- Russisch
- Italienisch
- Kroatisch / Serbisch

Sprachcodes:

```text
de
en
ru
it
hr
```

Nach dem Login steht oben ein kompakter Sprachwähler zur Verfügung.

Auch die Hallenbezeichnung wird übersetzt:

- DE: Königreichssaal
- EN: Kingdom Hall
- RU: Зал Царства
- IT: Sala del Regno
- HR: Dvorana Kraljevstva

---

## 6. Kopfbereich

Aktueller Haupttitel:

```text
Winterdienst Königreichssaal
```

Darunter:

```text
Hirblinger Str. 99 · Augsburg
```

Auf kleinen Displays wird die Titelschrift reduziert, damit der Titel möglichst in einer Zeile bleibt.

---

## 7. Wetter

Die Wetterdaten werden über **Open-Meteo** geladen.

Aktuell werden u. a. verwendet:

- aktuelle Temperatur
- Tagesminimum
- Tagesmaximum
- Niederschlag
- Schneefall

Die Wetterlage wird vereinfacht bewertet:

- Rot: deutlicher Schneefall bzw. Frost + Niederschlag
- Warnung: Schnee oder Frost
- Beobachtung: Temperatur nahe dem Gefrierpunkt
- niedriges Risiko: keine auffällige Lage

Die Wetteranzeige steht auf der Startseite im Bereich **Heute**.

---

## 8. Aktive Winterdienst-Monate

Die Winterdienst-Saison ist in Supabase hinterlegt.

Ursprüngliche Saison:

```text
01.11.2026 – 31.03.2027
```

Admins können festlegen, welche Monate aktuell im Tool sichtbar bzw. aktiv sind.

Standardmäßig sind die Wintermonate November bis März vorgesehen.

---

## 9. Startseite

Die Startseite ist bewusst einfach gehalten.

Reihenfolge:

1. Heute + Wetter
2. Meine Einsätze
3. Bereiche & Räumplan
4. heutiger Winterdienst
5. weitere Navigation über die unteren Tabs

Die Bereichsreferenz kann kompakt ein- und ausgeklappt werden.

---

## 10. Navigation

Hauptansichten:

- **Heute**
- **Offen**
- **Plan**

### Heute

Zeigt:

- Wetter
- heutigen Status
- Verantwortlichen
- Helfer
- Bereichsabdeckung

### Offen

Zeigt noch nicht übernommene reguläre Winterdienst-Tage.

### Plan

Zeigt den kompletten aktiven Saisonplan gruppiert nach Monaten.

---

## 11. Monatsübersicht

Jeder Monat kann zusätzlich als kompakter Kalender geöffnet werden.

Funktion:

```text
▦ Monatsübersicht
```

Im Monats-Modal sieht man alle Tage des Monats auf einen Blick.

Darstellung u. a.:

- besetzte Tage
- offene reguläre Tage
- FSG-/Großreinigungstage
- Vormittagsversammlungen
- weitere Sonderabdeckungen

Zusätzlich gibt es:

```text
← vorheriger aktiver Monat
→ nächster aktiver Monat
```

Die Pfeile springen nur innerhalb der aktiven Winterdienst-Monate.

Als Admin kann man aus der Monatsübersicht direkt einen Tag bearbeiten.

Normale Nutzer können offene Tage direkt übernehmen.

---

## 12. Verantwortliche und Helfer

### Hauptverantwortlicher

Bei einem regulären, noch offenen Tag kann eine Person die Verantwortung übernehmen.

Dann gilt:

- der Tag ist nicht mehr offen
- weitere Personen können sich als Helfer eintragen
- Bereichswünsche der Helfer werden sichtbar

### Helfer

Helfer können sich einem bereits übernommenen Tag anschließen.

Beim Eintragen wählen sie einen bevorzugten Bereich:

- Rot
- Blau
- Grün
- Flexibel

Der Bereich kann später geändert werden.

---

## 13. Räumbereiche

Die Einteilung orientiert sich am vorhandenen Schneeräum-Konzept.

### 🔴 Rot – Priorität

Gehwege auf dem Grundstück.

Aufgabe:

- räumen
- enteisen
- oder trittsicher streuen

Dieser Bereich hat Priorität.

### 🔵 Blau – öffentlicher Bereich

Gehwege entlang des Grundstücks.

Aufgabe:

- räumen
- enteisen

### 🟢 Grün – Zusatzbereich

Nur wenn genügend Helfer vorhanden sind.

Dazu gehören insbesondere:

- Zufahrt
- schmale Zugänge
- zusätzliche Flächen

### ⚪ Flexibel

Helfer ohne festen Bereich.

Sie unterstützen dort, wo noch Bedarf besteht.

---

## 14. Sichtbare Bereichsabdeckung

Sobald ein Hauptverantwortlicher eingetragen ist, zeigt das Tool sichtbar an:

- wer für Rot gemeldet ist
- wer für Blau gemeldet ist
- wer für Grün gemeldet ist
- wer flexibel hilft

So lässt sich z. B. sofort erkennen:

```text
Rot: 4 Helfer
Blau: noch offen
Grün: 1 Helfer
Flexibel: 1 Helfer
```

Rot und Blau können deutlich als noch unterstützungsbedürftig markiert werden.

Grün bleibt ein zusätzlicher Bereich, der erst bei ausreichend Helfern relevant wird.

---

## 15. Räumplan-Bilder

Für jede Sprache gibt es ein eigenes Bild.

Dateinamen:

```text
snow-plan-de.jpg
snow-plan-en.jpg
snow-plan-ru.jpg
snow-plan-it.jpg
snow-plan-hr.jpg
```

Die Bilder liegen im selben Repository wie die HTML-Datei.

Das Tool lädt automatisch das Bild passend zur gewählten Sprache.

Der Räumplan ist über die Bereichsreferenz erreichbar.

---

## 16. Meine Einsätze

Nach erfolgreicher Identifikation zeigt das Tool den Bereich:

```text
Meine Einsätze
```

Dort sieht die Person alle eigenen Termine innerhalb der aktiven Saison.

Unterschieden wird zwischen:

- Verantwortlicher
- Helfer

Bei Helfern wird zusätzlich der gewählte Bereich angezeigt.

Mögliche Aktionen:

- Bereich ändern
- Zusage als Verantwortlicher zurücknehmen
- eigenen Termin in den privaten Kalender übernehmen

---

## 17. Kalenderexport

Für eigene Termine gibt es:

```text
📅 In meinen Kalender
```

Das Tool erzeugt eine `.ics`-Datei.

Damit funktioniert die Übernahme unabhängig vom verwendeten Kalenderprogramm, z. B.:

- Google Calendar
- Apple Calendar
- Outlook
- andere ICS-kompatible Kalender

Enthalten sind:

- Datum
- Winterdienst-Titel
- Adresse
- Rolle
- ggf. gewählter Bereich

Der Termin wird derzeit als Ganztagestermin exportiert.

---

## 18. Wochenend- und Gruppenlogik

Wochenenden sind **nicht mehr als starre Regel fest verdrahtet**.

Admins können pro Tag auswählen:

- regulärer Winterdienst
- FSG / Großreinigung
- Vormittagsversammlungen
- andere Gruppe
- Sonderabdeckung

### Samstag

Typischerweise kann eine Dienstgruppe eingetragen sein, die ohnehin für die Großreinigung vorgesehen ist.

Deutsche Bezeichnung:

```text
Dienstgruppen · Großreinigung
```

Englische Bezeichnung:

```text
Field Service Groups (FSGs) · Hall cleaning
```

Wichtig: **FSG bedeutet Field Service Group**, nicht Facility Service Group.

### Sonntag

Typische Abdeckung:

```text
Vormittagsversammlungen
```

bzw. englisch:

```text
Morning congregations
```

---

## 19. Personen

Beim ersten normalen Login auf einem Gerät fragt das Tool nach:

- Name
- Versammlung

Alternativ kann eine bereits vorhandene Person ausgewählt werden.

Aktuell hinterlegte Versammlungen:

```text
Augsburg-Oberhausen
Neusäß-Nord
Neusäß-Süd
Augsburg-English
Augsburg-Italienisch
Augsburg-Kroatisch/Serbisch
Augsburg-Russisch-Nord
```

Die gewählte Person wird lokal auf dem Gerät gespeichert.

---

## 20. Admin-Funktionen

Admins können derzeit u. a.:

- Personen anlegen
- Personen aktiv / inaktiv setzen
- aktive Winterdienst-Monate definieren
- einzelne Tage bearbeiten
- reguläre Tage als offen setzen
- Verantwortliche zuweisen
- Gruppenabdeckung festlegen
- FSG-/Großreinigungstage setzen
- Vormittagsversammlungen setzen
- Sonderabdeckungen eintragen
- Notizen hinterlegen

---

## 21. Supabase-Tabellen

### `winter_people`

Speichert Personen.

Wichtige Felder:

- `id`
- `name`
- `congregation`
- `active`
- `created_at`

### `winter_assignments`

Speichert Winterdienst-Tage.

Wichtige Felder:

- `service_date`
- `responsible_person_id`
- `coverage_type`
- `coverage_label`
- `status`
- `note`
- `completed_at`

Mögliche `coverage_type`-Werte:

```text
regular
cleaning_team
morning_congregations
group
special
```

### `winter_helpers`

Speichert Helfer zu einem Einsatz.

Zusätzlich:

```text
area_preference
```

Mögliche Werte:

```text
red
blue
green
flexible
```

### `winter_service_log`

Protokolliert durchgeführte Aktionen.

Mögliche Aktionen:

```text
check
no_action
salted
snow_cleared
completed
note
```

### `winter_settings`

Speichert Einstellungen wie:

- Standort
- aktive Sprachen
- Saison
- aktive Monate
- allgemeine Konfiguration

### `winter_access_secrets`

Speichert gehashte Zugangsdaten.

### `winter_sessions`

Speichert aktive Sitzungen.

---

## 22. Edge-Function-Aktionen

Die Edge Function `winter-service-api` unterstützt u. a.:

```text
login
logout
bootstrap
register_person
take_responsibility
release_responsibility
join_helper
leave_helper
update_helper_area
log_service
admin_save_person
admin_update_assignment
admin_update_settings
admin_set_person_active
admin_assign
```

`bootstrap` liefert die benötigten Daten für die App:

- Einsätze
- Verantwortliche
- Helfer
- Bereichswünsche
- Personen
- Einstellungen
- Zugriffsebene

---

## 23. WhatsApp

Es gibt einen Link zur Winterdienst-WhatsApp-Gruppe.

Das Tool kann bei relevanter Wetterlage bzw. mehreren offenen Einsätzen einen Hinweis zur Abstimmung über WhatsApp anzeigen.

Die WhatsApp-Gruppe dient der kurzfristigen Koordination, während das Tool die eigentliche Planung und Übersicht übernimmt.

---

## 24. Grundgedanke der Bedienung

Das Tool soll nicht zu einer komplizierten Einsatzverwaltung werden.

Die gewünschte Grundlogik ist:

1. Wetter prüfen
2. offenen Tag übernehmen
3. Helfer tragen sich ein
4. Helfer wählen ihren bevorzugten Bereich
5. alle sehen sofort, welcher Bereich noch Unterstützung braucht
6. bei Bedarf Räumplan öffnen
7. eigenen Einsatz in den privaten Kalender übernehmen
8. kurzfristige Abstimmung bei Bedarf über WhatsApp

---

## 25. Noch mögliche Weiterentwicklungen

Sinnvolle, aber derzeit nicht zwingende Erweiterungen:

- Monatsübersicht zusätzlich mit Zähler:
  - `x offen`
  - `y besetzt`
- direkter Ausstieg eines Helfers über „Meine Einsätze“
- sichtbare Mindestbesetzung je Bereich
- Wetterwarnung stärker mit einzelnen Einsatztagen verbinden
- Admin-Auswertung über Saison / durchgeführte Einsätze
- optionaler „Winterdienst erledigt“-Status für den Hauptverantwortlichen
- Erinnerungsfunktion vor einem übernommenen Termin
- bessere Duplicate-Erkennung bei gleichen Personennamen

Diese Punkte sind bewusst noch nicht Teil der Kernlogik, damit das Tool übersichtlich bleibt.

---

## 26. Dateistruktur im Repository

Beispiel:

```text
winterdienst-v.0.10.4.html
snow-plan-de.jpg
snow-plan-en.jpg
snow-plan-ru.jpg
snow-plan-it.jpg
snow-plan-hr.jpg
```

Die Bilddateien müssen unter dem relativen Pfad erreichbar sein, den die HTML-Datei verwendet.

---

## 27. Projektstatus

Das Tool befindet sich weiterhin in der **0.x-Phase**.

`0.10.4` bedeutet daher nicht `1.0`.

Ein sinnvoller Sprung auf **Version 1.0** wäre erreicht, wenn:

- Kernfunktionen stabil sind
- mobile Ansicht zuverlässig funktioniert
- Saisonplanung getestet ist
- Verantwortliche/Helfer/Bereiche sauber zusammenspielen
- Admin-Bearbeitung stabil läuft
- keine wesentlichen Bedienprobleme mehr bestehen

Bis dahin bleibt die Versionierung bewusst unter `1.0`.

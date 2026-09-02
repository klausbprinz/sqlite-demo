---
marp: true
theme: default
paginate: true
footer: "Klaus Prinz | BIM25 - WIT"
style: |
  section {
    font-size: 24px;
  }
  table {
    font-size: 20px;
  }
---

![width:250px](../assets/graphics/330px-SQLite370.svg.png)
# als lokales Datenbanksystem  

**Klaus Prinz** | Datenbanken - BIM25 - WIT

<br>



---

## Agenda

> Ein kompakter Überblick von architektonischen Grundlagen zur praktischen Anwendung in Python.

* **Grundlagen & Konzepte:** Was ist SQLite, welche Ideen drückt es aus, wofür ist es (nicht) geeignet, wie unterscheidet es sich von Flat Files und Client-Server-DBs?
* **SQLite und Python:** Wie könnte man SQLite mit Python verwenden, welche Vorteile bietet die Verbindung mit Pandas für die Datenanalyse, was sind ORMs und wie nutzt man sie?
* **Live-Demo:** Hands-On in einem Jupyter Notebook.

---

## Wesentliches

> SQLite ist das am häufigsten installierte Datenbanksystem der Welt – unsichtbar integriert in Smartphones, Browser und Betriebssysteme.

**Zentrale Eigenschaften:**
* **Serverless:** Kein separater Datenbankserver (wie PostgreSQL oder MySQL) notwendig.
* **Self-Contained:** Gesamte Datenbank besteht aus genau **einer einzigen Datei**.
* **Zero-Configuration:** Kein Setup, kein "Installieren und Warten", läuft sofort "Out-of-the-Box".
* **Public Domain:** Quellcode ist frei von Urheberrechtsansprüchen und für jeden Zweck frei nutzbar.

---

## Entwicklung und Idee

> Ein relationales Datenbanksystem, das ohne Datenbankadministrator (DBA) und ohne administrative Hürden lokal betrieben werden kann.

* **Entwicklung:** Im Jahr 2000 von D. Richard Hipp (ursprünglich für US-Kriegsschiffe zur Fehlertoleranz bei Systemausfällen).
* **Kernidee:** Statt eines großen, zentralen Servers, mit dem man über das Netzwerk kommuniziert, wandert die gesamte Datenbank als **selbstständige Datei direkt auf die Festplatte**.

---

## Datenbehausungen
### Flat Files vs. SQLite

> Man skriptet schnell etwas, speichert Daten einfach in JSON- oder CSV-Dateien - und irgendwann wird es unübersichtlich.

<br>

| CSV / JSON / Excel | SQLite (Datenbank) |
| :--- | :--- |
| **Laden ganzer Dateien** in den Arbeitsspeicher | **Gezielte Abfragen** dank Indizierung (`SELECT` mit `WHERE`) |
| **Keine Datentyp-Integrität** (Dateien korrumpieren leicht) | **ACID-konform** (Transaktionssicherheit & definierte Typen) |
| Keine Mechanismen bei **gleichzeitigen Schreibvorgängen** | **Atomares File-Locking** (Concurrency Control) verhindert Datenverlust |

---

## Einsatzgebiete und Eignungen
### SQLite vs. Client-Server-DBs (PostgreSQL / MySQL)

> Ist SQLite jetzt die Lösung für alles, oder?

<br>

| SQLite (Lokal & Serverless) | Client-Server-DB (PostgreSQL / MySQL) |
| :--- | :--- |
| **Lokale Prototypen, Skripte & Jupyter Notebooks** | **Multi-User Webanwendungen** mit hoher Last |
| **Geringe Komplexität** (keine Server-Administration) | **Feingranulare Benutzerrechte** (ACLs pro Zeile/Tabelle) |
| **Einzelne Datei** auf der Festplatte / lokal | **Zentraler Datenbank-Server** über Netzwerk |
| **Grenze:** Hohe gleichzeitige Schreiblast (Write-Locking) | **Skalierbar** bei parallelen Schreibvorgängen |

---

## SQLite in Python
### Der integrierte Standardweg

> Python bringt alles mit, was man braucht: Das Modul `sqlite3` ist standardmäßig in der Python-Standardbibliothek enthalten.

* **Keine externen Treiber** oder pip-Installationen für den Einstieg nötig.
* **Typischer Ablauf:** Verbindung herstellen (`.connect()`), Cursor erzeugen (`.cursor()`), SQL-Befehl ausführen (`.execute()`), Änderungen bestätigen (`.commit()`), Verbindung schließen(`.close()`).

---

## Kurzer Blick unter die Haube
### Code-Beispiel: Verbindung & Abfrage

```python
import sqlite3

# establish connection (file is created if it does not exist yet)
conn = sqlite3.connect("data/library.db")

# obtain cursor handle
cursor = conn.cursor()

# execute SQL query
cursor.execute("SELECT title, author FROM books WHERE year > 2020;")

# fetch and output results
for row in cursor.fetchall():
    print(f"Titel: {row[0]}, Autor: {row[1]}")

# close connection
conn.close()
```

---

## Brücke zur Datenanalyse
### SQLite + Pandas = Jupyter Playground

> Daten direkt aus der Datenbank in einen Pandas DataFrame lesen und wieder in die Datenbank schreiben – mit zwei Zeilen Code.

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect("data/library.db")

# read SQL query directly into DataFrame
resultDF = pd.read_sql("SELECT * FROM books WHERE year > 2020", conn)

# persist DataFrame in database as table
anotherDF.to_sql("books", conn, if_exists="replace", index=False)
```
* **Ideal für Datenanalyse:** Große Metadatensätze (z. B. MARC-Records oder Katalog-Exports als CSV) einlesen, lokal bereinigen, in SQLite persistieren und interaktiv in Jupyter Notebooks analysieren.

---

## Ein wenig ORM: SQLAlchemy
### Vom rohen SQL zur objektorientierten Welt

> Wer sind diese Object-Relational Mapper (ORMs) und warum sollte man sie nutzen?

* **Das Prinzip:** Datenbank-Logik wird in Python-Logik übersetzt. Tabellen werden als Python-Klassen abgebildet; Zeilen entsprechen konkreten Objekten.
* **Vorteile:** 
  * Agnostisches Interface für verschiedene Datenbank-Anbieter.
  * Unabhängigkeit vom konkreten SQL-Dialekt.
  * Typsicherheit und automatisiertes Mapping.
* **Beispiel:** Statt `cursor.execute("SELECT ...")` arbeitet man direkt mit logischen Objekten wie `session.query(Book).filter(...)`.

---

## Nächster Schritt: Live-Demo!
### Hands-On in einem Jupyter Notebook

> **Praxisumgebung:** Start in VS Code mit Extensions: `Python`, `Juypter`, `SQLite Viewer`.

* **Inhalt der Demo:**
  1. Working with `sqlite3`
  2. `Pandas` Integration
  3. Object-Relational Mapping (ORM) with `SQLAlchemy`

---
## Quellen & Weiterführende Links
### Offizielle Dokumentationen

* **SQLite:** ([sqlite.org](https://sqlite.org)).
* **Python `sqlite3`-Modul:** ([docs.python.org](https://docs.python.org/3/library/sqlite3.html)).
* **Pandas:** ([pandas.pydata.org](https://pandas.pydata.org/)).
* **SQLAlchemy:** ([sqlalchemy.org](https://www.sqlalchemy.org/)).

### Datenanalyse-Kurs
* **freeCodeCamp:** ([freecodecamp.org/learn/data-analysis-with-python](https://www.freecodecamp.org/learn/data-analysis-with-python/)).

---

## Vielen Dank für eure Aufmerksamkeit!
### Fragen & Diskussion

<br>

> **Materialien & Code-Beispiele:**
> 🔗 [https://github.com/klausbprinz/sqlite-demo](https://github.com/klausbprinz/sqlite-demo)

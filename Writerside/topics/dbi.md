# UE306

## Anforderungsphase

Ich habe für diese Aufgabe den Entitätstypen Aufgabe <br></br>
**Attribute:** Name, Beschreibung, Startdatum, Fälligkeitsdatum, Status <br></br>
**Beschreibung:** Benutzer können Aufgaben für Projekte erstellen und verwalten, indem sie Informationen wie den Namen,
die Beschreibung, das Start- und Fälligkeitsdatum sowie den Status angeben <br></br>

## Entwurfsphase

<img  src="images/img.png"  alt="Er Diagramm"/>

**Datenkatalog:** <br></br>
Aufgaben ID: Eindeutige Kennung für jede Aufgabe (Primärschlüssel).<br>
Name: Der Name oder die Bezeichnung der Aufgabe.<br>
Beschreibung: Eine detaillierte Beschreibung der Aufgabe.<br>
Start: Das geplante Startdatum der Aufgabe.<br>
Fällig: Das geplante Enddatum der Aufgabe.<br>
Status: Der aktuelle Status der Aufgabe (z. B. geplant, in Bearbeitung, abgeschlossen).<br>

## Implementierung

**Erstellung eines Tables:**

```SQL
    drop table if exists ue306.aufgabe
    go

    drop schema if exists ue306
    go

    create schema ue306
    go

    create table ue306.aufgabe
    (
    AufgabenID   INT PRIMARY KEY IDENTITY (1,1),
    Name         VARCHAR(50),
    Beschreibung varchar(max),
    Startdatum   DATE,
    Enddatum     DATE,
    Status       VARCHAR(14)
    );
```

Dannach muss man test daten erstellen ich habe sie mit [mockaroo](https://www.mockaroo.com/) erstellt. <br>
Ein screenshot mit den eingaben für meine daten:

<img src="mockaroo.png" alt="Mockaroo"/>

## Abfragen

### Abfrage 1

wie viele Aufgaben sind in Bearbeitung?

```SQL
    select count(status) as inBearbeitung from ue306.aufgabe
    where Status = 'in Bearbeitung'
```

### Abfrage 2

welche aufgaben haben 2021 angefangen?

```SQL
    select name,Startdatum  from ue306.aufgabe as Aufgaben2021
    where YEAR(Startdatum) = 2021

```

### Abfrage 3

Welche Aufgaben haben ein geplantes Enddatum zwischen dem 1. Januar 2023 und dem 31. März 2023?

```SQL
    SELECT * FROM ue306.aufgabe
    WHERE Enddatum BETWEEN '2023-01-01' AND '2023-03-31';
```

### Abfrage 4

welche Aufgaben sind überfällig

```SQL
    select * from ue306.aufgabe
    where Enddatum < GETDATE()
    AND Status = 'geplant' or Status = 'in Bearbeitung
```

### Abfrage 5

wie viele projkte haben kein start datum?

```SQL
    select count(Startdatum) as NullAufgaben from ue306.aufgabe
    where Startdatum IS NULL;
```

### Abfrage 6

Es werden alle Aufgaben zurückgegeben mit einem bestimmten namen die nicht abgeschlossen werden und sie werden von alt
nach neu sotiert

```SQL
    SELECT *
    FROM ue306.aufgabe
    WHERE Name LIKE '%Livetube%' AND NOT Status = 'abgeschlossen'
    ORDER BY Startdatum;
```

### Abfrage 7

Gibt den namen und Anzahl zurück der aufgabe welche am häufigsten vorkommen

```SQL
    SELECT Name, COUNT(*) AS Anzahl
    FROM ue306.aufgabe
    GROUP BY Name
    HAVING COUNT(*) = (SELECT TOP 1 COUNT(*)
                     FROM ue306.aufgabe
                     GROUP BY Name
                     ORDER BY COUNT(*) DESC)
    ORDER BY Anzahl DESC;
```

## Update und Löschen

Erstellt ein SQL-Statement, mit dem ihr gezielt die Eigenschaften einer Entität in der Datenbank verändert.
```SQL
    UPDATE ue306.aufgabe
    SET Name = 'test'
    where AufgabenID = 2
```
Erstellt ein SQL-Statement, mit dem ihr gezielt die Eigenschaften mehrerer Entitäten in der Datenbankverändert.
```SQL
    UPDATE ue306.aufgabe
    SET Status = 'geplant'
    WHERE Enddatum < GETDATE()
```
Erstellt ein SQL-Statement, mit dem ihr gezielt einen Datensatz aus der Datenbank löscht.
```SQL
    DELETE FROM ue306.aufgabe
    where AufgabenID = 2
```
Erstellt ein SQL-Statement, mit dem ihr gezielt mehrere Datensätze aus der Datenbank löscht.
```SQL
    DELETE FROM ue306.aufgabe
    where AufgabenID = 2
```

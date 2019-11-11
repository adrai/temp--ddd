# Domain-Driven Design (DDD)

Domain steht im Fokus

- Fachlichkeit
- Fachexpertise
- "Thema, um das es geht"
- Diskursbereich

2003: Eric Evans
  "Domain-Driven Design: Tackling Complexity at the Heart of Software"
  aka "das Blue Book"

2013: Vaughn Vernon
  "Implementing Domain-Driven Design"
  aka "das Red Book"

20xx: Vaughn Vernon
  "Domain-Driven Design Distilled" ("DDD kompakt")

Interdisziplinäre Teams
  - Unterschiedliche "Sprachen", unterschiedliches Vokabular
  - "Language Gap"
  - Das Ziel von DDD ist es, den Language Gap zu überbrücken
    - Damit die Leute innerhalb eines Teams sich besser verstehen
    - Ziel: Gemeinsame Sprache

Top-Down:
- Vision, große Ganze, "was" überhaupt gemacht werden soll
- "Strategische Design"

Bottom-Up:
- Details, kleine Bausteine, "wie" Dinge gemacht werden
- "Taktische Design"

## Taktisches Design

        "Commands"            -->             "Domain Events"
Absicht, Intention, Wünsche         Fakt, Historisches Ereignis, unwiderruflich
Imperativ: Verb + Substantiv        Vergangenheitsform: Substantiv + Verb

    "Öffne Datei"                       "Datei wurde geöffnet"
    "Sende E-Mail"                      "E-Mail wurde gesendet"
    "Drucke Dokument"                   "Dokument wurde gedruckt"
    "Starte Motor"                      "Motor wurde gestartet"

                      Commands : Domain Events
                             m : n


                              State
                                |
                                v
        Client -> Command -> Command-Handler -> Domain Events

```javascript
const commandHandler = function (gameState, makeGuessCommand) {
  // ...

  return [
    new GuessMadeDomainEvent({ ... }),
    new LevelSucceededDomainEvent({ ... })
  ];
};
```

                                 State
                               /       ^
                              /         \
                             v           \
  Command -----> Command-Handler -----> Domain Events


### State

Repräsentiert den aktuellen Zustand
- Konsistent

Sehr groß:
- Einfach zu verwalten
- Gefahr von Data Races
- Commands sequenzialisieren
  - Performance / Parallelisierung leidet

Sehr klein:
- Aufwändig zu verwalten
- Data Races im Kleinen sind "weg"
- Dafür: Was machen bei State-Objekt übergreifenden Vorgängen?
  - Performance / Parallelisierung okay

Gesucht: Mittelweg

         Aggregate
+----------------------------+
|         State              |
|        /     \             |
| Commands --- Domain Events |
+----------------------------+

Struktur eines Commands           Struktur eines Domain Events
  - Aggregate-Typ ("Spiel")         - Aggregate-Typ ("Spiel")
  - Aggregate-Instanz ("4711")      - Aggregate-Instanz ("4711")
  - Name ("Äußere Vermutung")       - Name ("Vermutung wurde geäußert")
  - Payload (vermutung: "3")        - Payload (nächstesLevel: "3")


### Alternative Aggregates-Erklärung

Entity
- Hat (künstliche) Identität
- Daten können sich ändern

```typescript
class User {
  public readonly id: string;
  public password: string;
  public address: Address;
}
```

Value Objects
- Hat keine Identität
- Daten können sich nicht ändern

```typescript
class Address {
  public street: string;
  public zipCode: string;
  public city: string;
  public country: string;
}
```

Aggregate =
  Entities (*ein* Entity davon ist das Aggregate Root)
    Commands
  Value Objects

## Strategisches Design

Gemeinsame Sprache !== Universal Language
- Weil: Konflikte in der Terminologie sind vorprogrammiert

Doppelt belegte Begriffe können bedingt sein durch:
- Abteilungen
- Rollen
- Personen
- ...

=> Perspektiven

Die gemeinsame Sprache gilt nur innerhalb von Teilbereichen
Ein solcher Teilbereich heißt "Bounded Context"

Die gemeinsame Sprache innerhalb eines Bounded Contexts heißt:
- *Ubiquitous Language*

### Die DDD-Hierarchie

- Domain
  - Subdomains (fachliche Gliederung)
    - Core Domain
    - Supporting Domains
      - Generic subdomains
  - Bounded Contexts / Ubiquitous Language (sprachliche Gliederung)
    - Aggregates
      - State
      - Commands
      - Domain Events

## Saga / Process Manager / (Workflow, Flow, ...)

              State
                |
Commands -> Aggregates -> Domain Events

If-this-then-that (IFTTT):
- Domain Events -> Process Manager -> Commands

Stateful process managers:

                      State
                        ^
                        |
- Domain Events -> Process Manager -> Commands

## Sonstiges

- Shared Kernel
  - Gemeinsam genutzte Datenstrukturen über eine ausgelagerte Bibliothek, die von mehreren Stellen geladen werden kann
- Anti-Corruption Layer
  - Adapter / Fassade, um "korrupte" Dienste von den Verwendern sauber zu trennen, so dass beide unabhängig voneinander evolvieren können
- Repository
  - `loadAggregateById`
  - `saveAggregate` (= 1 Transaktion)

# CQRS

CQS = Command Query Separation
- Design-Pattern für OOP

```javascript
stack.push(42);   // Ändert Zustand, gibt nichts zurück: Command
stack.top();      // Ändert Zustand nicht, gibt etwas zurück: Query
stack.pop();      // Ändert Zustand, gibt etwas zurück: Command + Query (!!!)
```


CQRS = CQS auf Applikationsebene
- Command Query Responsibility Segregation
- Greg Young

             API (Write) - Datenbank (Write)
           /                  |
        UI                    | (AP => Eventual Consistent)
           \                  v
             API (Read) -- Datenbank (Read)


## CAP-Theorem

      Partition Tolerance
            /     \
           /       \
          /         \
Consistency ------- Availability

CP = Consistency > Availability
AP = Availability > Consistency

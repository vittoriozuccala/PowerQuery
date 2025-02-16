# PowerQuery
Questo repository contiene diversi esempi e spunti per l'ambiente *PowerQuery*

Inizio delle informazioni

Arrivato ad Chapter 3 page 64

## Estrarre da una lista di colonne solo le non date
Se ho una lista di colonne tipo:
- Product
- Color
- 31-Jan-22
- 28-Feb-22

```sql
= List.Select(
    Table.ColumnNames(ChangedType),
    each (try Date.From(_))[HasError]
)

-- Il contrario basta mettere un not
= List.Select(
    Table.ColumnNames(ChangedType),
    each not (try Date.From(_))[HasError]
)

-- Per vedere l'errore basta sostituire il Select con Transform
= List.Transform(
    Table.ColumnNames(ChangedType),
    each not (try Date.From(_))
)

```

## Skippare una lista in base al tipo di dato
Per non avere un numero hard coded all'interno del codice, invece di usare
```sql
= List.Skip (Record.ToList (_), 2)
```

è possibile dirgli di skippare finchè non trovi un numero:

```sql
= List.Skip (Record.ToList (_), each not ( _ is number)
```
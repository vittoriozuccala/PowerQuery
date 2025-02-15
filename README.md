# PowerQuery
Questo repository contiene diversi esempi e spunti per l'ambiente *PowerQuery*
Arrivto ad Example 2 page 39

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
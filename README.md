# PowerQuery 

Questo repository contiene diversi esempi e spunti per l'ambiente *PowerQuery*

Inizio delle informazioni

Arrivato ad Chapter 3 page 64

## Chapter 01: Listes
### Estrarre da una lista di colonne solo le non date
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

## Chapter 02: Records
### Skippare una lista in base al tipo di dato
Per non avere un numero hard coded all'interno del codice, invece di usare
```sql
= List.Skip (Record.ToList (_), 2)
```

è possibile dirgli di skippare finchè non trovi un numero:

```sql
= List.Skip (Record.ToList (_), each not ( _ is number)
```

## Chapter 03: Table
### Table.Schema
E' possibile avere lo schema delle tabelle inteso come *nome campi*, *tipi di dato*, etc...

```vb
Table.Schema(Table1) = Table.Schema(Table2)
```

### Table.Profile
Per sapere se una colonna è interamente null e cmq per avere uno schema che dica il valore minimo, quello massimo, la deviazione sandard di ogni campo, è possibile utilizzare la funzione Table.Profile.

A quel punto posso eliminare le colonne vuote in maniera decisamente più corretta come:

```vb
= Table.SelectRows(
    Table.Profile([Column], [Count], [NullCount]),
    each [Count] = [NullCount]
)
```

Non è sempre utile prendere tutta la tabella, soprattutto se molto grande.
Eventualmente si può prendere come esempio, le prime 5000 righe

```vb
= Table.Profile (Table.FirstN (LargeTable, 5000))
```

## Chapter 04: Navigation
E' possibile usare {} per scegliere una riga e quindi un record oppure [] per scegliere una colonna sottoforma di lista.
Nel caso in cui non trovi il record potrebbe andare in errore:

```vb
= TableReference {[Column 1 = "value", Column 2 = "value"]} [Column4]
```

Per mitigare questo fenomeno basta inserire un punto interrogativo alla fine.
Questo mitiga il fatto che non trovi il record o la colonna.

```sql
// this returns an error since there are only 3 columns
#"Changed Type" {[Col1 = "B"]} [Col4]
// writing a ? at the end of column reference
// returns a null instead
#"Changed Type" {[Col1 = "B"]} [Col4] ?
// ? can be written after the lookup operator too.
#"Changed Type" {[Col1 = "NewValue"]} ?
// ? can also be written both after
// the column and row reference
#"Changed Type" {[Col1 = "NewValue"]} ? [Col4] ?
```

## Chapter 05: Manipulating between Lists, Records, Tables

# shoebox - A cash-flow accounting program

shoebox is a simple cash-flow accounting program that's just a bit more sophisticated than a check register. It's purpose is to provide projected cash flow and expenditures into the future, and provide the ability to 
perform what-if scenarios without actually having to modify the books themselves. There is no formal chart of account - just debits and credits. 

The data store will be a custom flat-file and integrated, the idea is to hold payees, payors, and transactions all in the same file, including redundancy, error checking, and error correction. 

Journaling

A file is built on a history of changes, with a "current state" reflecting the last change made. Rather that duplicate each record in full, only the record id, field, and value are journaled. 

table ord | record | change cnt | field ord | value | ...

To reduce space, fields are represented by an field ordinal in the scheme. The value is dependent on the field type.

Scheme

The scheme of various tables 

GUID (immutible)
field ordinal (based on inclusion in a set of books, but once set is immutable)
name
description
field_cnt
field ord, type, (default, range, max size, etc.)
...
```
create_date
update_date

```

A GUID in this context has no need to be globally unique, just unique within the dataset. So can be a simple counter, of which 16-bits should suffice (as I cannot imagine a dataset with more then 2^16 tables, 
actually 8-bits should do nicely, but since I can conceive of a dataset with more than 256 tables - leave it at 16-bits.)


Changesets

Meant to record alterations to existing records rather than reproduce entire records where only one datum has changed.
If all datum have changed, makes sense to just include the entire record, as identifying each individual column
would be more verbose. I suppose it would be that if the length of the serialization of changed columns exceeds the
length of the record, then just include the record, otherwise the fields.

The entries in a changeset are in chronological order - so multiple changes to the same field/record can be included in a changeset.

A changeset can be squashed to the minimum number of changes to achieve the end state of the changeset.

table_ord/MOD_VERB/record_ord/field_ord/new_value/field_ord/new_value/...

have a reserved field_ord (ALL_FIELD_ORD) to indicate the entire record

MOD_VERB/record_ord/ALL_FIELD_ORD/record/

File Format

To keep things very simple, there is only one file to hold the dataset.

```
SCHEME
TABLE 1 BASE
TABLE 1 JOURNAL
TABLE 2 BASE
TABLE 2 JOURNAL
...
```



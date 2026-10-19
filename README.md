# shoebox - A cash-flow accounting program

shoebox is a simple cash-flow accounting program that's just a bit more sophisticated than a check register. It's purpose is to provide projected cash flow and expenditures into the future, and provide the ability to perform what-if scenarios without actually having to modify the books themselves. There is no formal chart of account - just debits and credits. 

The data store will be a custom flat-file and integrated, the idea is to hold payees, payors, and transactions all in the same file, including redundancy, error checking, and error correction. 

Journaling

A file is built on a history of changes, with a "current state" reflecting the last change made. Rather that duplicate each record in full, only the record id, field, and value are journaled. 

table ord | record | change cnt | field ord | value | ...

To reduce space, fields are represented by an field ordinal in the scheme. The value is dependent on the field type.

Scheme

The scheme of various tables 

GUID (immutible)
ordinal (based on inclusion in a set of books, but once set is immutible)
name
description
field_cnt
field ord, type, (default, range, max size, etc.)
...

# Transactions



A - atomicity - Each transactions performed as a single atomic operation.\
C - consistency - Every completed transaction commits only consistent information.\
I - isolation - Parallel transactions shouldn't affect to transaction.\
D - durability - Committed changes must be save to db.\


Types of data inconsistency

* lost-update - when one blocks **change** at the same time all changes **except last** is lost.
* dirty read - **reading the data**, added or modified by transaction, which **won't be committed**
* non-repeatable read - within **one transaction** repeatable reading returns **different result**.
* phantom reads - within **one transaction** repeatable selecting returns **different sets of rows.**

\
Isolation levels

* Read uncommitted - only committed **updates no losing**, but reading can be dirty
* Read committed - transaction can **read** changes which **other** committed **transaction made** (versioning or blocking strategy)
* Repeatable read - **reading** transaction doesn't see changes other transaction made, other transactions **can not change** reading by first transaction data. Lock for reading.
* Serializable - completely isolated transactions

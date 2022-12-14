# Naming Conventions

## General Guidelines

1. Never use names with a leading numeric character.
2. Always choose meaningful and specific names.
3. Avoid using abbreviations unless the full name is excessively long.
4. Avoid long abbreviations. Abbreviations should be shorter than 5 characters.
5. Any abbreviations must be widely known and accepted. 
6. Create a glossary with all accepted abbreviations.
7. Never use keywords as names. A list of keywords may be found in the dictionary view `v$reserved_words`.
8. Avoid adding redundant or meaningless prefixes and suffixes to identifiers.<br/>Example: `create table emp_table`.
9. Always use the spoken language (German) for all objects in the application.
10. Always use the same names for elements with the same meaning.

## Naming Conventions for PL/SQL

In general, the Oracle Database is not case sensitive with names. A variable named personname is equal to one named PersonName, as well as to one named PERSONNAME. Some products (e.g. TMDA by Trivadis, APEX, OWB) put each name within double quotes (&quot;) so the Oracle Database will treat these names to be case sensitive. Using case sensitive variable names force developers to use double quotes for each reference to the variable. Our recommendation is to write all names in lowercase and to avoid double quoted identifiers.

A widely used convention is to follow a `{prefix}variablecontent{suffix}` pattern.

The following table shows a possible set of naming conventions. 

Identifier                   | Prefix | Data type | Suffix  | Example
---------------------------- | ------ | -------- | ------- | --------
Global Variable              | `g_`   | Number   |         | `g_num_version`
Local Variable               | `l_`   | Varchar2 |         | `l_var2_version`
Cursor                       | `cu_`  |          |         | `cu_employees`
Record                       | `r_`   |          |         | `r_employee`
Array / Table                | `t_`   |          |         | `t_employees`
Object                       | `o_`   |          |         | `o_employee`
Cursor Parameter             | `p_`   |          |         | `p_num_empno_oid`
In Parameter                 | `in_`  |          |         | `in_empno`
Out Parameter                | `out_` |          |         | `out_ename`
Record Type Definitions      | `r_`   |          | `_type` | `r_employee_type`
Array/Table Type Definitions | `t_`   |          | `_type` | `t_employees_type`
Exception                    | `e_`   |          |         | `e_employee_exists`
Constants                    | `c_`   |          |         | `c_empno`

Data type | Abbreviation 
--------  | ------------
Number    | `num`
Varchar2  | `var2`
Char      | `chr`
Boolean   | `bool`
Date      | `date`
Clob      | `clob`
Blob      | `blob`


## Database Object Naming Conventions

Never enclose object names (table names, column names, etc.) in double quotes to enforce mixed case or lower case object names in the data dictionary.

### Naming prefix

Scope | Prefix
----- | -------
TGD Apex Core Objects | not applicable
Labor | LAB_
Import-Tabls VIS | IMP_VOE_

#### TGD Protokoll

Use the prefix `PD_` for protocol definitions.

Example:

* `pd_protokoll`  
* `pd_protokoll_frage`  
* `pd_protokoll_frageblock`

### Table

Singular name of what is contained in the table.
Optionally prefixed by a project abbreviation. (see naming prefix)

Examples:

* `betrieb`
* `tierarzt`

### Mapping tables

Vor mapping tables for example mapping Data between scopes.

*Syntax*:
 ```sql
  MAP_[BASE_ENTITY]
 ```

Example:  
 Mapping `tierart` between LAB and TGD.
 ```sql
  MAP_TIERART

 ```
 
### Journaling tables

Are prefixed `J_` followed by the name of the base table.

*Syntax*:
```sql
 J_[BASE_ENTITY]
```

Example:
```sql
 J_PERSON
 J_BENUTZER
 J_BERECHTIGUNG
```
##### Snippet for creating journaling tables

<details>
    <summary>Snippet</summary>

```sql
    declare
          v_out_var2_sql_table clob;
          v_out_var2_sql_trigger clob;
    begin
      core_generator_pkg.refresh_jour_table(
          p_in_var2_table_name => 'ARZNEIMITTELANWENDUNG_XML_TAG',
          po_var2_sql           => v_out_var2_sql_table,
          po_var2_sql_trigger   => v_out_var2_sql_trigger
      );
      core_generator_pkg.print_clob_to_output (p_clob => v_out_var2_sql_table);
      core_generator_pkg.print_clob_to_output (p_clob => v_out_var2_sql_trigger);
    end;
```

</details>

### Column

Singular name of what is stored in the column (unless the column data type is a collection, in this case you use plural names). Add a describing comment for every column.

- Each tables get a unique prefix. Every column name starts with this unique prefix
- Primary keys end with the suffix `_OID`
- Flags are abbreviated with `KZ` (data type `char(1 char)`)


### Check Constraint

Table name followed by the column and the suffix `_chk`.

Syntax:
```sql
[TABLE]_[COLUMN without PREFIX]_CHK
```

Example:
```sql
BENUTZER_KZ_GELOESCHT_CHK
```

### DML / Instead of Trigger

Name of the object to which the trigger is assigned
DML_OP_Abbreviation
Suffix "_TRG"

Syntax:
```sql
[TABLE]_AUDIT_TRG
[TABLE]_JOURNALE_TRG
[TABLE]_[DML_OP_ABREVIATION]_TRG
```
[DML_OP_ABREVIATION]:

 - B: before
 - A: after
 - X: instead of 
 - I: insert
 - U: update
 - D: delete
 
Example: 
```sql
BETRIEB_AUDIT_TRG
BETRIEB_JOURNAL_TRG
BENUTZER_BIU_TRG
```


### Foreign Key Constraint

Table followed by referenced table abbreviation followed by a `_fk` and an optional number suffix.
Name of the referenced table, current table, suffix `_FK`.

Syntax:
```sql
 [REF_TABLE]_[CURRENT_TABLE]_FK
```
Example:

`REF_TABLE`: Benutzer  
`CURRENT_TABLE`: BENUTZER_BENUTZERROLLE  
`BENUTZER_BENUTZER_BENUTZERROLLE_FK`

### Function

Name is built from a verb followed by a noun in general. Nevertheless, it is not sensible to call a function `get_...` as a function always gets something.

The name of the function should answer the question “What is the outcome of the function?”

Optionally prefixed by a project abbreviation.

Example: `employee_by_id`

If more than one function provides the same outcome, you have to be more specific with the name.

### Index

Indexes serving a constraint (primary, unique or foreign key) are named accordingly. 

#### Primary

Are defined using the PKs.

#### Nonunique index for FK columns

`[CONSTRAINTNAME]_IDX`

#### Unique index

`[TABLE]_[COLUMN without prefix]_UIDX`


### Package

Name is built from the content that is contained within the package followed by the suffix `_pkg`.

Optionally prefixed by a scope abbreviation and purpose.

Meaning | Prefix
------- | -----
Core (business logic) | `core_`
Presentation layer (data preparation for display) | `pl_`
Data access (obsolete) | `DA_`


Examples:

* `CORE_BETRIEB_PKG` - Main funcionalities for the domain entity `betrieb`
* `PL_TIERARZT_PKG` - Presentation layer for `tierarzt`.
* `logging_up` - Utilities including logging support

### Primary Key Constraint

Table name followed by the suffix `_pk`.

Most of the time the constraint is created using an `identity` column.

Examples:

* `benutzer_pk`

### Procedure

Name is built from a verb followed by a noun. The name of the procedure should answer the question “What is done?” 

Examples:

* `calculate_salary`
* `set_hiredate`
* `check_order_state`

### Sequence

Most of the time sequences are created implicit with the keyword `identity`.
If a sequence is created manually use the following convention.
Name is built from the table name the sequence serves as primary key generator and the suffix `_seq`.

Examples:

* `persin_seq`
* `kontakt_seq`

### Synonym

Synonyms should be used to address an object in a foreign schema rather than to rename an object. Therefore, synonyms should share the name with the referenced object.

### Unique Key Constraint

Are not created explicitly. Only by using unique indeces.

### View

Singular name of what is contained in the view followed by the suffix `_v`.
Optionally prefixed by a scope abbreviation and purpose.

See `Packages` for scope and purpose.

Examples:

* `benutzer_v`
* `betreuungsvertrag_v`

### Materialized View

For materialized views the same rules are applied as for views except the prefix `_mv`.

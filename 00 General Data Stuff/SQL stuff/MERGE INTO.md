A command to do updates, insertions, and deletions all in one query. 
I will split an example into multiple parts with some explanations

```
MERGE INTO target t USING source s
ON t.id = s.id

WHEN MATCHED 
AND...
THEN UPDATE SET
t.email = s.email
t.name = s.name (any other fields )
```
when matched means a row with id same in both tables. If the relationship isn't one to one this is more complicated
Any fields not in the UPDATE SET will be left as they are in target.

You could have multiple of any kind of block, for instance `when matched and s.status = 'update' t.x = s.x...` 
and 
`when matched and s.status = 'delete' DELETE`

```
WHEN NOT MATCHED [BY TARGET] THEN
INSERT (col1, col2) VALUES (s.col1, s.col2)
```
This is for new rows where the ID in the source table doesn't exists in the target table yet. This is a simple insert but you could do other functionality.
By default NOT MATCHED means NOT MATCHED BY TARGET but that part is optional
```
WHEN NOT MATCHED [BY SOURCE] THEN DELETE
```
This targets all the rows in the target where the source table does not have the ID anymore but the target still does. This often means that the row has been removed since the last update and needs deleting.
All these blocks are optional, you can have only one type or any combination of all 3
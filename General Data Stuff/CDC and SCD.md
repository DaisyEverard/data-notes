### CDC
Change Data Capture is a way of tracking and capturing changes in a data source then applying it to a target table. This includes updates, new information (inserts), and information to be deleted.

### SCD
Slowly Changing Dimensions define how to track historical changes such as a customer's address.
Type 1 SCD overwrites existing data effectively erasing history. Fine if you don't need history and care more about reducing storage costs.

Type 2 SCD stores previous version so you can query historical states. You need extra columns for validity such as start date, end date, or 'current' status/flag. Every update creates a new row, deleted information is still retained but has an end period or invalid status added.

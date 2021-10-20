# roll_your_own_pt-online-schema-change
An explaination of how I do the same functionality of pt-online-schema change. 

# Use Case
Large table needs a change and downtime is not an option. 

Percona Toolkit has a utility that does this, but it has some issues. This roll your own method is basically the same as pt-online-schema-change with additional functionality. 

Its a bit more tricky to do than a single line of command line code, so some explaination as to why you may want to do it this way is in order. 

## Let's look at what pt-online-schema-change is actually doing under the hood 

1. Creates a new target table in the above schema that will replace the old source table when the process is complete
2. Add triggers to the source table that replicate traffic to the old table into the new table 
3. Backfill from old source table into new target table. This can take quite some time depending on the size of the table 
4. when backfill is finished, rename the tables 

In many cases this is just fine. Here's why I choose to roll my own. 

1. Foreign keys get goofy names. 
2. Rollback is clunky and almost impossible 
3. Cannot stop and restart the backfill process to allow for peak times in traffic. The backfill process can take up a good amount of cpu. Once pt-online-schemac-change starts, you cannot pause for peak or spikes in traffic.
4. Have no control over when the rename happens.  
5. Cannot split the table into multiple tables. Too limited in the kinds of transformations allowed. 














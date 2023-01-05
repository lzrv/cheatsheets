 # PostgreSQL cheatsheet

 ## max_connections and connection pooling
 In PostgreSQL, the optimal number of active database connections is usually somewhere around **((2 \* core_count) + effective_spindle_count)**. Above this number, both throughput and latency get worse. NOTE: Recent versions have improved concurrency, so in 2022 I would recommend something more like **((4 \* core_count) + effective_spindle_count)**

 https://wiki.postgresql.org/wiki/Number_Of_Database_Connections

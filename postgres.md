# PostgreSQL cheatsheet

## max_connections and connection pooling
 In PostgreSQL, the optimal number of active database connections is usually somewhere around **((2 \* core_count) + effective_spindle_count)**. Above this number, both throughput and latency get worse. NOTE: Recent versions have improved concurrency, so in 2022 I would recommend something more like **((4 \* core_count) + effective_spindle_count)**

 https://wiki.postgresql.org/wiki/Number_Of_Database_Connections

## Check Postgres SSL connection with `openssl s_client`
```
openssl s_client -starttls postgres -connect $PG_HOST:5432 -showcerts
```

## Connect over SSL with psql
```
psql -a "sslmode=verify-ca sslrootcert=server-ca.pem sslcert=client-cert.pem sslkey=client-key.pem hostaddr=$PG_HOST port=5432 user=$UNAME dbname=$DB_NAME"
```

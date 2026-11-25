# Files for SQL

This folder is mounted **read-only** at `/data`, the one folder the SQL Query page (0.16.0) may
read files from. A query joins a topic with a file here:

```sql
SELECT p.category, round(sum(o.quantity * (o.priceEur - p.unitCostEur)), 2) AS margin
FROM kafka_topic('orders', value_format := 'Avro') o
JOIN read_csv('/data/products.csv') p USING (item)
GROUP BY ALL
ORDER BY margin DESC
```

`products.csv` is a price list for the seeded `orders`: each item's category and unit cost, the
kind of data that lives in a spreadsheet and never travels through Kafka.

Kafka is the subject of every query; a file only adds to it. Add your own CSV, Parquet or JSON
files here and read them with `read_csv`, `read_parquet` or `read_json` and the path under
`/data`. Nothing outside this folder can be read, and nothing can be written: keep the mount
read-only, because the console turns SQL off when the folder is writable.

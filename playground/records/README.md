# Bring your records

Drop record files here and they are produced into the playground's broker at every
`docker compose up`, so a sample of your data survives `docker compose down`. The folder is
optional and does nothing while empty.

| File | Produced as |
| --- | --- |
| `orders.avro.jsonl` | topic `orders`, each value framed against subject `orders-value` (Avro) |
| `shipments.json-schema.jsonl` | topic `shipments`, framed against `shipments-value` (JSON Schema) |
| `payments.protobuf.jsonl` | topic `payments`, framed against `payments-value` (Protobuf) |
| `notes.json.jsonl` | topic `notes`, plain JSON, no registry |

The name before the first dot is the topic (three partitions, created if missing); the part
between the dots is the format; the value subject is always `<topic>-value`, registered from
`../schemas/` or already there. One record per line, as the JSON the schema describes. A line
`key|{...}` is a keyed record with a String key; a line `{...}` has a null key. Any other
extension is ignored, this file included.

Keep a sample here, not a mirror: a few hundred lines prove a design's logic; volume and restore
time are a matter for a real cluster. Anything personal in the records is yours to redact before
it lands here.

To take a sample from your own cluster, consume with the registry-aware console consumer of the
same format, which prints one JSON record per line, into a file named for the topic and format.

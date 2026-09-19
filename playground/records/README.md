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

Keep a sample here, not a mirror. A small sample proves a design's logic if it is the right one:
the ordinary record, and the ones that break things, a null where a field is optional, an empty
list, a value on the boundary of your predicate. Production may not be able to give you those;
here each is one more line you write by hand. Volume and restore time are a matter for a real
cluster. Anything personal in the sampled records is yours to redact before it lands here.

To take a sample from your own cluster, consume with the registry-aware console consumer of the
same format, which prints one JSON record per line, into a file named for the topic and format.

`examples/` holds three templates. `shipments.json-schema.jsonl`, keyed lines against a JSON Schema
subject, and `invoices.avro.jsonl`, bare lines with a null key against an Avro one; their schemas
are in `../schemas/examples/`. And `shipments-cdc.json.jsonl`, which needs no schema at all: four
Debezium change events captured from a Postgres, plain JSON as the JSON converter writes them, so
copying that one file up gives you a CDC topic with no database and no connector. Subfolders are
skipped, so the templates do nothing where they are: copy one up a level, `docker compose up -d`,
and the topic, and its subject where it has one, are there.

The Debezium sample has two things worth knowing, both of them the envelope's rather than the
playground's. Its lines carry no key, because a Debezium JSON key is itself an object and starts
with `{`, which the key rule above reads as the start of a value; the envelope is what the sample
is for, and it is untouched. And a delete arrives as a record whose `after` is null, followed by
a tombstone, whose value is null rather than absent. A file cannot express that, so produce it
yourself: the same key, `Void` as the value type, one record from the console.

A topic the playground already seeds (`orders`, `payments`, `customers`, `text-lines`) receives
your records on top of the seeded ones. Use your real names; they only collide if they are
literally these.

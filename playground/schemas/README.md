# Bring your schemas

Drop schema files here and they are registered in the playground's Schema Registry at every
`docker compose up`, so they survive `docker compose down`. The folder is optional and does
nothing while empty.

| File | Registered as |
| --- | --- |
| `orders-value.avsc` | subject `orders-value`, type AVRO |
| `shipments-value.json` | subject `shipments-value`, type JSON (JSON Schema) |
| `payments-value.proto` | subject `payments-value`, type PROTOBUF |

The file name before the extension is the subject; name it `<topic>-value` (or `<topic>-key`)
and the builder's source node binds it to that topic. A schema that references another subject
needs that subject registered first; files register in name order, so name the referenced one
to sort first. Any other extension is ignored, this file included.

To copy a subject from your own registry, save the `schema` field of
`GET /subjects/<subject>/versions/latest` as the file's content.

`examples/` holds two templates, a JSON Schema subject and an Avro one, matching the record
templates in `../records/examples/`. Subfolders are skipped, so they do nothing where they are:
copy them up one level to try the folders, then replace them with your own.

A name the playground already seeds collides with the seed: `orders-value` is registered at
every `up` with the demo's schema, and a different schema under that subject is refused as
incompatible. Use your real names; they only collide if your topic is literally called `orders`.

# Alginte Playground

The playground is where to start with [Alginte](https://www.alginte.com),
whether or not you have a cluster: **one command**, no Kafka needed. This
compose file starts Alginte together with a single-node Kafka (KRaft) and a
Schema Registry, seeded with sample topics, records, and a consumer group
showing real lag — so every screen has something to look at. The `orders` topic
holds real Avro records framed against the registered `orders-value` subject,
and a Kafka Streams topology (`orders` → order total → big orders →
`orders-enriched`) is deployed through Alginte's own API on startup — open the
Streams area to see it running, with per-node throughput on the canvas.

It is also the product's isolated mode. The stack is yours alone and leaves
nothing behind, so a stream can be designed, deployed and run wrong here at no
cost, then carried to your cluster as a file once it runs right: **Download** on
the builder here, **Upload** on the Alginte that sees your cluster.

```bash
curl -O https://raw.githubusercontent.com/alginte/community/main/playground/docker-compose.yml
docker compose up -d
```

Then open **http://localhost:8888**.

Reset everything (nothing is left behind — no volumes, by design):

```bash
docker compose down
```

## Take the deployed topology apart

The seeded `orderEnrichment` stream is the one from our blog post: a
`mapValues` computing each order's total from a SpEL expression, a `filter`
keeping totals over 50, and a JSON sink. Open any node's drawer to read the
expression, watch a **real record** from `orders` flow through the preview
(the newest one is `ord-1010` — a Kettle, 3 × 17.0), and browse
`orders-enriched` to read what the expression actually produced.

## Build your first stream

The playground also seeds a `text-lines` topic as ingredients for your first
own Kafka Streams topology — follow the
[Streams builder walkthrough](https://docs.alginte.com/streams/building) to
build word-count on the canvas and watch `word-counts` fill up.

## Bring your schemas and your data

Two folders beside the compose file, created empty on the first `up`, are registered and
produced at every `docker compose up`, so your schemas and a sample of your records survive
`docker compose down`:

- `schemas/<subject>.avsc` | `.json` | `.proto` — registered under `<subject>` (Avro, JSON
  Schema, Protobuf); name it `<topic>-value` and the builder binds it to that topic.
- `records/<topic>.avro.jsonl` | `.json-schema.jsonl` | `.protobuf.jsonl` — produced into
  `<topic>`, each value framed against `<topic>-value`; `records/<topic>.json.jsonl` — plain
  JSON, no registry. One record per line; `key|{...}` for a keyed record, `{...}` for a null key.

Keep a sample here, not a mirror, and redact anything personal before it lands in the folder.
The READMEs inside the two folders carry the details, and each has an `examples/` subfolder with
a JSON Schema pair (`shipments`) and an Avro pair (`invoices`) as templates: subfolders are
skipped, so copy a pair up one level and `docker compose up -d`.

## Notes

- The UI is published on `127.0.0.1` only, with no login — it serves your
  machine and nothing else.
- `alginte/alginte:latest` — the playground always runs the current release.
- Sandbox mode is on (`ALGINTE_SANDBOX_ENABLED`): from Alginte 0.13.0 the MCP
  endpoint at `/mcp` also offers the tools that validate, deploy and reset
  stream designs, so an agent can work the builder here the way a person does.
  Nothing here is shared, and `docker compose down` undoes everything.
- Full documentation: https://docs.alginte.com — questions and feedback:
  [Discussions](https://github.com/alginte/community/discussions).

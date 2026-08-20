# Alginte Playground

Try [Alginte](https://www.alginte.com) on your machine with **one command** — no
Kafka cluster needed. This compose file starts Alginte together with a
single-node Kafka (KRaft) and a Schema Registry, seeded with sample topics,
records, and a consumer group showing real lag — so every screen has something
to look at. The `orders` topic holds real Avro records framed against the
registered `orders-value` subject, and a Kafka Streams topology
(`orders` → order total → big orders → `orders-enriched`) is deployed through
Alginte's own API on startup — open the Streams area to see it running, with
per-node throughput on the canvas.

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

## Notes

- The UI is published on `127.0.0.1` only, with no login — it serves your
  machine and nothing else.
- `alginte/alginte:latest` — the playground always runs the current release.
- Full documentation: https://docs.alginte.com — questions and feedback:
  [Discussions](https://github.com/alginte/community/discussions).

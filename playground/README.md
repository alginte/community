# Alginte Playground

Try [Alginte](https://www.alginte.com) on your machine with **one command** — no
Kafka cluster needed. This compose file starts Alginte together with a
single-node Kafka (KRaft) and a Schema Registry, seeded with sample topics,
records, and a consumer group showing real lag — so every screen has something
to look at.

```bash
curl -O https://raw.githubusercontent.com/alginte/community/main/playground/docker-compose.yml
docker compose up -d
```

Then open **http://localhost:8888**.

Reset everything (nothing is left behind — no volumes, by design):

```bash
docker compose down
```

## Build your first stream

The playground seeds a `text-lines` topic as ingredients for your first Kafka
Streams topology — follow the
[Streams builder walkthrough](https://docs.alginte.com/streams/building) to
build word-count on the canvas and watch `word-counts` fill up.

## Notes

- The UI is published on `127.0.0.1` only, with no login — it serves your
  machine and nothing else.
- `alginte/alginte:latest` — the playground always runs the current release.
- Full documentation: https://docs.alginte.com — questions and feedback:
  [Discussions](https://github.com/alginte/community/discussions).

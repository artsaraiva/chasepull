# Community collation data is the source of truth, labelled as estimated

Booster composition and sheet weights come from MTGJSON, which takes them from the community project `taw/magic-sealed-data`. That project calls its data a "close approximation" reverse-engineered from opened packs. It is not official publisher data. We chose it over hand-curating every set from the publisher's "Collecting [Set]" articles, because those articles give slot-level percentages at best and hand curation doesn't scale across hundreds of sets.

Consequences:
- Odds computed from this data are always shown as **estimated**, never as official.
- The publisher's own percentages are stored as **official shares**. They are used as checks: on a verified product, a mismatch of more than 1 percentage point fails the ingest. On an unverified product it only warns.
- A mismatch is fixed with an explicit override. Weights are never rescaled automatically to match official numbers, because automatic rescaling would hide errors in the source data.
- Where no weights exist, odds are shown as unknown, never guessed.

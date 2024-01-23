# Notion SRS Scanner

A Python cronjob that periodically updates my Anki Master Deck by using data
from my Notion

## Why Does This Exist?

I wanted to make it as easy as possible to inject new things I learn into my
longterm memory. Better longterm memory across a large swath of knowledge
domains
[is the best way I know to improve my general intelligence](https://www.amazon.com/Uncommon-Sense-Teaching-Practical-Insights/dp/0593329732),
so I take the tool I already put all my information in
([Notion](https://www.notion.so/)) and am using this script to automate moving
that data into my [Anki Deck](https://apps.ankiweb.net/).

## Getting Started (TODO: still needs the cronjob launch script)

To get started, run the following:

```
$ nix develop
$ poetry run python -m src
```

## Flows

1. Type @srs-item into a block
2. every day a cron job (launchctl or systemd) runs which scans my last 7
   daysand:

   i) check if @srs-item is not striked through, and if it's not, uses block
   data to create a anki card

   ii) load card into existing anki deck using
   [genanki](https://github.com/kerrickstaley/genanki)

   iii) strikethrough the @srs-item block to prevent it from being re-processed.
   Note, I need to handle possible failure here, or else these operations will
   not be automic
3. review card in Anki

### Stretch Goal

Instead of requiring human to create the @srs-item in a card-ready format, have
this cronjob process call GPT-4 with a prompt we've already tested works, and
use that to create the card

## TODO

- [ ] Design and implement a data format for the card data in Notion. Learn and
      make an example call with genanki so we know we have sufficient data
- [ ] Write function for scanning last 7 days of Notion workspace looking for
      @srs-item, this should return a list of block ids that we can later query
      for the card data
- [ ] Use genanki with a specific deck to create the card
- [ ] Hook up the output of the @srs-items to the card creation logic
- [ ] write a script for automatically creating a launchctl cronjob a. Bonus
      points if it's all done determinstically through Nix

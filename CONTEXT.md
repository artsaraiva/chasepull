# ChasePull

ChasePull tells a trading card game player what can come out of the exact booster pack in their hand, how much each outcome is worth, and how likely it is. It models booster products and their collation, not just sets.

## Catalog

**Game**:
A trading card game, such as Magic: The Gathering.

**Set**:
A released group of cards within a game, identified by a set code (e.g. LCI).
_Avoid_: Expansion, edition

**Card**:
A game-level card identity shared by all its printings (e.g. "Cavern of Souls").

**Printing**:
One specific printed version of a card: its set, collector number, rarity, art and treatment. A printing always belongs to its own set, even when a product from another set can contain it.
_Avoid_: Version, edition, print

**Finish**:
The surface of a physical card: non-foil, foil or etched.
_Avoid_: Foiling, foil type

**Variant**:
A printing in one finish. The unit that has a price and can be pulled.
_Avoid_: Card (when a specific finish is meant), SKU

## Pack composition

**Product**:
One booster pack type of one set, in one language, that a person can buy and open (e.g. "The Lost Caverns of Ixalan Collector Booster", English). The same booster type in another language is a different product, because its pool can differ. Boxes, bundles and precons are not products.
_Avoid_: Sealed product, booster, SKU

**Booster type**:
The kind of booster a product is: draft, set, play, collector, jumpstart, or the single pre-2019 booster.

**Pack**:
One physical, sealed copy of a product, opened once.
_Avoid_: Booster (for a physical copy)

**Pack configuration**:
One possible internal layout of a product's packs, with a relative frequency. A product has one or more.
_Avoid_: Booster layout, collation variant

**Slot**:
A position in a pack configuration that holds a given number of cards drawn from one sheet.

**Sheet**:
A weighted collection of variants that a slot draws from. A sheet can contain printings from other sets.
_Avoid_: Card pool, print sheet

**Pool**:
Every variant that has a chance of coming out of a product. Shown to users as "All Possible Pulls".
_Avoid_: Card pool, card list, set list

**Pull**:
A variant coming out of an opened pack.
_Avoid_: Hit (unless valuable), drop

## Treatments

**Tag**:
A normalized label on a printing or variant, in one of four groups: rarity, finish, frame or special. Filters are built from tags.
_Avoid_: Attribute, property

**Treatment**:
The visual version of a printing, given by its frame and special tags (e.g. Borderless, Showcase, Neon Ink, Special Guest).
_Avoid_: Version, style, art type

**Treatment family**:
The variants of one card within a product that share the same tags and finish, such as the five neon ink colors of one card. Shown as a single expandable row.
_Avoid_: Variant group, color variants

## Value

**Price**:
The latest approximate market price of a variant in one currency.
_Avoid_: Value, cost

**Odds**:
The chance that at least one copy of a variant (or treatment family) comes out of one pack of a product.
_Avoid_: Pull rate, drop rate, probability

**Odds source**:
Where an odds figure comes from: official (published by the game's publisher), estimated (computed from collation data) or unknown.

**Chase pull**:
A treatment family that ranks among a product's most valuable outcomes. The chase list is ranked by family, not by card.
_Avoid_: Chase card, hit

## Data quality

**Override**:
A hand-maintained correction or label for a set's data, layered on top of the source data.
_Avoid_: Patch, fix

**Official share**:
A percentage published by the game's publisher for a slot's contents, used to check the computed data.

**Verified product**:
A product whose composition a person has checked against the publisher's official breakdown. Its computed data must agree with its official shares.

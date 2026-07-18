# Canonicle

**An onchain trinket holder.** A gizmo NFT whose parts and holdings are themselves onchain NFTs, e.g. collect them, equip them, swap them, show them off.

## The idea in one breath

A free-to-mint "device" NFT (the **Canonicle**) paired with its own token-bound account (ERC-6551), so the device itself owns its parts: a soulbound **Feed Seed** that generates a unique scrolling ASCII banner, tradeable **Lightbulb** NFTs that set the device's skin/theme, and 16 display slots for **Talis**, i.e. limited-edition onchain pixel-art trinkets you can equip in any arrangement you like.

Think: a curatorial onchain Pokédex meets a pinbook, fully composable and tradeable 24/7.

## The four foundations

| Component | Type | Role |
|---|---|---|
| **The Canonicle** | ERC-721 + ERC-6551 TBA, free mint | The main device. Its token-bound account holds everything below. Fully transferable. |
| **The Feed Seed** | Soulbound NFT, minted at creation | An onchain seed that generates a unique scrolling ASCII banner across the top of the device. Guarantees every Canonicle is baked-in unique. Can never leave its Canonicle. |
| **The Lightbulbs** | Tradeable limited editions, cheap paid mints | Equippable skins. A moon bulb gives a midnight theme, a sun bulb a summer theme, and so on, with routine new releases. Rendered as a small icon in the device's upper-left corner. |
| **The Talis** | Tradeable limited editions (1/1s, packs, 1/1/X sets) | Onchain pixel-art trinkets equippable into any of the 16 display slots, in any order. Cadenced official releases plus community-created releases (à la Nouns / BasePaint). |

See **[SPEC.md](./SPEC.md)** for the full design: architecture, bonus modules (the Stronghold hold-timer, an OKPC-style sketchpad), prior art, and open engineering questions.

## Why

I helped brainstorm the Canonicon (a dynamic, soulbound governance NFT) at the NFT curation protocol JPG, and I've kept turning the concept over in the years since JPG wound down. Canonicle is where that thinking landed after two questions: what if the system were composable and standards-based rather than single-contract with mutable metadata? And what if the NFT were less a governance/reputation instrument and more an onchain art object / collectible toy?

Original write-up: ["An NFT Experiment I'd Like to See"](https://x.com/wmpeaster/status/2076400765825126607) (July 2026).

## License

Released under [CC0](./LICENSE), no rights reserved. Feel free to take the idea and run with it.

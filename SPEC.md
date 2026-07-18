# Canonicle — Design Spec (v0.1, concept stage)

*This document describes an unbuilt system. Everything here is a design intention, not a shipped implementation. Contributions, forks, and critiques welcome.*

## 1. Overview

Canonicle is a fully onchain, composable collectible system. The core object is a device-like NFT — an "onchain trinket holder" — that owns its own parts and contents via a token-bound account. Holders customize their device by equipping tradeable component NFTs into it, and the device's art is rendered onchain from the live state of what it holds.

Design goals, in order:

1. **Composable and standards-based.** No single monolithic contract with mutable metadata. Each component class is its own real, tradeable ERC-721 collection, tied together by ERC-6551.
2. **Fully onchain.** Visuals rendered at runtime from contract state (SVG and/or ASCII), no prerendered images, no offchain metadata servers.
3. **Toy first, not instrument.** This is an art object / collectible, not a governance or reputation system.
4. **Low likelihood of collection overlap.** Many small, limited-edition drops (the Disney Pinnacle distribution model) so that any two holders' devices rarely look alike.

## 2. Architecture

```
Canonicle #123 (ERC-721)
   │
   └── Token-Bound Account (ERC-6551 wallet owned by the NFT)
          ├── Feed Seed #123 (soulbound — never leaves)
          ├── Lightbulb: "Moon Bulb" (equipped → midnight theme)
          ├── Tali × N (up to 16 equipped into display slots)
          └── ...anything else the holder deposits
```

- The **Canonicle contract** is a standard ERC-721. Each token is paired with an ERC-6551 token-bound account (TBA): a real wallet address owned by the NFT itself.
- Selling or transferring the Canonicle transfers everything inside its TBA along with it — the whole loaded device moves as one object, tradeable 24/7 like a pinbook.
- The renderer reads the TBA's holdings and the device's equip-state at `tokenURI` time and generates the composite art live (the Chonks pattern, in the same family as Uniswap V3/V4 LP NFTs rendering from live position state).

## 3. Components

### 3.1 The Canonicle (device)

- Free to mint.
- ERC-721 + ERC-6551 TBA.
- Fully transferable/sellable.
- The visual frame: banner region up top, Lightbulb icon in the upper-left corner, a 4×4 grid of 16 display slots, plus optional bonus modules (§4).

### 3.2 The Feed Seed

- One per Canonicle, minted directly into the device's TBA at creation.
- **Soulbound to the device**: transfer out is permanently blocked at the contract level. (Note: soulbound *to the Canonicle*, not to a human wallet — the seed travels with the device when the device is sold.)
- Its onchain seed, fixed at mint, deterministically generates a unique scrolling ASCII banner pattern across the top of the device — guaranteeing every Canonicle has a baked-in element of uniqueness even before anything is equipped. (Spiritual descendant of Terraforms' runtime ASCII rendering.)

### 3.3 The Lightbulbs

- Tradeable, limited-edition, inexpensive paid mints. Own collection (ERC-721).
- Equipping a Lightbulb sets the device's skin/theme: moon bulb → midnight theme, sun bulb → summer theme, etc.
- Appears as a small icon in the device's upper-left corner.
- Routine new releases keep the theme space growing.

### 3.4 The Talis

- The trinkets. Tradeable, limited-edition onchain pixel art. Own collection(s).
- Release shapes: some 1/1s, some packs, some 1/1/X large sets, at varying price points.
- Equippable into any of a Canonicle's 16 display slots, in any order the holder chooses; rearrangement is a holder action.
- Two release tracks: cadenced official drops, and a community-created track (a submission pipeline in the spirit of Nouns' trait process and BasePaint).

## 4. Bonus modules (optional depth)

- **The Stronghold** — a box on the device that changes color based on how long the Canonicle has sat in its current wallet (e.g. blue → 1 day, green → 1 month, red → 1 year), resetting on transfer. Inspired by Corruption(s*)'s Insight XP mechanic, inverted into a pure hold-timer.
- **The Sketchpad** — an onchain drawing surface at the bottom of the device that holders can tag and doodle on, à la OKPC and token.works S2 patron passes.

## 5. Prior art & inspirations

| Project | What it contributes |
|---|---|
| **Canonicon (JPG)** | The origin point: a dynamic soulbound NFT with XP-unlocked customizable visual layers and a display carousel. Canonicle inverts it — toy instead of governance instrument, composable instead of single-contract. |
| **Terraforms (Mathcastles)** | 100% runtime onchain ASCII rendering; no prerendered images. |
| **Corruption(s\*) (dhof)** | Time/holding-based involuntary visual mutation; state resets on transfer. |
| **Uniswap V3/V4 LP NFTs** | Fully onchain SVG generated live from actual contract state. |
| **OKPC / token.works S2** | Onchain drawing surfaces on NFTs. |
| **Nouns** | Flat, cheap onchain SVG trait art; community trait submission culture. |
| **Disney Pinnacle** | Distribution model: many small limited-edition drops → near-infinite collection variety, low holder overlap. |
| **Chonks** | ERC-6551 TBAs where the body NFT owns its trait NFTs as separate, fully tradeable ERC-721s. |
| **BasePaint** | Community-created, cadenced onchain art releases. |

## 6. Open engineering questions

These are unresolved and where a builder would need to make real decisions:

1. **Rendering budget.** Compositing a seed-driven ASCII banner + themed skin + up to 16 pixel-art trinkets + bonus modules into one `tokenURI` output, fully onchain — what's the gas/bytecode strategy? SSTORE2-style storage? Shared palette + run-length-encoded sprites (the Nouns approach)? How large can a composite SVG get before marketplaces choke?
2. **Equip-state storage.** Where does slot ordering live — on the device contract, or inferred from the TBA plus an ordering map? What does the rearrange UX cost per transaction?
3. **Soulbound enforcement.** Cleanest way to make the Feed Seed non-transferable *out of a TBA* while the TBA's owner NFT remains freely transferable.
4. **Equip semantics.** Is "equipped" simply "held in the TBA," or held + explicitly slotted? What happens to slotted state when a Tali is sold out of the TBA?
5. **Chain choice.** Mainnet (cheap enough now for most actions) vs. Base (Chonks precedent, Coinbase distribution) — and whether component collections should ever be multichain.
6. **Community submission pipeline.** How are community Tali proposed, approved, and revenue-shared?

## 7. Status & invitation

I'm not a smart contract developer and I'm not currently pursuing a build. This spec is open-sourced so anyone can. If you take a swing at it — or want to riff on the design — reach out: [@wmpeaster](https://x.com/wmpeaster) / wmp.eth. I'd love to be your first minter.

# Dscryb

Sound, maps and read-aloud boxed text for your table, inside Foundry. Dscryb's ambience, music and sound effects come from Michael Ghelfi Studios.

[![](http://img.youtube.com/vi/p5D-bxJT0BU/0.jpg)](http://www.youtube.com/watch?v=p5D-bxJT0BU)

## What it does

- **Opus browser.** Search the whole sound library, filter by music, ambience or SFX and by setting, and play for everyone. The Essentials drawer keeps rain, wind, sea, tavern, campfire and more one click away, and one Master slider sets the level for the table. Music crossfades from track to track, switches to a combat track when combat starts, and a scene can remember its sounds so they change with the map.
- **Sound for D&D adventures.** A play button on each section of 71 D&D adventures, for the GM only: ambience, plus music where it fits. It works on adventures imported from D&D Beyond with DDB Importer, and on the official Foundry modules of Phandelver and Below, Tomb of Annihilation, Heroes of the Borderlands, Adventures in Faerûn and Ravenloft: The Horrors Within. Their maps get each room's ambience at its pin, so it fades in as tokens walk in, and you can drag any section's play button onto a map to place it yourself. Dscryb supplies the sound; the adventure is yours.
- **Prep your own adventures.** Pick a sound for each heading of any journal, with suggestions from Dscryb's search.
- **Sheet sounds.** Spells, weapons and monsters play a sound when they are used or defeated, in D&D 5e and Pathfinder 2e. Each sheet has a Sounds window to change them, and monsters list the ambience of the places they live.
- **Boxed text.** Read-aloud descriptions of places, monsters, spells and items, synced into your journals, and shown to everyone with one click.
- **Maps.** Foundry-ready maps with walls, lights and pins, and day and night variants. When a token walks into a place, the GM gets its boxed text to read aloud, and its ambience can start for the table.
- **Stream Deck.** Play and stop sounds from the Dscryb Stream Deck plugin.

## Free and paid

Anyone can try a set of free sounds and boxed text. A Dscryb membership unlocks the rest, monthly or annual: **Heroes** for boxed text, sound effects and maps, **Composer** for music, ambience and sound effects, and **Celestial** for everything. Every plan starts with a [free 14-day trial](https://dscryb.com/tryforfree/?aff=836&utm_content=listing).

Sign in from the Dscryb button in the scene controls.

## What the module sends to Dscryb

Once a day, when a GM who is signed in to Dscryb loads a world, the module tells Dscryb which versions of the module, Foundry and the game system that world uses (for example `6.2.0`, `14.346`, `pf2e`). Dscryb counts it as one day of use for that Dscryb account. Nothing about your world is sent: no world, scene, actor, journal or player names, and players' clients never send it. It helps us see how many tables play with Dscryb and whether they keep using it.

To turn it off, open **Configure Settings → Dscryb** and untick **Share daily usage with Dscryb**. The setting is per world.

**Rating a sound** (the thumbs that appear beside an adventure section's play button once it has played a few seconds, or at once when you right-click the button) sends the vote with the book, chapter and section it is for, the sound, and a random id for the world, so one table counts once. No account and no world content. Stopping a section's sound within 15 seconds counts as a quiet thumbs down, sent only while **Share daily usage with Dscryb** is on. The D&D Beyond extension sends the same votes, and we use them to fix sounds that don't fit.

**How often a section's sound is shown and played.** While **Share daily usage with Dscryb** is on, the module counts how many times each adventure section's play button comes into view and how many times it is played, and sends that day's counts once the day is over: the book, chapter and section, the sound, the two counts and the world's random id. A sound that is often shown and rarely played probably doesn't fit, and we fix those.

**Report a bug** (in the Opus browser and in the settings sidebar) sends only what you type, plus the versions above, your browser and recent Dscryb error messages. It never includes world content.

## Macro API

The module exposes its API on `game.dscryb.api` for use in macros, scripts, and other modules.

### `playSfxByName(name, options?)`

Play a Dscryb SFX track by its display name without needing its internal id.

```js
// Exact name, broadcast to all players at the current interface volume
await game.dscryb.api.playSfxByName("Sword Clash");

// Fuzzy substring match (default), play locally only at half volume
await game.dscryb.api.playSfxByName("dragon", { volume: 0.5, broadcast: false });
```

**Parameters**

| Name                  | Type      | Default                          | Description                                                                |
|-----------------------|-----------|----------------------------------|----------------------------------------------------------------------------|
| `name`                | `string`  | —                                | Track name to look up. Case-insensitive.                                   |
| `options.volume`      | `number`  | `core.globalInterfaceVolume`     | Playback volume in the range `0..1`.                                       |
| `options.broadcast`   | `boolean` | `true`                           | If `true`, plays on every connected client via socket; otherwise local-only. |
| `options.fuzzy`       | `boolean` | `true`                           | If `true`, falls back to a substring match when no track matches exactly.  |

Returns a `Promise<string|null>` resolving to the track's id, or `null` if no match was found or the matched track has no playable file. The user sees a UI warning in either failure case.

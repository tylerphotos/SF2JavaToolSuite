# Changes on `feature/p0-p3-tool-fixes`

This branch is a fork of [ShiningForceCentral/SF2JavaToolSuite](https://github.com/ShiningForceCentral/SF2JavaToolSuite) by **tylerphotos**.

The old Python Caravan ROM editor is not being revived. These Java tools already edit SF2DISASM split files. This work finishes open bugs, leftover old-format I/O, a stats editor, and a launcher so the suite can stand in for Caravan.

Two commits on top of `main`:

1. `9ad8ad2` — Fix open tool-suite bugs and add map/battle overlays.
2. `5323d7a` — Add stats editor, tool launcher, and disassembly-native processors.

## Bugfixes (existing tools)

These map to open SFC issues where noted.

| Area | What changed | Issue |
|---|---|---|
| MapEditor roofs | One formula for auto-source 255,255 roofs; invalid boxes draw red; `setSource` no longer destroys the 255 sentinel | #61 |
| Map animation save | Dest written as `$%03X`; empty dest is `$100`; inherit previous dest; frames always written | #59 |
| Mapsprite export | Skip missing mapsprites with `continue` instead of `break` | #51 |
| Dialog portraits | `PORTRAIT_` prefix handling; unique-entry import instead of `entriesCount` | #58 |
| Table checkboxes | Compact boolean editor/renderer so the hitbox fills the cell | #42 |
| Mapsetup entities | Parse `mapsetups/s1_entities.asm` and overlay entities on the map | #36 |
| Unused blocks | Overlay unused blocks in the blockset | #45 |
| MapEditor window | Smaller default layout | #46 |
| AI regions | Fill per-tile with `Polygon.contains` | #35 |
| Coord bars | Paint at `getVisibleRect()` so they stay pinned while scrolling | #33 |
| Portrait metadata | Keep-metadata option when reimporting | #38 |
| CI | Split into build / tag-and-release / push-disasm jobs | #53 |

## New tools

### SF2StatsEditor

Tabbed editor for SF2DISASM **standard** stats macros:

- Ally growths and spell lists (`allystatsXX.asm`)
- Spell definitions (`spelldefs.asm`)
- Item definitions (`itemdefs.asm`)
- Class definitions (`classdefs.asm`)
- Shop inventories (`shopinventories.asm`)

Default paths assume the jar is run from `disasm/data/stats/`.

### SF2ToolLauncher

Scans a folder for `SF2*.jar`, prefers `store/` fat jars over `dist/` jars, and launches the selected tool with the same Java 17 process that started the launcher. Skips CoreLibrary and itself.

## Leftover processors (old format → SF2DISASM macros)

| Tool | What it does now |
|---|---|
| SpellAnimationEditor | Reads and writes `vdpSpell` data (`SPELLTILEn`, `Vn\|Hn\|32/33`). 33 is horizontal flip. |
| GraphicsManager / TilesetManager | Layout import/export lives on `TilesetManager` (no longer commented-out old code). |
| PortraitManager | `exportDisassemblyToEntryFile` writes unique `incbin` entries plus bins, matching unique-entry import. |

`release.properties` lists StatsEditor (`disasm/data/stats/`) and the launcher (`disasm/`).

## How to build

Need **JDK 17** and **Apache Ant**. These NetBeans projects use `platform.active=JDK_17`, so Ant needs that home:

```text
ant "-Dplatforms.JDK_17.home=<path-to-jdk-17>" package-for-store
```

Fat jars land in each project's `store/` folder. `build/`, `dist/`, and jars are gitignored.

On Windows, quote the `-D` flag so PowerShell does not split on `=`.

## How to run

Use Java 17, not an older JRE:

```text
java -jar SF2StatsEditor.jar
java -jar SF2ToolLauncher.jar
```

Point the launcher at the suite root (or a folder of `store/` jars) and Scan.

## Status

- Compiles locally with JDK 17 / Ant 1.10.
- Not yet clicked through against a live SF2DISASM tree.
- This branch is on https://github.com/tylerphotos/SF2JavaToolSuite — intended as a PR into SFC `main`, not a direct push there.

# World of Filtercraft

## What this is

An NLP (Neuro-Linguistic Programming) role-playing game designed by Jennifer Bruederl as her NLP Master thesis (DVNLP certification). It's a 30-minute simultaneous coaching event where participants take on fantasy hero roles to explore their cognitive filters (meta-programs, beliefs, values).

Core concept: "Die Landkarte ist nicht das Gebiet." Players swap their real-world filters for defined hero stats, making NLP concepts experiential through gameplay.

## Game mechanics

- 8 hero classes (Volk + Klasse), each with RPG stats AND NLP meta-programs (Prozedural/Optional, Hin-zu/Weg-von, Internal/External, etc.)
- Each hero has a "Magischer Filter" (limiting belief) that distorts perception
- 8 "Epische Ereigniskarten" (event cards) - ALL intentionally positive events (dragon hoards, immortality, world peace). The game mechanic is that each hero's filter distorts even these good things.
- 4 coaching stations using NLP Master methods: Museum der Glaubenssaetze, Mentoren-Technik, Sleight of Mouth, Core Transformation

## Card generation

Cards are generated using the `generate-with-refs` skill (Gemini API via Go script in `.claude/skills/generate-with-refs/scripts/generate.go`).

Key parameters for event cards:
- `--aspect "9:16"` (tarot portrait format, 70x120mm)
- `--size "1K"` for drafts, `--size "4K"` for final print-quality
- Card 1 is generated without refs, cards 2-8 use card 1 as `--refs` for style consistency
- Prompts live in `cards/prompts/card0N-prompt.md`

Card layout: header ("Epische Ereigniskarte") > themed emblem > title > stats bar > description (from Ereigniskarten_Epic_Loot.docx.md) > flavor/lore quote > "World of Filtercraft" branding. Each card has unique themed border motifs.

The generate.go script was modified to support `--aspect` flag and optional `--refs`.

## Source documents

- `NLP_Spiel_World_of_Filtercraft_Masterarbeit_final.md` - Jennifer's thesis
- `Heldenklassen_V9_Volk_und_Klasse(1).docx.md` - Hero classes with stats and meta-programs
- `Ereigniskarten_Epic_Loot.docx.md` - Canonical event card descriptions (use these, not rewrites)

## Important notes

- Jennifer (wife) reviews card designs and provides feedback
- Use her ORIGINAL German text from Ereigniskarten doc, don't rewrite
- German umlauts matter (Goldstücke not Goldstucke, Hochkönige not Hochkoenige)
- Cards will be printed at a copy shop on thick paper - final versions need 4K resolution

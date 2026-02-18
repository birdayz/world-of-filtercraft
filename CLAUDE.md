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

## Print PDF generation

Output: `print/world_of_filtercraft_cards.pdf` - 16-page A4, double-sided layout (front/back pairs).

### Dimensions

- A4 at 300 DPI = 2480x3508 pixels
- Tarot card (70x120mm) at 300 DPI = 827x1417 pixels
- Station card (DIN A5 landscape) resized to 2400x1350 on A4

### Tarot cards (events + heroes): 2x2 grid on A4

```bash
montage card1.png card2.png card3.png card4.png \
  -tile 2x2 -geometry 827x1417+40+40 -resize 827x1417 \
  -background white -gravity center page_front.png
magick page_front.png -gravity center -background white -extent 2480x3508 \
  -density 300 -units PixelsPerInch page_front.png
```

Back pages use the same montage with `cards/card_back.png` repeated 4 times.

### Station cards: centered on A4

```bash
magick station.png -resize 2400x1350 -gravity center \
  -background white -extent 2480x3508 \
  -density 300 -units PixelsPerInch page_station.png
```

Back pages use `cards/stations/station_back.png` the same way.

### Combine into PDF

```bash
magick page01.png page02.png ... page16.png \
  -density 300 -units PixelsPerInch print/world_of_filtercraft_cards.pdf
```

### Compress for GitHub (< 100MB)

Full quality PDF is ~120MB. Compress with Ghostscript to ~12MB:

```bash
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/printer \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile=compressed.pdf input.pdf
```

### Page order (double-sided printing)

1. Event cards 1-4 (front)
2. Card backs x4 (back)
3. Event cards 5-8 (front)
4. Card backs x4 (back)
5. Hero cards 1-4 (front)
6. Card backs x4 (back)
7. Hero cards 5-8 (front)
8. Card backs x4 (back)
9-16. Station cards 1-4, each front + back pair

### Design consistency

- Hero cards: hero01 is the design reference, all others use `--refs hero01_mensch_paladin.png`
- Station cards: station02 is the design reference, all others use `--refs station02_mentoren_technik.png`
- Event cards: card01 is the design reference for cards 2-8

## Important notes

- Jennifer (wife) reviews card designs and provides feedback
- Use her ORIGINAL German text from Ereigniskarten doc, don't rewrite
- German umlauts matter (Goldstücke not Goldstucke, Hochkönige not Hochkoenige)
- Cards will be printed at a copy shop on thick paper - final versions need 4K resolution

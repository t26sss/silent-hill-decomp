# Android port: local changes

## Minimap (rotating mode)
- `pc_port/src/pc_minimap.c`: the map can turn around Harry so his arrow
  always points up. Markings are clipped at the panel edge. The turn is done
  in cell space (1 cell = 1 image column = 2 image rows) so the picture does
  not look skewed. The radius is `MM_ROT_RHO` (in cells).
- `pc_port/include/pc_config.h`, `pc_port/src/pc_config.c`: new option
  `minimapRotate` (config key `minimap_rotate`, default 1).
- `pc_port/src/pc_quick_options.c`, `src/screens/options/options.c`:
  "Minimap Rotate" On/Off row.

## Thai language
Code (`pc_port/src`, `pc_port/include`):
- `pc_kanji.c/.h`: draws Thai text from a table, one cell per syllable,
  12 px wide. Spacing between syllables is `THAI_GAP` in `Pc_KanjiAdvance()`
  (lower = tighter); `ThaiInkExtent()` measures each glyph's real width.
- `lang_pack.c/.h`: reads the language pack, knows `!font=thai`.
- `lang_text.c`: picks the Thai font, uses the PAL Latin atlas for English
  letters and digits, and shows the Language row when a Thai pack exists.
- `lang_quick.c/.h`: Quick Options text.

Data:
- `th.lang` (in `pc_port/assets/gamedata/lang/`): the Thai pack. Text inside
  is syllable codes, so never edit it by hand.
- `kanji_font_th.inc`: the Thai syllable font table, built from GNU Unifont.

Tools:
- `make_thai_pack.py`: builds `th.lang` and `kanji_font_th.inc` from a UTF-8
  translation file. Always regenerate both together.
- `story_conv.py`, `build_all.py`: convert the format, check control codes
  (`~J ~C ~S`) against the original, and check lines stay within 20 cells.

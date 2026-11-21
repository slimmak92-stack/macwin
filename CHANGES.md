# ABRA 0.6.0 – Windows 10 fixes (untested build, see notes)

1. **Missing icons** – `platform/src/text.rs`: the icon font was hardcoded to "Segoe Fluent Icons" (Windows 11 only).
   New `icon_face()` falls back to "Segoe MDL2 Assets" (Win10) then "Segoe UI Symbol". All 6 call sites updated.
2. **English by default** – `core/src/settings.rs`: default `lang` is now `Lang::En`. (Saved settings with `lang=id` stay
   Indonesian until changed in Settings → General → Language.)
3. **Lyrics delay** – `platform/src/media.rs`: SMTC position is now corrected by `LastUpdatedTime` (browsers report it
   rarely, which made lyrics seconds late). `app/src/notch/mod.rs`: lyrics use a 350 ms lead (`LYRIC_LEAD_MS`) and the
   extra 15 ms timer slack was removed.
4. **Blurry text** – `platform/src/text.rs`: text is rendered at native size (hinted) instead of 3x supersampled then
   box-filtered, with a small coverage gamma. Composition sprite offsets are rounded to whole pixels
   (`controlcenter.rs`, `notch/draw.rs`) so surfaces are not resampled.
5. **Liquid glass** – `ui/src/glass/panel.rs`: stronger saturation, lighter tint, brighter rim, diagonal specular
   sheen with a faint bottom-edge catch-light.

Tweak knobs: `LYRIC_LEAD_MS`, `SS` and the `powf(0.8)` in text.rs, glass numbers in `GlassParams::light`.

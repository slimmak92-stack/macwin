# ABRA 0.6.0 – Windows 10 fixes (untested build, see notes)

1. **Missing icons** – `platform/src/text.rs`: the icon font was hardcoded to "Segoe Fluent Icons" (Windows 11 only).
   New `icon_face()` falls back to "Segoe MDL2 Assets" (Win10) then "Segoe UI Symbol". All 6 call sites updated.
2. **English by default** – `core/src/settings.rs`: default `lang` is now `Lang::En`. (Saved settings with `lang=id` stay
   Indonesian until changed in Settings → General → Language.)
3. **Lyrics delay** – `platform/src/media.rs`: SMTC position is now corrected by `LastUpdatedTime` (browsers report it
   rarely, which made lyrics seconds late). `app/src/notch/mod.rs`: lyrics use a 350 ms lead (`LYRIC_LEAD_MS`) and the
   extra 15 ms timer slack was removed.
4. **Text rendering** – reverted to the original 3x supersampled renderer (`SS = 3`, no gamma) after the native-size
   attempt looked worse. Only change kept: composition sprite offsets are rounded to whole pixels
   (`controlcenter.rs`, `notch/draw.rs`).
5. **Liquid glass** – `ui/src/glass/panel.rs`: stronger saturation, lighter tint, brighter rim, diagonal specular
   sheen with a faint bottom-edge catch-light.

Tweak knobs: `LYRIC_LEAD_MS`, glass numbers in `GlassParams::light`.
6. **Start button** – `app/src/dock/mod.rs`: the Dock's Start item (click and right-click → Open) now opens ABRA's glass
   Apps panel (`launchpad::toggle()`) instead of the plain Windows 10 Start menu, which is a system window ABRA cannot
   restyle. The Win key still opens the normal Windows Start menu.
7. **Blurry menu bar left side** – `app/src/menubar.rs`: `sprite()` placed text at fractional pixel offsets
   (`draw_text` centres with `(w - tw) / 2`), so DWM resampled it; the right side used `small_text`, which rounds.
   Offsets are now rounded in `sprite()`; same fix in `spotlight.rs` and `settings_win.rs` text sprites.
8. **Low-quality / jaggy glass** – `ui/src/glass/effect_graph.rs`: the backdrop Gaussian blur used D2D optimization
   mode 0 (SPEED: blurs at reduced resolution then upsamples → blocky, shimmering edges). Now 2 (QUALITY). If GPU
   use or animation smoothness suffers, set it to 1 (BALANCED).

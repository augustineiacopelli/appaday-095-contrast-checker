# AppADay 095: Contrast Checker

Set a text color and a background color and watch the WCAG 2.1 contrast ratio move as you type, with pass or fail marks for normal text, large text, and interface elements. When a pair falls short, the app hands back the nearest color that would actually clear AA or AAA so the fix is one click instead of a guessing game.

**Live app:** https://augustineiacopelli.github.io/appaday-095-contrast-checker/

**Portfolio:** https://augustineiacopelli.github.io/appaday/

## What it does

Two fields drive everything. Each takes a native color picker or a typed value, and the typed field accepts short hex, full hex, hex with alpha, `rgb()` notation, and CSS named colors like `rebeccapurple`. Anything it cannot parse turns the field red and leaves the last good value in place rather than blanking the result.

The ratio is computed from relative luminance exactly as WCAG 2.1 defines it, using the sRGB linearization curve and the `(L1 + 0.05) / (L2 + 0.05)` formula. Black on white returns 21.00 and `#767676` on white returns 4.54, which is the canonical AA boundary gray, so the math can be spot checked against any published reference.

Five thresholds are graded at once: AA normal text at 4.5, AA large text at 3, AAA normal text at 7, AAA large text at 4.5, and the 3.0 floor that applies to interface components and meaningful graphics. Each gets its own pass or fail badge with the required number shown alongside, so it is obvious how much headroom a pair has rather than just whether it squeaked by.

The preview panel renders the actual pair at four scales, from a 24px bold heading down to thirteen pixel fine print, plus a solid button, an outlined button, and a rule, because a ratio that reads fine as a number can still look thin once it is set in small type.

## Nearest passing colors

When a pair fails, the app walks the lightness channel of the offending color outward in HSL, hue and saturation held fixed, until the target ratio is met, then reports the first color that clears it in each direction and keeps the closer one. It does this for the text color and the background color independently, at both the AA and AAA thresholds, so up to four suggestions appear as clickable chips with their resulting ratio. Applying one loads it straight into the fields. If a pair already clears AAA at every size, the section says so instead of offering busywork.

## Other conveniences

Swap flips the two colors. Random pair generates a legible starting point rather than pure noise. Save pair keeps up to fourteen combinations in `localStorage`, rendered as small swatches showing the real pair, click to reload and click the corner to drop. Copy CSS puts a ready `color` and `background-color` declaration with the ratio in a comment on the clipboard, and clicking the big number copies just the ratio. The current pair is written into the URL hash, so a link carries the exact colors to whoever opens it.

## Technical notes

Single `index.html`, no build step, no frameworks, no dependencies beyond Google Fonts. Source is fully ASCII with Unicode delivered as HTML entities. `localStorage` is wrapped in `try`/`catch` so the app survives a `file://` origin, and the `history.replaceState` hash sync is guarded the same way. Named color parsing goes through a hidden computed-style probe rather than a canvas, so nothing depends on a 2D context being available. Layout uses `min-height: 100dvh` with a two column grid that collapses to one below 880px and stays usable at 375px.

## Stack

HTML, CSS, and vanilla JavaScript. Space Grotesk and JetBrains Mono from Google Fonts. Deployed with GitHub Pages.

## About AppADay

One complete, functional, mobile-friendly, visually polished web app, designed and shipped every single day. Inspired by Jonathan Mann's Song A Day. Browse the full archive at https://augustineiacopelli.github.io/appaday/

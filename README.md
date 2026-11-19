Walker

A Pokéwalker-style step counter for a Neocities site. It shows today's step count from an iPhone (Apple Health) on a pure HTML/CSS recreation of the device, with a swappable Pokémon sprite.

Live page: https://regueritos.neocities.org/pokewalker-css

How it works
Apple Health -> iOS Shortcut -> Neocities upload API -> steps.json -> widget page (fetches every 5 min)

A static site can't read your phone, so the phone pushes a small JSON file to the site and the widget reads it.

Files
File	Purpose
pokewalker-css.html	The widget page (markup and the fetch logic). Embed this in an iframe.
pokewalker.css	All widget styling, including the three sprite variables at the top.
steps.json	The data file the widget reads. Overwritten by the Shortcut.
sprite-editor.html	Static page to preview a sprite and export a new pokewalker.css.
pokewalker-img.html, Pokewalker.png, Screen.png	Optional alternate version built from image layers. Not used by the CSS version.

Keep pokewalker-css.html, pokewalker.css and steps.json together, or update DATA_URL in the HTML to wherever steps.json actually lives.

Data format
json
{"steps": 4231, "updated": "2026-10-12T14:30:00-04:00"}
steps must be a plain number (no commas, not a string).
updated should be an ISO 8601 timestamp.
Widget behavior
Fetches steps.json on load, then every 5 minutes, with a cache-busting query string.
Shows "Updated X ago" under the label.
If the data is older than 6 hours, the line reads "Stale: X ago" in red.
If the file can't be loaded or is malformed, the number shows -- and the line says "Can't load steps.json".
The big center button refreshes immediately. The left and right buttons are decorative.

Settings are constants near the bottom of pokewalker-css.html: DATA_URL, POLL_MS, STALE_MS.

Embedding
html
<iframe src="pokewalker-css.html" width="300" height="300" style="border:0" title="Step counter"></iframe>

The widget keeps a square aspect ratio and scales to the iframe. It uses container query units (cqw), so it needs a reasonably modern browser.

Changing the sprite
Upload the sprite image to your Neocities folder (hotlinking breaks if the other site moves or blocks the image).
Open sprite-editor.html, paste the sprite's URL or path, and adjust size and pixelated scaling in the preview.
Click Copy CSS or Download pokewalker.css.
Upload the exported pokewalker.css to Neocities, replacing the old one.

The variables it changes, if you'd rather edit by hand:

css
:root {
  --sprite: url("sprites/pikachu.png");
  --sprite-h: 22;           /* sprite size, roughly 8 to 25 */
  --sprite-render: pixelated; /* use "auto" for non-pixel art */
}

A relative path in the CSS resolves from the CSS file's folder, not the HTML file's.

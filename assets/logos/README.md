# Client logos

Intentionally empty. The social-proof marquee in `index.html` renders styled
**text wordmarks**, not images — that is the design, not a fallback, so the page
requests no files from this folder.

To switch an item to real artwork: drop the file here, then in `index.html` give
that `.logo-item` an `<img src="assets/logos/name.png" alt="Name">` and remove
its `noimg` class. Images render greyscale and reveal full colour on hover.

Transparent PNG or SVG, roughly 200px tall, lowercase hyphenated filenames.

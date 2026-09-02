# Old Time Radio Player Project

## Environment

- The host computer is an Acer Chromebook Plus 515.
- Development files run inside the ChromeOS Linux development environment (Crostini).
- Chrome runs on the ChromeOS host, outside the Crostini container. A Linux path such as `/home/jerrywiltz/projects/old_time_radio/superman-player.html` may therefore not be directly accessible to a Chrome bookmark using the equivalent `file:///home/...` URL.
- When diagnosing local Chrome access, account for the ChromeOS/Crostini filesystem boundary. Prefer opening a file through ChromeOS **Linux files** and bookmarking the successfully opened page, or serve the project over a local HTTP server when stable browser URLs are required.

## Source

- Old Radio World homepage: https://www.oldradioworld.com/
- Show pages use URLs such as `https://www.oldradioworld.com/shows/Superman.php`.
- Episode audio is hosted under `https://www.oldradioworld.com/media/`.

## Adding a Show

When the user asks to add another Old Radio World show, perform the following process:

1. Download the requested show page unchanged into `raw/`, retaining its original PHP filename. For example, `Superman.php` is saved as `raw/Superman.php`.
2. Old Radio World may return HTTP 406 to a default command-line request. If needed, request the page with normal browser headers, redirects enabled, and compressed-response support.
3. Extract every MP3 link matching `/media/*.mp3` from the downloaded page. Preserve the order in which the links appear on the source page.
4. Create a standalone player HTML file at the project root. Use a lowercase, hyphenated filename such as `superman-player.html` or `bold-venture-player.html`.
5. Match the existing player code and styling. Use one of the current player files as the template.
6. Customize the document title, heading, episode filenames, and display-title cleanup for the new show.
7. Keep audio remote. Construct each URL from `https://www.oldradioworld.com/media/` plus the encoded source filename; do not download MP3 files into the repository.

## Required Player Behavior

Every player should provide:

- An episode list generated from the source filenames
- A sticky HTML audio player
- Previous and Next buttons
- A Shuffle checkbox
- A checked-by-default `Play next automatically` checkbox
- Sequential wraparound from the final episode to the first
- Random Next and automatic selection when Shuffle is enabled
- Protection against immediately repeating the current episode while shuffling
- Active-episode highlighting and scrolling into view

Previous remains sequential even when Shuffle is enabled.

## Validation

After creating a player:

1. Parse its inline JavaScript to catch syntax errors.
2. Compare its MP3 filename array with the links extracted from the corresponding file in `raw/`.
3. Confirm the number, spelling, and order of player entries exactly match the source.
4. Confirm the `previous`, `next`, `shuffle`, and `autoNext` controls are present.

Do not modify downloaded files in `raw/`; they are reference snapshots of the source pages.

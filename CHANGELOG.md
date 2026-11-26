# Changelog

All notable changes to Reader (formerly Read Later and Reading List) are recorded here. Versions follow
[Semantic Versioning](https://semver.org): a major version for changes that need
a library conversion, a minor version for features and redesigns, and a patch
version for fixes. Each release is a `vX.Y.Z` tag on `main`.

## [0.3.9] - 2026-11-26

### Changed
- Group by publication: each publication row matches an article row's height,
  shows the publication's icon and name (with its web address underneath), and
  drops the duplicate count badge. Opening a publication shows its name in the
  header; feed drill-ins use the same header.
- Choosing articles to delete or for a magazine shows a tidier card with the
  count, Cancel and the action on one line, and lists articles with checkboxes
  even while grouped by publication.
- Popup menus (list actions, publication and feed actions) use full-width rows
  with ordinary text, separators and red destructive items.
- Feed errors are one quiet line with Try again; a feed that is queued or being
  checked no longer shows its previous error.
- The reader's font picker matches the other text controls; the Text | PDF
  switch sits beside them, and text controls hide in PDF view.
- The reader's "Fetched" line sits closer to the byline.
- Status tags are pill-shaped, disabled primary buttons stay legible, dialogs
  dim the window more in Dark, and the Unread/Read switch is slightly larger.
- Magazine editions list their articles in equal-width cards, and word counts
  use thousands separators.
- WhatsApp and Import / export share one page width; import activity shows only
  non-empty counts.

### Fixed
- Paragraphs no longer start with a stray space when the page's HTML indents
  its text.
- Grouped publication rows keep their height while hovered, and page scroll
  bars sit at the window edge instead of beside the content.
- Import activity says "1 record", not "1 records".
- The unread marker in Favorites and Archive sits in the left margin instead of
  under the publication icon.

## [0.3.8] - 2026-11-10

### Added
- Google Drive share links to PDFs save the PDF itself (through Drive's
  download link, including the large-file confirmation). The article keeps the
  share link as its URL.
- YouTube and Vimeo players in RSS articles: feed bodies now turn embedded
  players into the reader's video figure, as saved pages already did.
- Scanned PDFs with no selectable text are saved and open in PDF view.

### Changed
- The in-reader PDF view caps portrait pages at a readable width; landscape
  pages still fill the reader.
- Embedded posts from X/Twitter show as X's own embed in the reader (avatar,
  media, quoted post, in the reader's light or dark theme, sized to the post;
  links open in the default browser). Without a connection the post shows as
  a quoted card: author, text, and a dated "View on X" link.
- WhatsApp: the link no longer shows you as online, so your phone keeps its
  notifications.
- The repository is now `rakshithxaloori/reader-macOS`. Check for Updates
  follows GitHub's redirect when the repository is renamed, so later renames
  do not break it.

### Fixed
- The open-in-browser toolbar button and Open original now open the article
  in the default browser.
- Opening a saved PDF no longer shows "The image format could not be
  determined".
- YouTube and Vimeo players respond to clicks: play, pause and the player's
  controls work (the text view's own click handling no longer intercepts
  them), and links out of a player, such as "Watch on YouTube", open in the
  default browser. The empty gap below a video is gone.
- Embedded X posts on saved Substack pages are recognized too, including a
  quoted post.

## [0.3.7] - 2026-11-08

### Changed
- A saved PDF link shows a Text | PDF switch in the reader toolbar. PDF shows
  the original document in place, inside the reader, instead of the
  extracted text; Text goes back. The choice holds for the session, and the
  book button still opens the PDF in its own window.

## [0.3.6] - 2026-11-08

### Added
- A Favorites tab between Inbox and Archive (View > Favorites, ⌘8). The star
  in the reader toolbar favorites an article without changing its read,
  archive or cache state, and Favorites lists it wherever it lives. Export
  and import carry the favorite date.
- Animated GIFs play in the reader, and YouTube, Vimeo and direct
  mp4/m4v/mov/webm video embeds play inline. Players pause when the reader is
  hidden, and the article keeps scrolling with the pointer over a video.
- Save a local copy of an article from the reader toolbar so it stays readable
  offline; it is never evicted and can be removed again.
- Links to PDFs can be saved, from the app or from WhatsApp "save" messages.
  The reader shows the PDF's text as reflowed prose (title, author, reading
  time and search come from it), and the toolbar's book button opens the exact
  PDF pages in the PDF viewer. PDFs up to 15 MB are kept in the article cache;
  a PDF with no selectable text shows a notice and still opens in the viewer.

### Changed
- The reader's text style and size controls sit behind an "Aa" button.
- The library database gains a `favorited_at` column (schema version 3),
  applied automatically on first launch; existing articles start unfavorited.

### Fixed
- Articles open quickly after a restart. The article cache parsed and wrote
  its JSON one byte per system call, so opening a large cached article made
  millions of reads. Every list load also read each publication icon from the
  cache one at a time, rewriting the cache index with two full disk flushes
  per icon, then the icon worker read them all again, and every storage owner
  rewrote the unchanged index on creation. Opening an article waited behind
  that work. Cache files are now read and written whole, icons are read in one
  pass with one index write and not read twice, and an unchanged index is
  never rewritten.
- Extraction keeps an HTML `<video>` element's own file link instead of the
  page URL.
- Links on WordPress.com and other IPv4-only sites no longer fail with "URL
  resolves to a private network" on networks that use DNS64/NAT64. Synthesized
  `64:ff9b::/96` addresses are judged by the IPv4 address they embed.

## [0.3.5] - 2026-11-05

### Added
- Grouped publications have a redesigned row with an initial tile, a count and
  a hover menu with "Archive all articles in this publication" and "Delete
  all…". The same archive action is in the list header menu inside a
  publication or feed.
- Clicking a feed in Feeds opens all of that feed's articles, read or unread,
  archived or not.

### Fixed
- Links in the reader show the pointing-hand cursor.

## [0.3.4] - 2026-11-04

### Changed
- The library has no size limit. Saving, importing, feed intake, magazine
  production and updates no longer fail with "Storage budget exhausted". The
  only size budget is the app: `Reader.app` must stay below 50 MB, and
  `bundle.sh` fails a bundle that is not. `storage-audit` reports library size
  and checks only the bundle.

### Fixed
- Install and Relaunch failed with "Storage budget exhausted" on libraries
  near the old 100 MB limit. Updates now download into the system temporary
  folder, check free disk space instead, and clean up on success and failure.
- Reader no longer refuses to start when the app and library together pass
  100 MB, and the library database no longer stops growing at 26.5 MB.

### Upgrading
- Reader 0.3.3's updater can fail with "Storage budget exhausted", so 0.3.3
  users may need to download 0.3.4 once by hand. Updates work from 0.3.4 on.

## [0.3.3] - 2026-11-03

### Added
- "Check for Updates…" in the app menu, and a daily launch check that can be
  turned off with "Check for Updates Automatically". When a newer release
  exists, a banner offers Install and Relaunch, Later, or Skip This Version.
  Install downloads the release's `Reader.dmg` (or `Read-Later.dmg` from
  older releases), verifies the app inside, swaps the bundle after quitting
  and relaunches. A copy that cannot replace itself opens the release page in
  the browser instead.

### Changed
- The app is now called Reader. The bundle is `Reader.app` and the release
  disk image is `Reader.dmg`. Updating from Read Later replaces
  `Read Later.app` with `Reader.app` in the same folder. The bundle
  identifier, library location, Keychain item and settings are unchanged, so
  existing libraries open as before.
- The repository is now `rakshithxaloori/macOS-reader`.

### Known issues
- The app is ad-hoc signed, so after each update macOS asks again before
  Reader can read its WhatsApp key from the Keychain.
- A Dock icon pinned to `Read Later.app` needs pinning again after the update
  turns it into `Reader.app`.
- An update briefly needs room for two copies of the app, which can take the
  app and library above 100 MB until the old copy is removed.

## [0.3.2] - 2026-11-03

### Added
- `scripts/dmg.sh` builds a downloadable `Read-Later.dmg` with a portable app
  that opens its own library in Application Support.
- `bundle.sh --portable` builds that shareable bundle.
- README sections on downloading the app and on forking it with a coding agent,
  and `docs/hacking.md`, a map of the code for agents.

### Changed
- `bundle.sh` points the app at `.local/native` only when that library exists,
  so a fresh clone or fork uses Application Support as documented.

## [0.3.1] - 2026-10-30

### Fixed
- "Database command could not be queued: sending on a full channel" no longer
  appears after the window was hidden or covered during feed checks. Any number
  of library changes now causes one reload, and a reload that finds the queue
  full is retried instead of being dropped, so the list cannot stay out of date.

## [0.3.0] - 2026-10-30

### Fixed
- RSS feeds hosted on WordPress.com (192.0.78.0/24) no longer fail with "URL
  resolves to a private network". Only 192.0.0.0/24 and 192.0.2.0/24 are
  treated as special-purpose.
- Full-text feeds no longer exhaust the storage budget on replay. Lean replay
  reserves space per entry rather than four times the XML size, so one large
  feed can no longer fill the shared replay buffer and block other feeds.
- Section headings with "header" classes (as on Substack) and horizontal rules
  inside wrappers are no longer dropped during extraction.
- The reader now draws horizontal rules. AppKit ignored the RTF border, so
  rules were invisible even when kept.
- Videos are no longer silently removed. Each one becomes a "Video · Watch on
  the original page" (or YouTube/Vimeo) link where the player was.
- Substack tweet embeds become a quoted post with the author, date and link
  instead of loose avatar, name and view-count lines.

### Changed
- Reader quotes are upright with an accent bar instead of indented italics,
  section headings (h3 and below) are smaller than top-level ones, and
  images get space before the next paragraph.
- Redesigned the Feeds screen. Feeds are grouped into Needs attention,
  Following and Paused cards with publication monograms, status dots,
  relative check times, a summary line, and a filter once there are six or
  more feeds. Errors show inline with Try again, and the More menu adds
  Copy feed address.
- Renamed the app to Read Later. The bundle is `Read Later.app`, the crate and
  executable are `macos-read-later`, and the repository is `macOS-read-later`.
  The bundle identifier, Keychain service and portable export format keep their
  old identifiers so existing libraries and WhatsApp sessions keep working.

## [0.2.2] - 2026-10-28

### Fixed
- Reader toolbar hover labels open beside their buttons instead of below,
  where the article view hid them.

## [0.2.1] - 2026-10-28

### Changed
- Article rows no longer show a warning triangle. A failed copy says
  "Unavailable" and a pending one "Fetching" in the row's metadata line;
  partial copies are explained in the reader.

## [0.2.0] - 2026-10-26

### Added
- Versioned releases: the bundle version comes from `Cargo.toml`, the build
  number from the commit count, and **Reading List → About Reading List** shows
  both. `scripts/release.sh X.Y.Z` verifies, tags, bundles and archives a release.
- A new app icon: an ivory open book with a copper bookmark on deep green.
  `packaging/macos/icon/make-icon.sh` regenerates it.

### Changed
- One continuous window: the panes run under a transparent title bar.
- Inbox and Archive header fits on one line: title and count, an Unread/Read
  segmented control, and search with a filter-and-sort popover.
- Rows show the publication name and reading time; only failed copies use the
  warning color.
- Reader actions (re-fetch, mark read, archive, delete, open original) moved
  from inside the article to toolbar icons. Reader line height is now 1.6.
- Feeds are compact single rows, so about twice as many fit on screen.
- Warmer, deeper Dark appearance; the rail's "Yours." label is gone.

### Removed
- One-time migration, compaction and cutover commands (`migrate`, `compact`,
  `cutover`) and their tests. Libraries still in the retired full-copy format
  must be converted with v0.1.0 first.

### Fixed
- The app icon no longer turns blank when the window is closed or the app quits;
  the bundle now ships `AppIcon.icns`.
- The Magazines heading read "FROM SCREEN TO colors.paper".

## [0.1.0] - 2026-10-22

First versioned baseline.

- Native Rust reader (egui, AppKit text views, PDFKit) with Inbox, Archive,
  Feeds, Magazines, WhatsApp capture and Import/export.
- RSS/Atom intake keeps older articles when a feed's window slides, with
  15-minute polling and crash-safe replay.
- WhatsApp self-chat `save <link>` capture with fresh native pairing.
- Lean storage: app plus all managed data stays under 100 MB, with a seven-day,
  20 MB on-demand article cache (Oct 18).
- Reader font picker: Georgia, Baskerville, Avenir Next, Helvetica Neue (Oct 18).
- Intake resumes correctly after sleep (Oct 21).
- Read Inbox rows fade instead of showing status dots (Oct 21).
- Light and Dark appearance, shared control family, vector icons, clearer reader
  recovery and a visual magazine workflow (Oct 22).

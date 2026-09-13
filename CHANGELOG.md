# 0.1.2-rc (released)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

Release candidate on top of `0.1.2-beta2`: Windows resize and wheel, stable animation loop, smarter covers, artist poster background, centered empty states.

Tag: `v0.1.2`  
Title: `0.1.2`


### Added
- Artist poster background: a new Settings toggle uses the band photo from the artist folder as the page background on artist pages (a hint in Settings explains where to put the photo).
- Windows: the app now uses the immediate presentation mode by default — scrolling and animations lost the small stutter of the vsync queue; the mode can still be overridden with the `ICED_PRESENT_MODE` environment variable.

### Fixed
- Animations no longer freeze when the mouse leaves the window: card caption marquees, the tab pill and every other animated element keep running without pointer movement (a hit-testing gate was eating window redraw events; the frame arbiter counts every animation source, and content pages always keep their frame loop).
- Media keys no longer act "every other press": the system media overlay is told the playback status immediately after each action (it decides which button to send next based on the status we report), and repeated quick presses are no longer collapsed into one action.
- Windows: resizing by any edge or corner is smooth — the resize keeps the mouse captured (fast pulls no longer break off), window geometry is applied atomically so there are no distorted intermediate frames, and the edge follows the true cursor position instead of oscillating. Grids keep their morph animation while resizing.
- Windows: mouse-wheel scrolling reaches the scroll physics — discrete wheel clicks get their own gentle timing instead of the fast trackpad curve, and each click travels further (176/140 px for grids/lists).
- 24-bit audio is no longer truncated to 16 bit during decoding in the gapless mode — the bit-perfect indicator shows the real file depth.
- Cover picking prefers the real front artwork: inserts, booklets, vinyl backs and "Back" files no longer beat the front cover; large vinyl scans up to 64 MB are accepted (was 20 MB).
- Artist posters are normalized to a true JPEG when cached — photos stored as WebP bytes under a .jpg name no longer break the renderer (the background of some artists did not show).
- Scrolling huge grids no longer touches the disk for every visible cover (lookups are cached), and a full rescan now cleans up orphaned files in the covers cache.
- Removing the last track from Favorites now crossfades to the empty page right away — no lingering gray cover ghost, and the hint text appears in time.
- Leaving a tab with an open album or artist page fades the whole panel cleanly — no one-frame flash of the artist poster or the page veil.
- The tab pill spring returns smoothly on the leftmost tabs and always settles exactly on target (no snap-jump when the other animations sleep); the toast slides out from under the player island instead of fading in place.

### Changed
- Empty states are centered: the empty queue (logo + hints), empty album/artist/folder/playlist grids, and the "nothing found" search page.
- The "new version available" status line appears with a smooth height/alpha animation, and clicking it opens the releases page again.
- Release archives for all platforms are built with unified names (`nemesis-<version>-<platform>-<arch>`) from the Linux Docker pipeline.


---

# 0.1.2-beta2 (released)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

Tag: `v0.1.2-beta2`  
Title: `0.1.2-beta2`


### Added
- Search page: a magnifier button in the top island (next to the theme toggle) opens a dedicated search tab. The field morphs from a "Search" label; matching against title/artist/album is case-, spacing- and Cyrillic-insensitive (fold-key matching).
- Search results are full track rows (as on the Tracks tab: number, title, tech columns, duration) with hover +/list buttons, accent now-playing streak, scroll with the shared rail, and double-click plays the results as a queue starting from the picked track.
- AppImage packaging target: built in the Debian container (glibc 2.36 floor); bundles the binary, desktop file, icon and libasound.
- Volume diagnostics: `NEMESIS_VOLUME_LOG=1` traces every volume application — UI requests, worker commands, pactl/WASAPI writes, fades, watchdog restarts.

### Fixed
- Grid scroll no longer lags or stutters during wheel and inertia.
- End-of-scroll jitter removed — smooth stop instead of tiny bounce.
- In-app volume no longer resets to a stale value: the app no longer mirrors volume into MPRIS nor accepts desktop SetVolume events, so PipeWire stream restore cannot overwrite the slider — and the volume no longer drops mid-track when the system recreates the audio stream.
- Album/artist/folder/playlist pages no longer stutter when scrolling long lists (2000+ tracks): per-frame O(n) work (facts, row offsets, now-playing lookup, plus-states) is cached or made allocation-free.
- The invisible underlay grid is no longer rebuilt or repainted every frame while a page is open.
- Removing the last track from the queue now plays the same smooth leave animation as the Stop button (the page and cover fade out instead of snapping to the empty state).
- Playback no longer gets stuck: if sound stops, the player recovers by itself and moves on to the next track.
- Tracks keep switching automatically even when the window is minimized.
- When you switch tracks, the player immediately shows the new one — no leftovers from the previous track on screen.
- Deleting a playlist now goes smoothly, without the list jumping.
- Saving a large queue as a playlist no longer freezes the app.
- The queue page scrolls just as easily with thousands of tracks as with a short one.
- Windows: grid rebuild animation no longer freezes while resizing by the window edge (winit#3272 — the event loop does not resume during the modal resize loop, so `RedrawRequested` ticks stop; a fallback tick timer keeps the morph running until it settles). Minimize/restore no longer replays the grid rearrangement: degenerate minimized sizes are ignored and the animation restarts only on a real viewport change.
- Windows build fixed: the WASAPI output thread was left on the old 8-argument signature.

### Changed
- Scroll wheel speed increased by 30% for all grids, queues, and album pages.
- “Save queue as playlist” on the queue page now looks and animates exactly like the “New playlist” button in the playlist picker island (accent label, morph into the name field, same confirm/cancel chips), sized to the queue panel.
- Many reliability improvements under the hood: the app starts faster, handles its music database and covers more carefully, and stays stable in situations that previously could cause glitches or a hang.


---

# 0.1.2-beta (unreleased)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

Tag: `v0.1.2-beta`  
Title: `0.1.2-beta`

### Added
- Windows support: the same app now runs on Windows — system media keys and media overlay (SMTC), system light/dark theme, regular window with rounded corners, and native format output through WASAPI.
- Playlists tab: covers grid and page like albums — create, add tracks from a cover or a track menu, delete with the same smooth collapse as the queue, rename by clicking the title.
- Interface in Russian and English — switch in Settings, defaults to the system language.
- CUE sheets: cue-based albums play as one album with CD1/CD2 and side labels.
- "About" page with app info, links, and a once-a-day update check with a badge on the settings button.
- Fullscreen cover screen (the cover button in the player), separate from window fullscreen.
- Bit-perfect indicator in the player with the file's bit depth and rate.
- Artist pages open on a large poster from the artist's folder, in the same rounded style as covers.
- Queue improvements: album covers and dividers in the queue, a colored bar for the playing track, and removing tracks/albums with a small × (the list smoothly closes the gap).
- Session memory: shuffle, repeat, and the open tab survive an app restart.
- Ready-to-install builds for Linux (deb, rpm, Arch, AppImage, tar.gz) and a zip for Windows.

### Fixed
- The app no longer breaks after a long minimize — the queue keeps playing and stays intact; closing the app is instant.
- Tracks keep auto-advancing when the window is minimized.
- Bit-perfect mode no longer produces clicks and pops; the queue no longer desyncs on a damaged track.
- Native format on Windows no longer rejects files whose sample rate differs from the system mix — the file plays as is (the system mixer may resample in shared mode).
- The Windows volume slider in native format changes the actual WASAPI session volume.
- All-tracks list groups CD1/CD2 and sides like the queue and album page, instead of mixing them by track number.
- The "in queue" checkmark on album and artist cards follows the canonical card, so featured artists and CD1/CD2 no longer hide it.
- Next/prev no longer flings the queue past the playing track if the list was scrolled away.
- The library no longer collects files the player can't decode (for example, opus).
- The spectrum renders at the correct width; Stop→Play and shuffle/repeat behave as expected.

### Changed
- Scrolling feels smoother everywhere: no rubber-band bounce, smooth decay at the end of a fling, and faster wheel response across grids, the queue, and album pages.
- The "+" and "×" buttons on tracks and covers appear only on hover.
- Album, artist, and folder grids slide cards to their new cell on window resize, same as playlists.

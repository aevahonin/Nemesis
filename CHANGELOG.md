# 0.1.6 (release)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

The `0.1.6` release, on top of the `0.1.5` release: the settings page is rebuilt around tidy cards, the equalizer looks cleaner, the window corners are noticeably smaller, and the Flatpak build regains its desktop integration — on top of a Windows window-restore fix.

Tag: `v0.1.6`  
Title: `0.1.6`  
Released: 2026-10-23


### Changed
- Settings are rebuilt around cards: labels no longer scroll as marquees but stay put; every card sizes itself to its content (descriptions cap at three lines). "Equalizer" became a parent card with a master toggle, and dependent visualization settings now dim and come back with a smooth fade.
- Equalizer: the fill under the curve is now a single flat tone with a clean break exactly at the zero axis — no stair-steps, no stray lines, no on-screen artifacts.
- Linux: window corners are rounded noticeably less — the 16 px radius gave way to 8 px, matching modern apps (Steam, Discord). Maximized and fullscreen windows have square corners.
- Linux: the "No transcoding" (PipeWire) setting is actually applied now — it used to be written in a form PipeWire silently ignored.

### Fixed
- Windows: restoring the window from a minimized state could draw the title bar stretched across the whole screen — after restoring, the window now paints its full contents right away.
- Linux (Flatpak): in the Flatpak build, external control of the player did not work — keyboard media keys and track information in the desktop shell: the player's D-Bus name was not covered by the sandbox permissions, and the service thread crashed. It works now.


---# 0.1.5 (release)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

The `0.1.5` release, on top of the `0.1.4` release: a sound processing page with an equalizer, the app arrives at Microsoft Store, the Flatpak bundle is back, crossfade now works in bit-perfect mode, and library rescans plus startup covers got much faster — on top of another long pass of scrolling, covers and stability fixes.

Tag: `v0.1.5`  
Title: `0.1.5`  
Released: 2026-10-04


### Added
- A "Sound processing" page: the faders button in the top island opens a graphic equalizer with seven presets — Flat, Bass, Vocal, Rock, Pop, Jazz, Classical — and a preamp (−12…+12 dB). Picking a preset is heard immediately, and the band markers reshape themselves to follow the curve.
- Microsoft Store (Windows): the app now also ships as an MSIX package for the Store. In a Store installation the in-app update check is hidden — keeping the app up to date is the Store's job, so the two never update over each other.
- Flatpak builds are back: a `.flatpak` bundle is attached to the release on GitHub and installs with `flatpak install`.

### Changed
- Crossfade works in bit-perfect mode now: the "smooth transitions" setting no longer stands idle there, and manual switching to the next/previous track also glides over with a crossfade instead of cutting the stream.
- Library rescans are noticeably faster: files that have not changed (checked by modification time and size) are skipped instead of being re-read and re-parsed.
- The app redraws itself less when nothing is happening — a lighter idle load, especially on pause.
- Linux: the app now installs under a new application identifier, `io.github.aevahonin.Nemesis` (previously `com.nemesis.player`) — the applications menu entry and the icon are tied to it. For already installed versions: an icon pinned to the taskbar will reset after the update — pin it again.

### Fixed
- Track repeat: when the same track repeated (or two identical tracks played in a row), the title in the player could switch to the next track a couple of seconds before it actually began — the title now changes exactly at the junction.
- Volume: when the system could not spare resources for the volume helper thread, changing the volume could freeze playback for seconds — the volume now applies quickly and as well as possible in that case.
- Pausing right at the junction between two tracks no longer breaks the playback position and no longer clips the beginning of the next track.
- Settings: running two copies of the app at the same time can no longer corrupt the window-size and theme settings files, and temporary truncated files left behind in the data folder after a crash are cleaned up on startup.
- Scrolling: no more twitching and rhythmic "bumps" at the end of a scroll — the motion now glides gently into place and stops, with the whole tail within half a second.
- Covers no longer "swim" or "rebuild themselves" near the end of a scroll: cards are drawn with exactly-sized images, without re-scaling on every step.
- Covers no longer take many times longer to load at startup: the first screen fills in through the fast path again (warmed up in several threads), and a rescan no longer makes every visible cover reload.
- Rescanning no longer creates duplicate tracks when the same folder arrives under a different letter case (Windows), and libraries carried over from older versions no longer double their CUE tracks on a rescan.
- When one audio file has two .cue sheets, the first one wins instead of mixing segments from both.
- Tracks with legacy non-UTF-8 file names (old CP1251 rips) no longer end up in the library under broken names they cannot be opened from.
- A whole drive (`C:\`, `/`) can be added as a scan folder.
- The letter "ё" no longer sorts after "я": an artist like "Ёлка" now stands between Е and Ж.
- Unsynced lyrics on the "Now Playing" page no longer jump back to the beginning after a manual scroll; Esc and mouse-back close the lyrics variants popup instead of the whole page.
- The lyrics island above the player now disappears immediately when turned off in settings — before, it only went away after restarting the app — and its state is remembered between launches.
- Old .lrc files saved in the CP1251 encoding are read again, and .lrc files saved by the app open in Windows text editors without mojibake.
- The remembered window position now works on monitors to the left of or above the primary one (negative coordinates used to be discarded), and minimizing the window no longer corrupts its saved size.
- Windows: dragging and resizing the window no longer freeze animations; if you prefer the even smoother system drag, a new Settings toggle, "System window dragging", returns it (in-app animations pause while the system drags).
- Windows: files under paths longer than 260 characters (deep NAS collections) are no longer silently skipped when scanning.
- Plus another round of small fixes: m3u playlists written on the other OS (with forward slashes) find their files, files for CUE and playlists are found regardless of Cyrillic letter case on Windows, removing a scan folder during a rescan no longer leaves ghost tracks, and clicking the heart can no longer bring the app down on machines starved for resources.


---


# 0.1.4 (release)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

The `0.1.4` release, on top of the `0.1.3` release: song lyrics with the live line above the player, a real Windows installer, a "Now Playing" page with the track's waveform, track change notifications on Linux, honest resampling for mismatched sample rates — plus another stability pass and a cheaper, smoother frame.

Tag: `v0.1.4`  
Title: `0.1.4`


### Added
- Song lyrics: text from local .lrc files and audio tags; the current line shows above the player in a small pill, and the "Now Playing" page shows the full lyrics following the playback — with section markers ("Chorus", "Verse"). A text can also be searched for manually, with a choice between variants; everything works with CUE rips too, where every segment gets its own times. A master Settings toggle turns lyrics off entirely, and the network download can be switched off separately.
- A Windows installer: the app on Windows now installs through a real setup.exe — "for the current user", without administrator rights. The installer itself comes in English and Russian, the app gains an icon in Explorer and on the taskbar plus proper version information, and an upgrade or uninstall gently closes a running player (the app also exits cleanly when Windows shuts down). The portable zip is still there — both options are available, plus builds for Windows on ARM.
- A "Now Playing" page: opens by clicking the track title in the player island. The cover, the track's waveform across the full window width (left and right channels drawn apart, bar heights follow the loudness), click or drag the wave to seek, transport buttons, an "Up next" line, and the artist/album lines open their pages. The page follows the playing track; Esc closes it and the pages opened from it.
- Track change notifications (Linux): an optional system notification with the title, artist and album (a Settings toggle). On Windows they will arrive in a future update.

### Fixed
- Large album covers and artist posters no longer crash the app while rendering: full-size pictures are scaled down to a reasonable size (1600 px) before they are drawn.
- Spectrum: the first low-frequency bars no longer twitch in lockstep with each other (each bar now measures its own frequencies); the peak caps no longer disappear on quiet bars and fall off smoothly while paused.
- The Settings page starts right below the top island (it used to sink far down), and on Windows its scrolling reaches the very bottom.
- The lyrics panel on the "Now Playing" page keeps its scroll anchored: the start of the text no longer scrolls above the middle and the end still reaches it, and after a manual scroll the text holds its place for 3 seconds instead of 5.
- Mismatched sample rates: a track whose sample rate differs from the output (a 48 kHz file on a 44.1 kHz device and so on) no longer plays as full-volume static with bit-perfect off — it is honestly resampled to the output.
- Rare clicks on tracks whose sample rate differs from the output (a FLAC at 44.1 kHz on a 48 kHz device, for example) when played through the regular output path.
- An unavailable network storage no longer wipes the library on rescan: folders that cannot be read are skipped instead of being treated as empty.
- Covers: the covers of an album that has disappeared are removed from the cache for real, and temporary files no longer pile up as garbage after a crash.
- Playback: "Previous" on pause no longer leaves the player silent with a Play button that does nothing.
- A damaged file with a degenerate container can no longer freeze the decoder — and the whole player with it — for good.
- Windows: switching the output device now applies on the fly, without restarting the app; dragging the window no longer freezes marquees and animations; scrolling feels the same as on Linux.
- Lots of small fixes from another bug-hunt pass: row selection no longer jumps between tabs and search, the queue and sorting behave, and failed library and playlist operations show clear messages.

### Changed
- The update check is now enabled by default (it can still be turned off in Settings).
- Smoother and faster: every frame costs less (the mouse and scrolling respond faster), and metadata scanning of large libraries now uses all CPU cores in parallel.


---
# 0.1.3 (release)

> Русская версия: [CHANGELOG_ru.md](CHANGELOG_ru.md)

The `0.1.3` release, on top of the `0.1.2` release: a visualization screen, ReplayGain volume normalization, sorting for the Tracks list, translucent islands, playlist import, Opus and ALAC decoding, a real crossfade — and a big stability campaign, with more than a hundred fixes from six independent bug-hunt passes.

Tag: `v0.1.3`  
Title: `0.1.3`


### Added
- Visualization screen: a spectrum around the cover of the playing track right on the player island, and an expand button that takes it fullscreen — bars rise from the bottom edge of the screen, with peak-hold caps above them that fall back to the base while paused. The fullscreen mode shows a large cover (a click sends it to the background) and hides the cursor after 3 seconds of inactivity; a Settings toggle turns the cover display on or off.
- ReplayGain volume normalization: from tags, with a target loudness (−24…−12 LUFS, −18 by default) and a switchable clipping guard; an indicator next to the player shows the normalization status (it gets crossed out when normalization is off).
- Sorting for the Tracks list: a card with Artist/Album/Genre/Year buttons — several keys at once, applied in the order they were pressed; after sorting is applied, the rows fade in as a cascade. Favorites always keeps its adding order.
- Translucent islands: a "Translucent islands" setting — the amount of transparency is set with −/+ buttons (5% steps), 5% by default.
- Playlists: import of `m3u`/`m3u8`/`pls` (old m3u files in CP1251 are decoded too).
- Opus and ALAC decoding.
- A "−" button instead of the "×" on queue and playlist tracks and on the playlist cover in the grid: the minus means "remove from this list", while the cross stays for real deletion and closing.
- Reworked Settings: uniform cards with descriptions, two columns on wide screens, scrolling with the same physics as the lists; the system language is picked automatically (English as the fallback); new toggles default to off.

### Fixed
- Crossfade: the transition now really lasts as long as the setting says (before, the old track was cut off after about 0.7 s, so every transition came out short); switching tracks in the middle of an unfinished transition no longer produces clicks or volume jumps.
- Bit-perfect: seamless joints between tracks without clicks (the next track is stitched into the same stream); changing the crossfade length mid-transition no longer "blows up" the volume of the outgoing track.
- Gapless: a slowly reading disk no longer skips the next track at the joint; the seamless hand-off no longer breaks shuffle (every second joint used to start unprepared); quickly pressing next/prev no longer loses the prepared next track.
- Opus: the click at the start position after a seek and at the beginning of a CUE segment is gone; Opus files with more than two channels give an honest error instead of scrambled channels.
- Row highlighting no longer "jumps" to other tabs: when the track changes, the queue is cleared, a rescan runs, sorting is applied or a heart is removed, the selection stays where you put it.
- Favorites: removing a track no longer shows a wrong track in the sidebar; an empty Favorites page appears right away, without a lingering gray panel.
- Covers: a failed thumbnail write no longer caches a broken picture forever; two copies of the app writing the cache at the same time no longer corrupt each other's files.
- Library and data: on Windows, scan folders find files regardless of letter case; damaged database records (infinite durations, giant track numbers) no longer crash the player; deleting a scan folder or a playlist is atomic; a session with a broken position is restored safely.
- Overall stability: six independent bug-hunt passes fixed more than a hundred defects in audio, data and the interface — from rare hangs in the cover pipeline to a lost position when switching tracks quickly.

### Changed
- The default volume is now 100% (was 80%).
- Noticeably faster on large libraries: sorting and search run tens of times faster (instant on tens of thousands of tracks), metadata scanning uses all CPU cores in parallel, scrolling huge lists is smoother, and the idle load is lower.
- Updated icons: the normalization indicator (volume bars / crossed-out), the visualization "expand" button, and the "remove from list" minus.


---

# 0.1.2 (released)

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

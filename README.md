# Kids Mode Parent Plus for Onion OS
DO NOT DOWNLOAD YET. NOT ALL MODES IMPLEMENTED YET
Most of this README is the same as Veuks'. I will reupload a new one when I am finished testing and uploading
This is a placeholder description for an update on the Onion Kids mode by Daverad based on the fork by Veuks. This is being tested on a Miyoo Mini Flip

Disclaimer: I have learned qBasic and Visual Basic in High school 25 years ago and C++, JavaScript, and Ruby in college 20 years ago and haven't done much besides teaching my middle school students Code.org and 
picking up some HTML5 here and there. Because of this, I used Claude Sonnet 5.5 almost exclusively in the development of this project.
I did go into the code here and there and checked some things as I know enough to do basic troubleshooting, but frequent testing followed by prompting to the AI was the majority of my debugging.

## Highlights

The following were included in the Veuks fork as well as the original Daverad kids mode
- One protected app for favorite games, children's videos and audio. *I don't use the video/audio mode at all so this remains untested
- Games are taken directly from Onion's Favorites list.
- Videos, audio files and nested folders for movies, series, music or stories
  are supported.
- Automatic resume follows the real shutdown state:
  - shut down while playing and that game or video resumes;
  - shut down from a carousel and the same section and selection return;
  - switching between Games and Videos restores the last selection on each.
- Video progress is saved regularly and completed videos restart from the
  beginning the next time they are opened. *Untested
- Every folder level reopens on its own last selected item.
- Optional shared session timer from 5 to 120 minutes, or no timer.
- PIN-protected parent menu with extra time, timer removal, exit, and a
  Games-only or Videos-only lock.
- Onion-style artwork, controls, colors and menus.
- ScreenScraper Mix V1-inspired reflection on video artwork for visual
  consistency with game thumbnails.
- Separate child save profile for games; the parent's saves and states are
  restored when leaving the mode. *Untested as this device is only for my child

  Parents+ additions:
  -Daily time limit automatically set (weekday/weekend)
  -Only need to enter the PIN once when time is up (original fork turned timer off and then required second input to create new timer or add time)
  -Designated play window (default 4PM - 7PM on weekdays and 8AM - 7PM on weekends) to lock the child out when they shouldn't be playing
  -A bypass button to allow for unlimited play when on vacation or for special circumstances
  -A blinking warning when at 30 seconds left. This is specifically because my child needs it. It can be disabled in the .sh file
  -Rewind and save states turned on (must first be enabled in the retroarch cores
  -Rentals. This is an optional system that allows the child to rent a game for a period. When the rental is up, it gets removed from the favorites list
  *The rental option goes with a storefront I created where my child can earn money towards purchasing or renting games

## Requirements

- Miyoo Mini, Miyoo Mini Plus, or Miyoo Mini Flip
- Onion OS 4.3 or newer
- Onion's FFplay binary, included with normal Onion installations

The current version was tested on real Miyoo Mini Flip hardware

## Installation

1. Download `Kids-Mode-parent-plus.zip` from a Kids Mode release, or use the
   `Kids-Mode-build` artifact while testing the current source. The
   historical `v1.0.0` release remains available as `KidsMode-Deluxe.zip`.
2. Extract the ZIP.
3. Copy the included `App` and `Media` folders to the root of the SD card,
   merging them with the existing folders. Do not delete other content already
   stored in `Media`. The application path must be:

   ```text
   /mnt/SDCARD/App/KidsMode/
   ```

4. Optionally add the games intended for the child to Onion's Favorites list.
5. Add videos and audio to the included category folders as described in the
   Media section below.
6. Reinsert the SD card, refresh the Apps list or reboot, then open
   **Apps → Kids Mode**.
7. On first launch, choose and confirm a four-digit PIN, then select a session
   timer or choose `OFF`.

Kids Mode remains active after a reboot until it is exited through the
parent menu.

PIN, timer state, selections and playback positions are stored under
`Saves/KidsMode`.

### Clean installation and manual file move

Exit the previous mode through its parent menu before replacing it. Back up
the SD card, then remove any former `App/KidsMode`, `App/KidsModeDeluxe` or
`App/SuperKidsMode` folder. Also remove the former `Saves/kidmode` folder:
on the SD card, `kidmode` and `KidsMode` are the same name, so leaving it in
place would reuse the former PIN, timer and state instead of starting clean.

Do **not** remove `Saves/KidsProfile`, `Saves/CurrentProfile`,
`Saves/MainProfile` or `Saves/GuestProfile`. `KidsProfile` contains the
child's game saves and remains separate from the app settings.

Copy the new `App/KidsMode` folder and launch it once. Kids Mode creates a
fresh `Saves/KidsMode` folder, PIN, timer state and playback history. Move
the videos, series folders and `Imgs` folder manually from whichever former
media folder exists:

```text
Media/VideoKidsMode        → Media/KidsMode
Media/SuperKidsMode        → Media/KidsMode
```

Move the **contents**, not the former media folder itself, to avoid creating
`Media/KidsMode/VideoKidsMode` or `Media/KidsMode/SuperKidsMode`. The old
`Saves/VideoCarousel` folder is not used and may be removed after testing.
The historical `v1.0.0` GitHub release itself is not changed.

## Media folders and local artwork

All media belongs under:

```text
/mnt/SDCARD/Media/KidsMode/
```

Supported video formats are MP4, MKV, AVI, MOV, M4V and WebM. Supported audio
formats are MP3, M4A, AAC, FLAC, OGG, OPUS, WAV and WMA. Media can be organised
through multiple folder levels. A folder appears when it contains at least one
supported file, either directly or in one of its subfolders. Entries are sorted
alphabetically.

Each directory can have its own `Imgs` folder. An image uses the same base name
as the media file beside it. PNG, JPG and JPEG are accepted. The complete
package includes four ready-to-use category folders and their 256 × 256
artwork. This organisation is recommended:

```text
Media/KidsMode/
├── Imgs/
│   ├── Movies.png
│   ├── Music.png
│   ├── Series.png
│   └── Stories.png
├── Movies/
│   ├── The Lion King.mp4
│   ├── The Visitors.mkv
│   └── Imgs/
│       ├── The Lion King.png
│       └── The Visitors.jpg
├── Series/
│   └── Ulysses 31/
│       ├── Episode 01.mp4
│       ├── Episode 02.mp4
│       └── Imgs/
│           ├── Ulysses 31.png
│           └── Episode 02.jpg
├── Music/
│   ├── Song 01.mp3
│   ├── Music video 01.mp4
│   └── Imgs/
│       └── Song 01.png
└── Stories/
    ├── Story 01.mp3
    └── Imgs/
        └── Story 01.jpg
```

For a folder cover, place an image named after that folder inside its own
`Imgs` directory. For example, `Series/Ulysses 31/Imgs/Ulysses 31.png` is used
for the Ulysses 31 folder in its parent carousel and as fallback artwork for
episodes without their own image. An exact episode image such as
`Imgs/Episode 02.jpg` takes priority. Folder captions show only the folder
name; movie and episode names remain below the selected image.

Artwork fallback follows the folder hierarchy. If the current folder has no
cover, Kids Mode searches its parent folders up to the category at the media
root. For example, content under `Series/Ulysses 31` can fall back to
`Series/Imgs/Series.png` when `Ulysses 31/Imgs/Ulysses 31.png` does not exist.
At every level, a folder cover may also be stored in its parent's `Imgs`
directory, such as `Media/KidsMode/Imgs/Series.png`; both layouts are inherited.

The same rule can give category folders their own artwork:
`Movies/Imgs/Movies.png`, `Series/Imgs/Series.png` or `Music/Imgs/Music.png`.
Without one, Kids Mode creates an automatic black folder card. The older
shared `Media/KidsMode/Imgs` layout remains accepted as a fallback, but local
`Imgs` folders take priority and prevent identical filenames in different
categories from sharing the wrong image.

Artwork is fitted inside a square without stretching, with black side bars
when necessary. Portrait cinema posters are recommended for movies. If no
image exists, the app creates a black card and wraps its title over up to six
balanced lines before reducing the font size.

Press A to descend into a folder and B to return exactly one level. Every
folder remembers its own last selected video or subfolder, including after a
reboot. Folder nesting is supported up to sixteen levels, which is effectively
unlimited for normal use.

## Carousel controls

| Button | Action |
| --- | --- |
| LEFT / RIGHT | Browse the current section |
| UP | Move from Games to Videos |
| DOWN | Move from Videos to Games |
| A | Launch/resume a game, play/resume a video, or open a folder |
| X | Restart the selected game or video after confirmation |
| B | Return to the previous folder level |
| Hold SELECT + START for 3 seconds | Open the PIN screen and parent menu |
| MENU | No action in the carousel |

UP and DOWN are unavailable while a section is locked. DOWN is also disabled
inside any media folder; press B as needed to return to the media root first.

## While playing a game

The game itself keeps its normal controls. A single press of MENU saves and
returns directly to the Kids Mode carousel. RetroArch configuration and
dangerous hotkeys are hidden while the mode is active and restored on exit.

Restarting with X skips Onion's automatic resume state but does not erase
normal in-game save data.

## Media player controls

| Button | Action |
| --- | --- |
| A | Resume playback, or wake the screen during audio playback |
| B | Pause playback |
| MENU + LEFT | Rewind progressively |
| MENU + RIGHT | Fast-forward progressively |
| MENU + DOWN | Rewind 10 minutes |
| MENU + UP | Fast-forward 10 minutes |
| MENU + X while paused | Capture the current frame for the carousel |
| MENU alone | Save the position and return to the carousel |
| Other buttons | Ignored by the player |

Holding MENU + LEFT or RIGHT repeats the seek and increases each step from
10 seconds to 1 minute. It remains at one-minute steps no matter how long the
combination is held. A small white indicator appears at the bottom-left when
rewinding and at the bottom-right when moving forward.

Audio playback shows the selected file's artwork, or the nearest inherited
folder artwork, with a thin progress line and elapsed/remaining time. After
10 seconds without input the display changes to minimum brightness; five
seconds later the backlight turns off while playback and the timer continue.
Press A, B or MENU to restore the display. That first press only wakes it and
does not pause, resume, seek or leave the player.

During video playback, the same progress line appears while paused and while
seeking. It disappears after seeking resumes and immediately when A resumes a
paused video.

Volume and brightness shortcuts remain usable. Releasing MENU after using a
combination does not accidentally leave the video.

To create a carousel image from a video, pause with B and press MENU + X.
Kids Mode saves `Imgs/Video name.bmp` beside that video. A later capture
replaces the BMP. Captured BMP files take priority over supplied PNG/JPG
posters; deleting the BMP restores the former poster or inherited folder
cover.

## Parent menu

Hold SELECT + START for about three seconds, enter the four-digit PIN, then
choose:

- **Exit Kids Mode** — restore Onion and return to its normal interface.
- **Add play time** — add 5 to 120 minutes to the current session.
- **Turn off timer** — continue without a time limit.
- **Games only / Videos only** — lock the section currently being viewed. The
  vertical navigation arrow disappears while the lock is enabled.
- **Back** — return to the child interface.

## Timer and resume behavior

The same timer is shared by games, video, audio and both carousels. When it reaches
zero, the running content closes and the child sees **Time's up!** and
**See you next time**. If that screen is left untouched for five minutes, the
device powers off cleanly.

The current floor, folder and selection are saved. Media playback is
checkpointed approximately every five seconds. A file that reaches its natural
end has its saved position cleared automatically.

There is no separate auto-resume option: resume behavior always follows the
real state at shutdown.

## PIN reset and recovery

Nothing on the SD card is permanently locked.

- **Forgotten PIN:** edit `App/KidsMode/kidmode.json`, clear `pin_hash`
  and `pin_salt`, and set `pin_plain` to a new four-digit PIN. Also delete
  `Saves/KidsMode/pin_backup.json` so the previous PIN is not restored.
- **Force exit from a computer:** delete the hidden `/.kidmode` file at the
  root of the SD card, then boot normally.
- **Log file:** `.tmp_update/logs/kidsmode.log` records startup and recovery
  information.
- **Interface failure:** after repeated launcher failures, the safety routine
  returns to normal Onion instead of creating a boot loop.

To uninstall, exit through the parent menu first, then remove
`App/KidsMode`. The optional saved state can also be removed from
`Saves/KidsMode` if it is no longer needed.

## Building from source

Every push starts the GitHub Actions workflow. It cross-compiles
`src/kidsMode`, places `kidui` and `libvcinput.so` inside
`App/KidsMode/bin`, and uploads the green artifact named
`Kids-Mode-build`.

Tagged builds additionally create `Kids-Mode.zip` and attach it to the
GitHub Release.

For a local build, copy `src/kidsMode` into an Onion source tree and use
the Miyoo Mini toolchain:

```sh
git clone https://github.com/OnionUI/Onion
cp -r src/kidsMode Onion/src/
docker run --rm -v "$PWD/Onion":/root/workspace \
  aemiii91/miyoomini-toolchain:latest \
  /bin/bash -c "source /root/.bashrc; cd src/kidsMode && make"
```

## Credits

- Original Kids Mode concept and implementation by Reddit user `u/daverad`.
- Kids Mode games-and-videos version maintained by
  [Veuks](https://github.com/Veuks).
- Built on [Onion OS](https://github.com/OnionUI/Onion) and its native UI
  components.
- Video artwork presentation inspired by ScreenScraper Mix V1.
- Developed through an AI-assisted, iterative hardware-testing workflow.

## License

GPL-3.0, matching Onion OS. See [LICENSE](LICENSE).

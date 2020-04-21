## 2.4.0 - 2020-Apr-21

New:
- Pause indicator received a `flash` mode, which is the default now. You can disable it completely with `pause_indicator=none`.

Changed:
- Window title no longer fades in the UI when enabled. I found that 3 sides of the window having proximity triggers was a bit too much, especially on small windows. Felt a bit like a game of Whac-A-Mole. If you want to see the title, hover window controls, volume, or timeline.

Fixed:
- Chapter indicators not updating after initial file load.
- Volume slider nudge rendering issue when `volume-max=100`.
- Sorting too inaccurate since lua's `<>` operators are weird.
- Playlist and directory navigators now respond to and update on file delete commands.

## 2.3.0 - 2020-Apr-20

New:
- Playback speed control widget. Off by default, enable with `speed=yes`. See docs for more options.
- Pause indicator. Off by default, enable with `pause_indicator=yes`.

Changed:
- UI flashing refactored:
	- `timeline_flash_duration=400` changed to `timeline_flash=yes`
	- `volume_flash_duration=400` changed to `volume_flash=yes`
	- Added `speed_flash=yes`
	- Duration now controlled with unified `flash_duration=400` option.
- All menus where it makes sense now update their active item as it changes. For example, if playlist navigation is open, the active item will update if you switch to a different playlist item with a hotkey. Same for directory and chapter navigation.
- Improved menu navigation, which now differentiates between selected and active item.
- File sorting now more closely (but not exactly) matches sorting of current OS.
- You can now use mouse wheel when mouse is hovering over timeline, volume, or speed controls to step by their predefined `{element}_step` option values.

Fixed:
- Chapter ranges `{eof}` was resolving to `0` in some files, causing inaccurate ranges.
- Glitchy menus when closing/reopening too fast.
- Glitchy elements when flashing as mouse is moving near them.

## 2.2.0 - 2020-Apr-15

New:
- Timeline now displays cached ranges for streaming videos. You can turn it off or tweak styles with `timeline_cached_ranges` option.

Changed:
- Volume bar now also flashes on mute changes.

Fixed:
- Escape button not closing open menus.

### 2.1.1 - 2020-Apr-13

Changed:
- Tweaked vertical alignment, scroll indicators, and button handling, for tall menus.

## 2.1.0 - 2020-Apr-13

New:
- Chapter navigation under `uosc/navigate-chapters` command. Bind with:
	```
	key script-binding uosc/navigate-chapters
	```

Changed:
- Use `media-title` instead of `filename` for window title.
- Updated default `chapter_ranges` to be more robust.

Fixed:
- Crash on opening a video from saved state.
- `context-menu` command not correctly toggling menus but always reopening them.
- Keyboard navigation not working properly in some menus.
- Timeline will no longer flash on tiny seeks such as `frame-step`.

# 2.0.0 - 2020-Apr-12

Substantial rewrite with tons of new features and improvements! A lot of options have changed or disappeared, you need to reconfigure your config file.

Changed:
- **Progressbar** and **Seekbar** have been merged into one element **Timeline**.
	Originally I wanted them to be separate because making them the same thing means I have to slide it in instead of fading it in, which I felt would make it a movable target and harder to use. But after actually testing it I've realized that is not the case at all. This change simplifies the code, options, and I'd even say it's even nicer to use than before.

New:
- Volume slider.
- Implemented menu rendering, which in turn allowed me to add a lot of new features on top of it:
	- Context menu - customizable context menu with nesting support which you can fill with whatever you want! Read the documentation to find out how.
	- External subtitles loader.
	- Subtitle/audio/video track selector.
	- Playlist navigation.
	- Directory navigation.
- A lot of new useful commands added. Rad the documentation to see exactly what they do. Here is a quick list of all currently available commands to bind your keys to:
	- `script-binding uosc/flash-timeline`
	- `script-binding uosc/toggle-progress`
	- `script-binding uosc/context-menu`
	- `script-binding uosc/load-subtitles`
	- `script-binding uosc/select-subtitles`
	- `script-binding uosc/select-audio`
	- `script-binding uosc/select-video`
	- `script-binding uosc/navigate-playlist`
	- `script-binding uosc/show-in-directory`
	- `script-binding uosc/navigate-directory`
	- `script-binding uosc/next-file`
	- `script-binding uosc/prev-file`
	- `script-binding uosc/first-file`
	- `script-binding uosc/last-file`
	- `script-binding uosc/delete-file-next`
	- `script-binding uosc/delete-file-quit`
- Option to briefly flash elements for set amount of time when the value they represent changes externally.
- Option to pause video on short clicks, allowing you to use left mouse button for both video dragging and pausing.

Also tons of fixes and tweaks.

## 1.4.0 - 2020-Apr-04

Changed:
- Rewritten `chapter_ranges` feature to support more powerful range definitions.
	The quick example from below can now be written as:
	```conf
	chapter_ranges=sponsor start<968638:0.2>sponsor end
	```
	Another example to display openings and endings of animes:
	```conf
	chapter_ranges=op<968638:0.5>.*,ed|ending<968638:0.5>.*|{eof}
	```
	Read options documentation to learn more about the syntax.

Fixed:
- Some minor bug fixes.

## 1.3.0 - 2020-Apr-03

New:
- Added `chapter_ranges` feature to display chapters that are intended to be ranges as bars instead of dots/lines. Read the docs for more details on how to use them.
	Quick example that displays skippable youtube video sponsor blocks from [](https://github.com/po5/mpv_sponsorblock):
	```conf
	chapter_ranges=Sponsor start-Sponsor end:968638:0.2
	```

## 1.2.0 - 2020-Apr-02

New:
- Added `toggleseekbar` script binding.
- Added `autohide` option to control UI autohide when cursor autohides. Off by default.

## 1.1.0 - 2020-Apr-02

New:
- Proximity UI elements now hide on `cursor-autohide` (mpv's cursor autohide time option).
- Added chapter indicators for both seekbar and progressbar. They can be configured with these new options:
	```conf
	# seekbar chapters indicator style: dots, lines, lines-top, lines-bottom
	# set to empty to disable
	seekbar_chapters=dots
	# seekbar chapters indicator opacity
	seekbar_chapters_opacity=0.3
	# progressbar chapters indicator style: dots, lines, lines-top, lines-bottom
	# set to empty to disable
	progressbar_chapters=dots
	# progressbar chapters indicator opacity
	progressbar_chapters_opacity=0.3
	```

Changed:
- Now renders a 1px bottom border for both bars in no-border window mode so they can be visible when window is over a light background.
- Improved formula for seekbar font size so it's more readable in thinner seekbar sizes.
- Tweaked some default option values for sizes and visibility of both bars.
- `bar_opacity` option got split up into `seekbar_opacity` and `progressbar_opacity`.
- `bar_color_foreground` renamed to `color_foreground`.
- `bar_color_background` renamed to `color_background`.

Fixed:
- Default examples as well as `uosc.conf` file were not working because comments were on the same line as option declarations, which apparently mpv can't parse. So that's fixed now.

### 1.0.5 - 2020-Mar-07

Ensures time text seen above the cursor during seeking doesn't overflow the screen. This is a naive implementation that is only guessing the width of the text, since there is no other API to use for this.

### 1.0.4 - 2020-Mar-04

Tweaked styling of window controls to be more visible against pure black backgrounds.

### 1.0.3 - 2020-Mar-04

Simplified options and made them more explicit.

These options are gone:

```
progressbar=yes/no # toggle progressbar
progressbar_fullscreen=yes/no # toggle progressbar
```

Their functionality moved here:

```
progressbar_size=4             # progressbar size in pixels, 0 to disable
progressbar_size_fullscreen=4  # same as ^ but when in fullscreen
```

And you can also disable seekbar if you want:

```
seekbar_size=40            # seekbar size in pixels, 0 to disable
seekbar_size_fullscreen=40 # same as ^ but when in fullscreen
```

### 1.0.2 - 2020-Mar-03

Fixed long window titles wrapping all over the place instead of being clipped by control buttons.

### 1.0.1 - 2020-Mar-03

**uosc** now won't render when default osc is not disabled (`osc=no`).

# 1.0.0 - 2020-Mar-02

Initial release.

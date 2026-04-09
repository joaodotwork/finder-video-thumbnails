# finder-video-thumbnails

[![npm version](https://img.shields.io/npm/v/@joaodotwork%2Ffinder-video-thumbnails.svg)](https://www.npmjs.com/package/@joaodotwork/finder-video-thumbnails)
[![license](https://img.shields.io/github/license/joaodotwork/finder-video-thumbnails.svg)](LICENSE)
[![platform](https://img.shields.io/badge/platform-macOS-lightgrey.svg)](https://www.apple.com/macos)

Generate macOS Finder thumbnails for video files that don't have one, using `ffmpeg` and [`fileicon`](https://github.com/mklement0/fileicon).

By default, video files in Finder show a generic icon (or a low-res QuickLook preview that disappears when you scroll away). This tool grabs a frame from each video and sets it as a permanent custom Finder icon — so the thumbnails are always visible, in any view, with the natural aspect ratio preserved.

## Requirements

- macOS (uses `fileicon` which sets `com.apple.FinderInfo` xattr — won't work on Linux/Windows)
- [`ffmpeg`](https://ffmpeg.org/) — `brew install ffmpeg`
- [`fileicon`](https://github.com/mklement0/fileicon) — `brew install fileicon`

## Install

```sh
npm install -g @joaodotwork/finder-video-thumbnails
```

Or run without installing:

```sh
npx @joaodotwork/finder-video-thumbnails <folder>
```

## Usage

```sh
finder-video-thumbnails [--force] <folder> [seek_seconds]
```

- `<folder>` — folder to process (recursive)
- `--force` — re-generate icons even if a custom icon is already set
- `seek_seconds` — timestamp (in seconds) to grab the frame from. Defaults to `1`

### Examples

Add thumbnails to every video in `~/Movies` (recursive):

```sh
finder-video-thumbnails ~/Movies
```

Grab the frame at the 5-second mark:

```sh
finder-video-thumbnails ~/Movies 5
```

Re-generate all thumbnails, overwriting existing ones:

```sh
finder-video-thumbnails --force ~/Movies
```

## Supported formats

`.mov`, `.mp4`, `.m4v`, `.avi`, `.mkv`, `.webm` (case-insensitive)

## How it works

For each video without a custom icon:

1. `ffmpeg` extracts a single frame at the seek timestamp.
2. The frame is scaled to fit a 512×512 box, preserving aspect ratio, then padded to a square with a transparent background. This ensures Finder displays the thumbnail with the video's natural aspect ratio rather than stretching it.
3. `fileicon` writes the resulting PNG as the file's custom Finder icon.

Re-runs are idempotent: files that already have a custom icon are skipped unless `--force` is passed.

## License

MIT

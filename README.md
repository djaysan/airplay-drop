<p align="center">
  <img src="icon/icon_1024.png" width="128" alt="AirPlay Drop icon">
</p>

<h1 align="center">AirPlay Drop</h1>

<p align="center">
  Drop a video, pick an AirPlay device, press Play.<br>
  A free, open-source alternative to Airflow for macOS.
</p>

---

AirPlay Drop sends local video files (MKV, MP4, AVI and friends) from your Mac to any AirPlay video receiver: Apple TV style boxes, smart TVs, or Android devices running receiver apps like AirPin or AirScreen. No screen mirroring, no lag: the receiver streams the file directly and plays it with its own hardware.

<p align="center">
  <img src="screenshots/app.png" width="820" alt="AirPlay Drop main window">
</p>

## Download

Grab `AirPlay-Drop-macOS-arm64.zip` from the [latest release](https://github.com/djaysan/airplay-drop/releases/latest), unzip, and drag **AirPlay Drop.app** into Applications. Done - no build, nothing else to install.

First launch: right-click the app and choose **Open** (it is not notarized with a paid Apple certificate, so macOS asks once). If macOS claims the app is damaged, clear the quarantine flag instead:

```sh
xattr -cr "/Applications/AirPlay Drop.app"
```

Apple Silicon (M1 and later) only. On an Intel Mac, install from source below.

## Features

- **Drag and drop** any video onto the window or the Dock icon
- **Device picker** with automatic discovery (Bonjour) and one-click rescan
- **Live subtitle burn-in**: subtitles are rendered into the picture while streaming (HLS), so playback starts in about 2 seconds, even for a full movie
- **Subtitle track picker**: choose between all embedded tracks (or a sidecar `.srt`), remembered per file
- **Resume**: per-file playback positions with a "Resume from 1:23:45?" prompt
- **Recent list** with progress bars showing where you left off
- **Fix audio** option: quick repack for files whose audio codec (DTS etc.) the receiver cannot decode
- **Password support** for protected receivers (AirPlay digest auth), remembered after first entry
- Pause, seek, and stop, straight from the window

## Requirements

- macOS (Apple Silicon or Intel)
- Python 3.10+ ([python.org](https://www.python.org/downloads/) build recommended)
- An AirPlay video receiver on the same network

The interface is a native WebKit window (dark glass, macOS style) driven by a small Python backend. No Electron, no browser tab.

## Install from source

```sh
git clone https://github.com/djaysan/airplay-drop.git
cd airplay-drop
./install.sh
```

The installer fetches two small Python packages plus a self-contained ffmpeg build (used for subtitles and audio fixing; your system ffmpeg, if any, is untouched), then builds `AirPlay Drop.app` in the folder. Drag the app to your Dock and you are done.

## Usage

1. Start the AirPlay receiver (your Apple TV, or the AirPin/AirScreen app on an Android device).
2. Open AirPlay Drop. Your receiver appears in the device dropdown.
3. Drop a video onto the window (or onto the Dock icon).
4. Optional: tick **Subtitles** and pick a track. Tick **Fix audio** if a file plays without sound.
5. **Play.**

Shortest path: drop a video on the window and press Play. Everything else is optional.

You can also use the CLI directly:

```sh
python3 airplay-play.py movie.mkv                # play
python3 airplay-play.py movie.mkv --subs         # burn subtitles first
python3 airplay-play.py --probe                  # check the receiver responds
```

## How it works

AirPlay video is a simple protocol: the sender POSTs a URL to the receiver, and the receiver fetches and plays the stream itself. AirPlay Drop serves your file over HTTP from the Mac and sends that URL. For subtitles it transcodes on the fly with hardware acceleration into an HLS stream with the subs rendered in, the same trick Airflow uses, so there is no waiting for a full conversion.

## Troubleshooting

- **Password box appears**: your receiver has an AirPlay password set (AirPin: Settings, password/verification). Type it once; it is remembered.
- **Video plays but no sound**: the audio track is a codec your receiver cannot decode. Tick **Fix audio** and play again (a one-time repack, cached next to the file).
- **No devices found**: make sure the receiver app is running and both devices are on the same network/VLAN.
- **Volume slider?** There is none: most receivers ignore AirPlay volume commands for video, so use the device or remote volume.
- Any other failure shows an error window with a **Copy error** button; paste that into a GitHub issue.

## Support this project

AirPlay Drop is free. If it saved you the price of a commercial app, you can buy me a coffee:

[<img src="https://storage.ko-fi.com/cdn/kofi2.png?v=6" alt="Buy Me a Coffee at ko-fi.com" height="36">](https://ko-fi.com/H2H81I6YY1)

## License

[MIT](LICENSE)

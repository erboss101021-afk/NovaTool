# NovaTool

NovaTool is a collection of Python and Windows terminal utilities gathered in one project. It includes playlist tools, public-web OSINT helpers, and an authorized Discord server administration and auditing module.

> **Project status:** version 1.0. Some components are experimental and depend on external websites or third-party APIs. External URLs, browser selectors, and API behavior may change over time.

## Components

### Playlist tools

#### `DownloaderPlaylist/SpotifyDownloader.py`

Reads Spotify playlist metadata through the Spotify Web API, searches for matching tracks on YouTube with `yt-dlp`, and converts the audio with FFmpeg.

Features:

- accepts a Spotify playlist URL or playlist ID;
- retrieves track titles, artists, and albums;
- searches for the first matching YouTube result;
- supports `mp3`, `flac`, `wav`, `m4a`, `opus`, and `vorbis`;
- saves files to `DownloaderPlaylist/downloads/` by default;
- supports custom output directories, audio quality, and file numbering.

#### `DownloaderPlaylist/PlaylistGenerator.py`

Opens TuneMyMusic in the default browser and attempts to enter a playlist query automatically. It uses Selenium when available, then tries `pyautogui`, and finally falls back to manual input.

### OSINT tools

The OSINT scripts build search URLs and open them in the default browser. They do not store search results in a local database.

#### `OSINT/DoxTracker.py`

Provides public-web search menus for:

- names;
- phone numbers;
- obituaries and memorials;
- IP addresses.

#### `OSINT/SKipTracer.py`

Provides public-web search menus for:

- email addresses;
- names;
- phone numbers;
- usernames and screen names;
- license plates;
- public references on Doxbin;
- a guided profiler that does not save data locally.

Results from external websites are not verified by NovaTool and may be incomplete, inaccurate, or outdated.

### Shadow-Nuker

#### `Shadow-Nuker/Shadow.py`

Shadow-Nuker is an asynchronous Discord bot administration and security-auditing utility for servers that you own or are explicitly authorized to test.

The module provides a terminal interface for high-impact server-management operations, including:

- server and channel administration;
- role and permission management;
- emoji and soundboard management;
- invite management;
- moderation and community settings;
- thread and message operations;
- server statistics and configuration checks.

The interface uses the `rich` library and is configured to display its styles in red, equivalent to the ANSI color code `\033[31m`.

> **Authorization required:** use Shadow-Nuker only on servers where you have explicit permission to perform the selected actions. Many operations can permanently delete or change server data.

## Requirements

- Python 3.10 or later;
- Windows is recommended for `NovaTool.bat`;
- a default web browser;
- FFmpeg in the `PATH` for the playlist downloader;
- Spotify Developer App credentials for `SpotifyDownloader.py`;
- a Discord bot token and the required permissions for `Shadow-Nuker/Shadow.py`;
- Windows Terminal (`wt`) for the batch launcher.

## Installation

Clone the repository and create a virtual environment:

```bash
git clone https://github.com/<user>/<repository>.git
cd NovaTool

python -m venv .venv
```

Activate the virtual environment.

### Windows PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

### Windows CMD

```bat
.venv\Scripts\activate.bat
```

### Automatic dependency installation

The repository includes `InstallDependencies.bat`, which automates the setup on Windows.

Place the file in the project root and double-click it, or run it from a terminal:

```bat
InstallDependencies.bat
```

The script automatically:

- detects the available Python installation;
- creates the `.venv` virtual environment;
- upgrades `pip`;
- installs the Spotify and playlist downloader dependencies;
- installs the optional browser automation dependencies;
- installs the dependencies listed in `Shadow-Nuker/requirements.txt`;
- checks whether FFmpeg is available in `PATH`.

FFmpeg is not installed by the batch file. If it is missing, the script displays a download link and audio conversion may not work.

Install the playlist downloader dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install spotipy yt-dlp
```

Install the Shadow-Nuker dependencies:

```bash
python -m pip install -r Shadow-Nuker/requirements.txt
```

For optional browser automation:

```bash
python -m pip install selenium webdriver-manager pyautogui pyperclip
```

Install FFmpeg by following the official documentation:

<https://ffmpeg.org/download.html>

Verify that it is available:

```bash
ffmpeg -version
```

## Spotify Configuration

### 1. Create a Spotify app

1. Open <https://developer.spotify.com/dashboard>.
2. Sign in and create a new app.
3. Enable access to the Web API.
4. Copy the `Client ID` and `Client Secret`.

### 2. Configure the credentials

Using environment variables is the recommended method.

#### Windows PowerShell

```powershell
$env:SPOTIFY_CLIENT_ID="your-client-id"
$env:SPOTIFY_CLIENT_SECRET="your-client-secret"
```

#### Windows CMD

```bat
set SPOTIFY_CLIENT_ID=your-client-id
set SPOTIFY_CLIENT_SECRET=your-client-secret
```

Alternatively, the downloader can save the credentials locally:

```bash
python DownloaderPlaylist/SpotifyDownloader.py --configure
```

The credential priority order is:

1. `--client-id` and `--client-secret` command-line arguments;
2. environment variables;
3. `DownloaderPlaylist/config.json`.

## Usage

### Windows main menu

After checking the paths in `NovaTool.bat`, run:

```bat
NovaTool.bat
```

The main menu provides:

1. **OSINT**
2. **Playlist**
3. **Shadow-Nuker**
4. **Credits**

> The included launcher contains example absolute paths such as `C:\Users\User\NovaTool\...`. Replace them with the actual path to your local repository before use.

All five tools are intended to be started through `NovaTool.bat`. If one of the scripts is launched directly, it displays a message asking you to start it through the launcher and then exits.

### Download a Spotify playlist

Open `NovaTool.bat`, select **Playlist**, and then select **SpotifyDownloader**. Enter the Spotify playlist URL or ID when prompted.

The downloader supports the following formats and options internally:

```text
Formats: mp3, flac, wav, m4a, opus, or vorbis
Quality: 128, 192, 320, or another value accepted by yt-dlp/FFmpeg
Output: DownloaderPlaylist/downloads/ by default
```

### Generate a playlist

Open `NovaTool.bat`, select **Playlist**, and then select **PlaylistGenerator**. Enter a description such as `80s rock` or `relax study`. The script will open TuneMyMusic and attempt to fill in the search field automatically.

### Run OSINT, Playlist, and Shadow-Nuker tools

Start the launcher from the project root:

```bat
NovaTool.bat
```

Use the menu to select **OSINT**, **Playlist**, or **Shadow-Nuker**. Follow the interactive menus and only enter information for which you have a legitimate purpose and appropriate authorization.

## Project structure

```text
NovaTool/
├── DownloaderPlaylist/
│   ├── PlaylistGenerator.py
│   ├── SpotifyDownloader.py
│   ├── config.json
│   └── downloads/
├── OSINT/
│   ├── DoxTracker.py
│   └── SKipTracer.py
├── Shadow-Nuker/
│   ├── assets/
│   │   ├── banner.jpg
│   │   └── screenshot.jpg
│   ├── LICENSE
│   ├── README.md
│   ├── requirements.txt
│   └── Shadow.py
├── InstallDependencies.bat
└── NovaTool.bat
```

## Limitations and technical notes

- The Spotify downloader does not download Spotify streams directly. It uses Spotify metadata to find a matching result on YouTube.
- Automatic matching selects the first result and may return an unwanted live version, cover, or remix.
- Audio conversion requires FFmpeg.
- Playlist and OSINT features depend on external websites and may stop working when those websites change.
- Some websites may require authentication, limit requests, or be unavailable in certain countries.
- Shadow-Nuker requires a valid Discord bot token and sufficient permissions for each selected operation.
- Discord rate limits, permission checks, and API changes may affect Shadow-Nuker operations.
- The project does not currently include a single root-level `requirements.txt` file or a unified automated test suite.

## Responsible and legal use

NovaTool must only be used for lawful, authorized activities that respect privacy, platform rules, and applicable laws.

The creator of NovaTool accepts no responsibility for any damage, consequences, violations, or events resulting from the use of this project. Users are solely responsible for how they use the software.

Do not use these tools to:

- collect or distribute personal data without authorization;
- harass, threaten, stalk, or identify people against their will;
- bypass access controls, anonymity measures, or security systems;
- modify or destroy Discord servers without explicit authorization;
- violate the Terms of Service of the websites or platforms being accessed;
- download or redistribute copyrighted material without the necessary rights.

For audio downloads, only use content that you have the right to download, and comply with the applicable laws in your country and the Terms of Service of Spotify, YouTube, and any other services involved.

## License

The `Shadow-Nuker` component includes its own MIT license in `Shadow-Nuker/LICENSE`.

The licensing status of the other NovaTool components is not specified in the original project. Add or update a root-level `LICENSE` file if you intend to distribute the complete project under a specific license.

## Author

The scripts identify the following authors:

- NovaTool scripts: `Er_Boss`;
- Shadow-Nuker: `Nystic Shadow`.

Verify and update this section before publishing the repository.
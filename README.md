# BlueBird 🐦🎵

<p align="center">
  <img src="icons/logo.png" alt="BlueBird Logo" width="128" height="128" />
</p>

<p align="center">
  <strong>A sleek, lightweight desktop music player built in Python and PyQt5.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/PyQt5-GUI-green?style=for-the-badge&logo=qt&logoColor=white" alt="PyQt5" />
  <img src="https://img.shields.io/badge/Deezer-API-00C7F2?style=for-the-badge&logo=deezer&logoColor=white" alt="Deezer API" />
  <img src="https://img.shields.io/badge/Platform-Linux%20%7C%20Windows%20%7C%20macOS-lightgrey?style=for-the-badge" alt="Platform Compatibility" />
</p>

---

## 🌟 Overview

**BlueBird** is a modern and intuitive desktop music application that connects directly to the **Deezer API**. It allows you to search millions of tracks, explore top charts, create customized offline playlists, organize your favorite songs, and download high-quality MP3 track previews to your computer.

The application leverages Python's powerful backend libraries combined with **PyQt5's** rich graphical user widgets to deliver an aesthetic and fluid desktop music streaming experience.

---

## ✨ Features

- 🎵 **Instant Streaming**: Seamlessly streams high-quality 30-second audio previews directly from Deezer.
- 📈 **Live Music Charts**: Automatically fetches and displays the current globally trending tracks upon launching.
- 🔍 **Global Song Search**: Find any track, artist, or album in real-time with comprehensive top matches powered by the Deezer search engine.
- 💾 **Local MP3 Downloader**: Download preview streams to your local storage via a native folder selector dialog.
- 📂 **Dynamic Playlists**: Create, edit, and play personalized local playlists. Playlists support automatic track progression (the player automatically proceeds to the next song).
- ❤️ **Favorites System**: Bookmark your beloved songs in a dedicated "Favorite Songs" category with simple one-click controls.
- 📶 **Smart Offline Guard**: Built-in connection verification checks your internet availability on startup. It alerts you via native dialogs and prevents application crashes if you are offline.

---

## 🎮 Interface & Controls

BlueBird provides a clean dual-panel layout designed for effortless navigation:

| Section                | Description                                                                                                                                     |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Top Menu**           | Switches between the **Trending Charts** ("Mas Escuchadas"), **Favorite Songs** ("Canciones Favoritas"), and houses the dynamic **Search Bar**. |
| **Left Sidebar**       | Manages your custom Playlists. Shows a checklist icon for each playlist, a quick play button, and a "+" button to create new playlists.         |
| **Central Song Panel** | Displays song titles and artist names. Click to play, or right-click to reveal the action context menu.                                         |
| **Bottom Control Bar** | Shows the currently playing track info (Title & Artist), play/pause controller, and an interactive seek slider to scrub through the audio.      |

### 🖱️ Mouse Bindings

- **Left-Click (Double-Click)** on a song: Selects and plays the track immediately.
- **Right-Click** on a song: Opens a context menu with options to:
  - 📥 **Download** (Descargar) the song to a custom directory.
  - ❤️ **Add to Favorites** (Añadir a favoritas).
  - ➕ **Add to Playlist** (Añadir a Lista de reproducción) — choose an existing playlist or create a new one.
  - ❌ **Delete from current view** (e.g., remove from playlists or favorites).
- **Right-Click** on a Playlist name in the Sidebar: Opens a menu to delete the playlist entirely.

---

## 🛠️ Installation & Setup

### 1. Prerequisites

Make sure you have **Python 3.8 or higher** installed.

#### Linux Users (Ubuntu/Debian)

PyQt5's multimedia engine requires system-level GStreamer/Qt plugins. Run the following command to ensure all audio codecs work perfectly:

```bash
sudo apt-get update
sudo apt-get install python3-pyqt5 python3-pyqt5.qtmultimedia gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

### 2. Clone the Repository

```bash
git clone https://github.com/carlossc212/BlueBird.git
cd BlueBird
```

### 3. Install Dependencies

Install Python dependencies using `pip`:

```bash
pip install -r requirements.txt
```

### 4. Run the Application

Start the music player by running:

```bash
python main.py
```

---

## 📁 Repository Structure

```
BlueBird/
├── main.py                  # App entry point, internet guard, and initialization
├── player/
│   ├── __init__.py
│   ├── api.py               # Deezer API client handler (Requests wrapper)
│   └── Player.py            # Core audio engine (QMediaPlayer) & file persistence
├── ui/
│   ├── __init__.py
│   ├── mainwindow.py        # Complete Qt5 user interface, CSS themes, and signal-slot bindings
│   └── NameChooserDialog.py # Custom PyQt dialog for naming new playlists
├── icons/                   # Visual graphic assets and button icons
├── playlists/               # User-created playlist files (Automatically generated)
├── favoritas                # Local favorites list database (Automatically generated)
└── requirements.txt         # Project dependencies catalog
```

---

## 🧠 Technical Deep-Dive

- **Audio Streaming & Caching**: When a song is triggered, the `Player` class fetches the audio preview via a secure network stream, writes it asynchronously into a temporary local file (`tempfile.NamedTemporaryFile`), and delivers a local URL (`QUrl.fromLocalFile`) to PyQt's hardware-accelerated `QMediaPlayer`. This eliminates network latency stuttering during playback.
- **Serialization & Storage**:
  - **Playlists** are organized in plain text files within the `playlists/` folder. Every song is serialized in the format: `url||title||artist`.
  - **Favorites** are logged in the `favoritas` database file in the project's root using a similar format, making the app entirely self-contained with zero database dependencies.
- **Auto-Play Progression**: The player listens to the `mediaStatusChanged` signal. When it intercepts a `QMediaPlayer.EndOfMedia` status, it increments the internal queue counter and loads the subsequent song in the active playlist.

---

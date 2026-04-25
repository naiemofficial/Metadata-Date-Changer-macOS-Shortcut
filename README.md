# Metadata-Date-Changer-macOS-Shortcut
A high-precision, developer-grade macOS Shortcut designed to solve the common "Ghost Metadata" problem. When standard tools fail to update the **Content Created** field in Finder or cause 6-hour timezone shifts, this tool uses a "Nuclear" greedy-overwrite strategy to force absolute consistency across all metadata namespaces.

<p align="center"><img width="1916" height="2168" alt="Metadata-Date-Changer-macOS-Shortcut" src="https://github.com/user-attachments/assets/daa1a347-ca03-480c-85b8-372cb001a8ca" /></p>


## 🛠 The Problem it Solves
macOS pulls the "Content Created" date from a complex hierarchy of tags and the Spotlight database. Simple EXIF editors often miss proprietary tags (Apple, Sony, GPS) or fails to update the filesystem layer. This leads to files showing different dates in the "Get Info" pane versus the Finder columns.

## 🚀 Key Features

### 1. Three-Layer Synchronization
To ensure the date you set is the date you see, the script operates on three distinct layers:
* **Layer 1 (Metadata):** Uses `ExifTool` with greedy wildcards (`-*Date*`, `-*Time*`) to hit every possible tag group (EXIF, IPTC, XMP, MakerNotes).
* **Layer 2 (Filesystem):** Employs the Unix `touch` command with seconds-precision (`.ss`) to align the hardware-level creation/modification dates.
* **Layer 3 (Spotlight):** Force-updates the macOS internal database (`MDItemContentCreationDate`) to ensure Finder UI consistency.

### 2. Dynamic Operational Modes
* **Option 1 & 2 (Manual Override):** Forcefully replaces every existing date/time tag with a manually provided string.
* **Option 3 (Content-First Auto-Sync):** * **Priority Scan:** First searches for the "Holy Grail" capture tags (`DateTimeOriginal`, `CreationDate`).
    * **Chronological Fallback:** If capture tags are missing, it performs a mathematical "Deep Scan" to find the absolute oldest epoch timestamp, ignoring "midnight" (00:00:00) ghosts and system defaults (1970/1904).

### 3. Developer-Centric Logic
* **Zero-Inference Engine:** Disables `QuickTimeUTC` API logic to prevent the common "+06:00" shift experienced by users in regions like Dhaka.
* **Recursive Power:** Built-in `find` logic allows you to select a folder and process every sub-directory and file in one go.

## 📦 Installation

1.  **Install ExifTool:**
    ```bash
    brew install exiftool
    ```
2.  **Import Shortcut:** Download the `.shortcut` file and add it to your library.
3.  **Permissions:** Ensure the Shortcut has permission to run Shell Scripts and access your files.

## 📖 Usage

1.  Select files or folders in **Finder**.
2.  Right-click → **Quick Actions** → **Metadata Date Changer**.
3.  Choose your mode:
    * **Calendar:** Pick a date from the macOS UI.
    * **Text:** Type a custom `YYYY:MM:DD HH:MM:SS` string.
    * **Auto:** Let the "Deep Scan" engine find the oldest anchor date.

---

## 🏗 Technical Details (HTML Snippet)

<table width="100%">
  <tr>
    <td bgcolor="#f8f9fa" style="padding:15px;">
      <b>Greedy Wildcard Strategy:</b><br>
      The script uses <code>-*Date*=$val</code> and <code>-*Time*=$val</code>. This effectively "greps" every tag name internally. If a tag is named <i>SonyDateTime</i>, <i>HistoryWhen</i>, or <i>AppleCreationDate</i>, it is identified and overwritten.
    </td>
  </tr>
  <tr>
    <td bgcolor="#ffffff" style="padding:15px;">
      <b>Timezone Preservation:</b><br>
      By bypassing the UTC shift API, the script writes the raw string digits into the tags. This ensures that the time you pick (e.g., 06:29 AM) is exactly what stays in the file, preventing the OS from subtracting offsets.
    </td>
  </tr>
</table>

<br>

> **Note for Users:** If the "Content Created" column in Finder doesn't update immediately, select the file and press `Cmd + I`. This forces macOS to re-scan the file and refresh its UI cache.

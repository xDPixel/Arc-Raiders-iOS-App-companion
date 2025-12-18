# Arc Raiders Companion 📦 — The Geek Edition
**The ultimate sidekick for Arc Raiders — now with extra nerd sauce.**

[![Releases](https://img.shields.io/github/v/release/xDPixel/Arc-Raiders-iOS-App-companion)](../../releases) [![License](https://img.shields.io/github/license/xDPixel/Arc-Raiders-iOS-App-companion)](LICENSE) [![Platform](https://img.shields.io/badge/platform-iOS-blue.svg)]()

![Arc Raiders Companion Banner](02%20%20/Assets.xcassets/AppLogo.imageset/AppLogo.png)

---

## 🔭 TL;DR
- Offline-first iOS companion app for Arc Raiders: items, ARC (machines) data, maps, and hideout progression tracking. **Built for people who prefer reproducible builds and deterministic assets.**
- Use Releases to grab a signed `.ipa`, or build locally with Xcode 14+.

---

## 🚀 Features (For Geeks)
- **Rich Item DB**: canonical item stats, rarity tiers, and canonical IDs.
- **Hideout Tracker**: deterministic material requirements for station upgrades (Gunsmith, etc.).
- **ARC Intel**: spawn tables and loot drops with references to original MetaForge IDs.
- **Map Layering**: vector-friendly map tiles plus annotation JSON for spawn points.
- **Offline-first**: embedded dataset with optional GitHub-backed auto-update checks.
- **Dev Hooks**: debug builds enable verbose logging, JSON dump endpoints, and in-app data reload.

---

## 🧭 Getting Started (Developer)
Prereqs:
- macOS with Xcode 14+ (recommended)
- Swift 5.8+
- Optional: CocoaPods or Swift Package Manager (if extras are added)

Clone and open:

```bash
git clone https://github.com/xDPixel/Arc-Raiders-iOS-App-companion.git
cd Arc-Raiders-iOS-App-companion
open ArcRaiders.xcodeproj   # or .xcworkspace if CocoaPods is used
```

Build & run (simulator or device):

```bash
# Xcode UI: select target and run
# or from CLI
xcodebuild -scheme "ArcRaiders" -configuration Debug -sdk iphoneos
```

Create .ipa (for sideloading):

```bash
xcodebuild -exportArchive -archivePath path/to/archive.xcarchive -exportOptionsPlist ExportOptions.plist -exportPath ./build
```

Install: AltStore, SideStore, or your preferred signing tool.

---

## 🗂️ Project Layout (Quick)
- `App/` — App entry + scenes
- `Models/` — Codable models (Item, ARC, MapAnnotation)
- `Store/` — Persistence layer (Core Data / local JSON / files)
- `Data/` — Embedded static dataset (JSON) used for offline mode
- `Assets.xcassets/` — App logos & image assets
- `Scripts/` — helper scripts (data import, conversion, validation)

---

## 🔬 Data Format & Tips
Canonical JSON shapes are used so that data can be diffed and audited.
Example: Item JSON (simplified):

```json
{
  "id": "item.weapon.laser_rifle",
  "name": "Laser Rifle",
  "rarity": "epic",
  "stats": { "dmg": 42, "rpm": 90 },
  "sources": ["meta_forge:items/laser_rifle.json"]
}
```

Data update flow:
1. Pull changes from MetaForge / community exports.
2. Run `scripts/import_to_json` to normalize fields.
3. Run validator: `scripts/validate_data` (checks schema + ID collisions).
4. Commit generated `Data/` JSON files and update release.

Geek tip: Use `jq` to quickly inspect and diff datasets:

```bash
jq '.items[] | select(.rarity=="epic") | {id, name}' Data/items.json
```

---

## 🧪 Testing & Debugging
- Unit tests: `Cmd+U` in Xcode (or `xcodebuild test` via CLI)
- Enable debug logging in Settings to get verbose network/data logs
- `in-app` dev menu (Debug builds) allows reloading `Data/` JSON and viewing raw models
- Use the `--record-fixtures` test helper to snapshot generated UI/state for deterministic tests

---

## 🧩 Contributing (Geek Rules)
- Fork, branch with `feat/<short-desc>` or `fix/<short-desc>`
- Tests required for new features or data migrations
- Use conventional commits (recommended): `feat:`, `fix:`, `chore:`
- Run `scripts/format` (SwiftFormat / swiftlint) before opening PR

PR checklist:
- [ ] Tests added/updated
- [ ] Data validated with `scripts/validate_data`
- [ ] No missing assets

---

## 🛠️ Troubleshooting
- Build fails with code signing: ensure your signing identity & provisioning profiles are set correctly.
- Missing assets at runtime: check that `Assets.xcassets` are included in the Copy Bundle Resources build phase.
- Data not updating: make sure `AutoUpdate` URLs are reachable and JSON schemas match the validator.

---

## 📡 Releases & Auto-Update
Releases are on GitHub Releases — grab latest `.ipa` from there. The app can optionally check GitHub release metadata to notify users of new data or app versions.

---

## ❤️ Credits & Data Sources
- Data: **MetaForge**, community contributors
- Maps, testing & hunting: **Arc Raiders Community**

---

## 📜 License
Open-source. See `LICENSE` for full details.

---

> **Geek Footer:** Build reproducible data, pin versions, and always include a failing test that demonstrates the bug. Happy hacking ⚙️

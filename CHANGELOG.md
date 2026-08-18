# Changelog

All notable changes to Wifi-JailUP are documented here.

## [1.0.0] - 2026-08-18

First documented release of this repository.

### Added

- Project `README.md` that describes the actual `netsh` workflow, commands, and limits.
- `CONTRIBUTING.md` for issues and pull requests.
- Official [GNU General Public License v3.0](LICENSE) at the repository root so GitHub can detect the license.
- `.gitignore` for files generated at runtime.
- `.gitattributes` so `*.cmd` stays CRLF.

### Fixed

- `scan` no longer hits a broken `current_ssid` `if` that could abort SSID listing.
- Interface detection matches any `Name` line from `netsh`, not only adapters named `Wi-Fi`.
- Adapter description now keeps the full `netsh` text instead of the first two words.
- Wordlist paths with spaces are quoted; empty lines are skipped; whole lines are read.
- Each guess starts from a fresh `importwifi_attempt.xml` instead of appending to a leftover file.
- Menu commands are case-insensitive.
- Script directory is quoted (`cd /D "%~dp0"`).

### Changed

- Runnable files live at the repository root next to `importwifi.xml` and `wordlist.txt`.
- Console title and headers identify the project as Wifi-JailUP v1.0.0.
- Obsolete profile-README, mixed MIT/GPL `License.txt`, and leftover TheBATeam notes were removed.

[1.0.0]: https://github.com/sanguirIS/Wifi-JailUP/releases/tag/v1.0.0

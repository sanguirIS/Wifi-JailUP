# Wifi-JailUP

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-lightgrey.svg)](#requirements)
[![Release](https://img.shields.io/github/v/release/sanguirIS/Wifi-JailUP?include_prereleases)](https://github.com/sanguirIS/Wifi-JailUP/releases)

Windows batch tool that tests whether a nearby **WPA2-PSK (AES)** network accepts a passphrase from a wordlist. It uses only built-in `netsh wlan` commands: add a temporary profile, try to connect, and read the interface state.

Wifi-JailUP is a maintained snapshot of [TechnicalUserX's Batch Wi-Fi Brute Forcer](https://github.com/TechnicalUserX/batch_wifi_brute_forcer), with a real project README, a proper GNU GPL-3.0 license, and fixes so the documented commands actually run.

**Use it only on networks you own or have explicit permission to test.** Unauthorized access to computer networks is a crime in most jurisdictions.

<p align="center">
  <img src="Preview/Main%20Menu.PNG" alt="Wifi-JailUP main menu" width="80%" />
</p>

## What it does

1. Detects wireless interfaces through `netsh wlan show interfaces`.
2. Lists nearby SSIDs through `netsh wlan show networks`.
3. For each line in a wordlist, writes a WLAN profile XML and runs `netsh wlan connect`.
4. Polls the interface state (`associating`, `authenticating`, `connecting`, `connected`).
5. If the interface reaches `connecting` or `connected`, writes `result.txt` and stops.

It is an *online* association test. Windows must complete 802.11 association and 4-way handshake for every guess, so it is slow by design. It is not a capture/handshake cracker, and it does not implement WPS, PMKID, or WPA3 attacks.

## Requirements

| Item | Detail |
| --- | --- |
| OS | Windows 10 (1511+) or Windows 11, **English** UI |
| Shell | `cmd.exe` (not PowerShell as the host) |
| Privileges | A user who can add/delete WLAN profiles |
| Adapter | A working Wi-Fi interface visible to `netsh wlan` |
| Console | ANSI escape sequences (Windows 10 1511+) |
| Files next to the script | `importwifi.xml`, optional `wordlist.txt` |

Windows 7 is not supported. Non-English Windows is not supported because the script parses English `netsh` text.

## Quick start

1. Download the [latest release](https://github.com/sanguirIS/Wifi-JailUP/releases) or clone this repository.
2. Keep `bruteforcer.cmd`, `importwifi.xml`, and `wordlist.txt` in the same folder.
3. Double-click `bruteforcer.cmd`, or run it from Command Prompt:

```bat
cd path\to\Wifi-JailUP
bruteforcer.cmd
```

4. If more than one adapter is found, pick one. A single adapter is selected automatically.
5. Type `scan`, then the number of the network you are allowed to test.
6. Type `wordlist` if you want a file other than `wordlist.txt`.
7. Type `attack` and read the warning before continuing.

<p align="center">
  <img src="Preview/Interface%20Detection.PNG" alt="Interface detection" width="48%" />
  <img src="Preview/Network%20Selection.PNG" alt="Network selection" width="48%" />
</p>

## Commands

| Command | Action |
| --- | --- |
| `help` | Show the in-app command list |
| `interface` | Re-run adapter detection and selection |
| `scan` | Disconnect the adapter, list SSIDs, pick a target |
| `wordlist` | Set the wordlist path (absolute or relative) |
| `counter` | Set how many times each guess is polled (default 10) |
| `attack` | Try each wordlist line against the selected SSID |
| `exit` | Quit |

Commands are case-insensitive.

### Typical session

```
bruteforcer$ scan
bruteforcer$ 2
bruteforcer$ wordlist
bruteforcer$ wordlist.txt
bruteforcer$ attack
```

### Counter

Each password is tried, then the script polls interface state in a loop. The default is **10** polls. If `associating` or `authenticating` is seen, the remaining count is increased by 5 so a slow handshake can finish.

### Result file

A successful guess appends a block to `result.txt` in the script directory:

```
Batch Wi-Fi Brute Forcer Result
Target     : ExampleSSID
At attempt : 12
Password   : the-matching-line
```

## Bundled wordlist

`wordlist.txt` is a **short demonstration list** of common 8+ character patterns. It exists so the tool can be run without extra files. It is not a real dictionary and will not recover a strong passphrase.

## Limitations

- Profile template is **WPA2-PSK + AES only** (`importwifi.xml`). WPA3, WEP, WPA-TKIP, enterprise (802.1X), and open networks are not handled.
- Hidden SSIDs show as "No Name" and cannot be selected as a target.
- `netsh` does not handle Unicode SSIDs or passphrases. ASCII only.
- Adapter names and state strings are parsed from English `netsh` output.
- Scanning disconnects the selected adapter from its current network.
- `attack` deletes any existing WLAN profile with the same SSID on that adapter. Save a known password first.
- Low signal produces false negatives: the correct password can fail to associate in time.
- Passwords with XML-significant characters (`&`, `<`, `>`, `"`) can produce an invalid profile.
- This is intentionally slow. Expect seconds per guess, not thousands per second.

## Project layout

```
Wifi-JailUP/
├── LICENSE              GNU General Public License v3.0
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── bruteforcer.cmd      Interactive tester
├── importwifi.xml       WPA2-PSK profile template
├── wordlist.txt         Demo wordlist
└── Preview/             Console screenshots
```

Generated while running (ignored by git): `importwifi_prepared.xml`, `importwifi_attempt.xml`, `result.txt`.

## Credits

Wifi-JailUP is based on **Batch Wi-Fi Brute Forcer** by [TechnicalUserX](https://github.com/TechnicalUserX/batch_wifi_brute_forcer), originally published under the MIT License.

Further distribution and documentation in this tree follow work shared by [TheBATeam](https://github.com/TheBATeam) / [TheKvc](https://github.com/TheKvc).

Upstream contributors called out by the original project include Ankitamehra93, lioen-dev, akshatbhatter1, and AACINI.

This repository is maintained by [sanguirIS](https://github.com/sanguirIS).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to report issues and open pull requests.

## License

Copyright (C) 2025-2026 sanguirIS

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the [GNU General Public License](LICENSE) for more details.

The original Batch Wi-Fi Brute Forcer by TechnicalUserX remains available under the MIT License. This modified work is released under GPL-3.0, which is compatible with that MIT grant. MIT copyright notices are preserved in `bruteforcer.cmd`.

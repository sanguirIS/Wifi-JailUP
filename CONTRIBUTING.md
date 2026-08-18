# Contributing to Wifi-JailUP

Thanks for helping keep this repository accurate and maintainable.

Wifi-JailUP is an educational Windows batch program. It must stay easy to read, honest about what it can do, and licensed under **GNU GPL-3.0**.

## Before you start

- Use the tool only on networks you own or have written permission to test.
- Do not open issues or pull requests that ask for help attacking someone else's network.
- Do not send exploit payloads, malware, compiled droppers, or capture/handshake crackers.

## Ways to help

- Fix parsing bugs in `bruteforcer.cmd` (interface detection, SSID listing, state polling).
- Correct documentation that does not match the code.
- Improve English-language `netsh` parsing without adding new attack methods.
- Add screenshots that do not leak real SSIDs, MACs, or passphrases.
- Report Windows 10/11 behavior changes that break the script.

## Development setup

1. Fork the repository and clone your fork.
2. Work on a topic branch. Do not commit generated files (`importwifi_prepared.xml`, `importwifi_attempt.xml`, `result.txt`).
3. Edit `bruteforcer.cmd` in a text editor that preserves **CRLF** line endings (see `.gitattributes`).
4. Keep `bruteforcer.cmd`, `importwifi.xml`, and `wordlist.txt` in the same directory when you test.

## Coding notes

- This is `cmd.exe` batch with `setlocal enabledelayedexpansion`. Test every change on a real English Windows 10/11 console.
- Do not replace `netsh` with third-party binaries.
- Do not add WPS, PMKID, WPA3, or packet-injection features.
- Quote paths. Assume adapter names and file paths can contain spaces.
- If you change user-visible commands, update `README.md` and the in-app `help` text in the same pull request.
- Keep the GNU GPL-3.0 header at the top of `bruteforcer.cmd`.
- Preserve credit to TechnicalUserX and other upstream authors.

## Pull requests

1. Describe the problem and the exact Windows version you tested.
2. Include `netsh wlan show interfaces` / `netsh wlan show networks` snippets when the change is about parsing, with personal data removed.
3. One concern per pull request when you can.
4. Do not reformat the entire script unless the PR is specifically about formatting.

## Issues

Include:

- Wifi-JailUP version or commit
- Windows version and display language
- The command you typed
- What you expected, and what happened
- Redacted console output

## License of contributions

By opening a pull request you agree that your contribution is licensed under the [GNU General Public License v3.0](LICENSE), the same license as this project.

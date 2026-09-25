# Security Policy

**Wifi-JailUP** is an educational proof-of-concept that demonstrates whether a
nearby **WPA2-PSK (AES)** network accepts a passphrase from a wordlist, using a
Windows batch script (`bruteforcer.cmd`, in this tree at
`Source Code/bruteforcer.cmd`) that uses only the built-in `netsh wlan` commands.

Because this is a dual-use offensive-security tool, this policy covers two things:
how to report a **real vulnerability in this code**, and what is deliberately
**out of scope** so the issue tracker stays useful.

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| `v1.0.0` (latest release) | :white_check_mark: |
| `main` branch tip | :white_check_mark: (rolling, best-effort) |
| Older commits, untagged snapshots, forks | :x: |

There are no LTS branches. Fixes land on `main` and are shipped in the next
release; please retest against the latest release or `main` before reporting.

## Reporting a Vulnerability

**Please do not open a public issue for a security problem.**

Use **GitHub Private Vulnerability Reporting**, which is enabled for this
repository:

- Security tab → **Report a vulnerability**, or
- <https://github.com/sanguirIS/Wifi-JailUP/security/advisories/new>

The report stays private until a fix is published. If the form is unavailable to
you, open a regular issue that contains **no technical details** and simply ask
for a private channel — we will follow up there.

Do **not** send vulnerability details to the community WhatsApp/Discord links in
`WHO-WE-ARE.txt`, or in a public GitHub issue: those channels are public and
moderated by volunteers.

### What to include

- Affected file and line (for example `Source Code/bruteforcer.cmd::attack`).
- The release tag or commit SHA you tested.
- Your Windows version and the adapter name as shown by `netsh wlan show interfaces`.
- Exact reproduction steps, including any crafted **SSID** or **wordlist entry**.
- Impact: what an attacker gains, and what the user is expected to notice.
- Optionally: a suggested fix, and whether you want public credit.

### What to expect

This is a volunteer, educational project, so response times are best-effort:

- **Acknowledgement:** within 7 days.
- **Triage** (severity + in/out of scope decision): within 14 days.
- **Fix or decision:** depends on severity; we will tell you the plan and keep you updated.
- **Disclosure:** coordinated — 90 days after the report, or as soon as a fix is released, whichever comes first.

We will credit you in the release notes unless you ask to stay anonymous.

## Scope

### In scope

Bugs that make the tool do something a user would not expect, for example:

- **Unintended command execution / command injection** caused by attacker- or
  file-controlled text (an SSID from a scanned access point, a wordlist entry, or
  a file name) being expanded into a batch command line.
- **Unexpected credential exposure on disk or in the environment** beyond the
  behaviour documented below — for example temp files or WLAN profiles that
  retain a passphrase after the tool says it cleaned up.
- **Destructive file operations** outside the documented set
  (`importwifi_prepared.xml`, `importwifi_attempt.xml`, `result.txt`, and the WLAN
  profile of the selected SSID).
- **Privilege issues:** performing a sensitive action silently, or using broader
  rights than the documented use requires.
- **Supply chain:** unexpected network calls, bundled binaries, or changes to
  `importwifi.xml` / `wordlist.txt` that cause code to run.

### Out of scope

These are intentional or already documented and will be closed as "not a vulnerability":

- "This tool can crack/attack Wi-Fi networks." That is its documented, intentional purpose.
- **Antivirus, SmartScreen or VirusTotal detections.** The project intentionally ships
  uncompiled source to avoid false alerts (see `Bin/Not-Compiled.txt`).
- Weak or guessable entries in the demo `Source Code/wordlist.txt`. It is a
  demonstration list, not a credential store or a claim about password strength.
- Reliability limits: low signal strength, slow online attempts, not finding a
  correct password, NIC/locale/`netsh` output differences, or the English-only parsing
  of `netsh` output (Windows 10/11, English UI).
- **Wordlist parsing quirks:** only the first space/tab-delimited token of a line is
  used, so entries containing spaces are truncated; empty lines are skipped.
- Deletion of a pre-existing saved profile for the target SSID. The script prints an
  explicit warning before it calls `netsh wlan delete profile`.
- Missing code signing, installer, auto-update, or a security policy on forks.
- Anything that already requires administrator/SYSTEM rights, physical access, or the
  user's own unlocked Windows account.

## Security-Relevant Behaviour (by design)

Read this before running the tool. These are documented risks, not bugs:

1. **Plaintext credentials on disk.** Every guess is written to
   `importwifi_attempt.xml` (`<protected>false</protected>`, `<keyMaterial>`), and a
   successful result is appended in cleartext to `result.txt`. Both are removed with
   `del`, not a secure wipe, so they can survive in free space, backups, or snapshots.
2. **Passwords are echoed to the console** on each attempt, so they can end up in
   scrollback, screen shares, remote-session logs, or screen recordings.
3. **Real Windows WLAN profiles are created** (`netsh wlan add profile`) and a real
   connection is attempted for every candidate. If the script is interrupted, its
   cleanup may not run, leaving a leftover profile plus a plaintext XML behind.
4. **A saved profile with the same SSID is deleted before the run**
   (`netsh wlan delete profile`), including a legitimate one belonging to you.
5. **No sandbox or dry-run mode:** it uses your real Wi-Fi adapter, the WLAN service,
   and your user rights.
6. **No telemetry and no third-party binaries.** The only external component is
   Windows `netsh`, and the project performs no network activity of its own — you can
   verify this by reading the ~760-line batch file.

### Hardening for testers

- Use a throwaway VM or spare Windows install with a dedicated adapter, and only test
  an access point you own or are authorised to test.
- After a run, check `netsh wlan show profiles` and remove leftovers with
  `netsh wlan delete profile name="<SSID>"`; delete `importwifi_prepared.xml`,
  `importwifi_attempt.xml` and `result.txt`; revert the VM snapshot afterwards.
- Keep the tool out of cloud-synced folders so credential files are not uploaded.
- Prefer a lab/air-gapped environment and never run it against third-party networks.

## Acceptable Use

Run this tool only against networks and devices you own or have explicit written
permission to test. Unauthorised interception or access is illegal in most
jurisdictions, and nothing in this repository or policy grants such permission.

The software is distributed under GPL-3.0 (see `License.txt`) **without warranty**;
its authors are not liable for damage or misuse. Reports of misuse will not be
treated as security vulnerabilities of this project.

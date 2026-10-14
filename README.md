# chan-sccp

This repository contains the working chan-sccp driver for Cisco SCCP phones on Asterisk.

**Working driver:** [timspb/chan-sccp](https://github.com/timspb/chan-sccp)

It is a compact fork-focused README with the basics only: build, install, and use.

English | [Русский](README.ru.md)

Known-good target range for this fork:

- FreePBX 16 or 17
- Asterisk 21 / 22 / 23

## What this project is for

- Cisco SCCP/Skinny phone support in Asterisk
- Device registration and call handling
- Provisioning support for SCCP-based phone setups

## Requirements

- A supported Asterisk build with headers and development files
- Build tools such as `gcc`, `make`, `autoconf`, and `libtool`
- XML and XSLT development libraries
- OpenSSL development libraries
- `gettext`

If your system uses a package manager, install the usual development packages first. Exact package names vary by Linux distribution.

Example for Debian / Ubuntu:

```bash
sudo apt update && sudo apt install -y build-essential git autoconf libtool pkg-config gettext \
  libxml2-dev libxslt1-dev libssl-dev libjansson-dev libsqlite3-dev
```

## Build from source

```bash
git clone https://github.com/timspb/chan-sccp.git
cd chan-sccp
./tools/bootstrap.sh
./configure
make -j2
make install
make reload
```

If you are updating an existing checkout:

```bash
git pull
./tools/bootstrap.sh
./configure
make -j2
make install
make reload
```

## Notes

- This fork is intended for active development and practical use.
- Keep the README short and focused so users can get to build and install steps quickly.
- The original upstream project is still the canonical chan-sccp source line.

## Troubleshooting

- If `./configure` fails, make sure the development packages are installed.
- If the build cannot find Asterisk headers, install the matching Asterisk dev package for your version.
- If the driver does not load, check Asterisk module loading and log output first.

## Improved stability on unreliable network connections

This fork includes experimental improvements to the reliability of
Cisco SCCP phones operating over unstable network connections,
including VPN links with packet loss, temporary outages and
interrupted TCP sessions.

Key improvements:

- Automatic recovery from stale SCCP sessions.
- Improved phone registration after temporary network interruptions.
- Safer handling of concurrent registration attempts.
- Improved device and session lifecycle synchronization.
- Better cleanup of disconnected TCP sessions.
- Reduced risk of phones remaining in a registration failure state.

### Validation

Tested with Cisco 7912 phones on FreePBX 17 / Asterisk 22.11.0.

During testing:
- Asterisk successfully registered 17 SCCP devices.
- A Cisco 7912 automatically re-registered after TCP disconnection.
- Registration recovered after a simulated 90-second network outage.
- No manual phone reboot was required.

These changes are experimental. Long-term stability and all possible
network failure scenarios have not yet been fully validated.

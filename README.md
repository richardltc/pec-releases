# Participate Engage Contribute (PEC): releases

Downloads of **pecd**, the PEC node.

> **Early test software.** PEC is in early development and there is no live
> network yet. The first network will be a **testnet**: its coins have no
> value. Expect frequent updates and occasional resets.

PEC ("Participate Engage Contribute", working name) is a new cryptocurrency
written in [Zig](https://ziglang.org), derived from Peercoin.

## Download

The latest version for your system (these links always point to the newest
release):

| System | Download |
|---|---|
| Linux (Intel/AMD, 64-bit) | [pecd-x86_64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-linux) |
| Linux (ARM 64-bit, e.g. Raspberry Pi 4/5) | [pecd-aarch64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-aarch64-linux) |
| Windows (64-bit) | [pecd-x86_64-windows.exe](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-windows.exe) |

macOS is not available yet. Older versions are on the [Releases](../../releases) page.

Each download is the program itself; there is nothing to unpack. You can
rename it to `pecd` (`pecd.exe` on Windows) if you like.

## Running pecd

1. Put the downloaded file in a folder of its own.
2. On Linux, make it runnable: `chmod +x pecd-x86_64-linux` (or the name
   you gave it).
3. Run it once. It creates `pecd.toml` (its settings file) in the same
   folder, with an explanation of every setting, and a `data` folder for the
   list of other nodes it finds.
4. pecd joins the testnet straight away, but it needs at least one other
   node to start from. Add the seed addresses shared by the team to `seeds`
   under `[peers]` in `pecd.toml` (for example
   `seeds = ["node.example.org", "203.0.113.7"]`), then run it again. After
   that it remembers the nodes it has found.
5. Press **Ctrl+C** to stop it. It saves what it has learned before it exits.

pecd keeps itself up to date: it checks for a new version when it starts and
every 6 hours, installs it and restarts. The previous version is kept next to
it with `.old` added to its name. To turn this off, set `auto_update = false`
under `[update]` in `pecd.toml`.

The log shows what the node is doing in plain English, with the time (UTC)
and a coloured tag on each line: **INFO** for normal events, **WARN** for a
problem with another node, **ERROR** for something pecd itself cannot do.

### Accepting connections from other nodes

pecd accepts connections from other nodes on the testnet port, **48334**,
unless you set `listen = false`. Other nodes can only reach you if you
forward that port (TCP) on your router to the computer running pecd. If your
internet provider uses carrier-grade NAT (the address your router shows
differs from what a "what is my IP" website shows), incoming connections
will not work, but your node can still connect out to others.

| Network | Node-to-node port |
|---|---|
| Testnet | 48334 |
| Mainnet (not live yet) | 48333 |

## Checking your download

Each release lists SHA-256 checksums. To check a file:

- Linux: `sha256sum <file>`
- macOS: `shasum -a 256 <file>`
- Windows (PowerShell): `Get-FileHash <file>`

## Licence

MIT; see [LICENSE](LICENSE).

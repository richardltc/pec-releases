# Participate Engage Contribute (PEC): releases

Downloads of **pecd**, the PEC node.

> **Early test software.** PEC is in early development and there is no live
> network yet. The first network will be a **testnet**: its coins have no
> value. Expect frequent updates and occasional resets.

PEC ("Participate Engage Contribute", working name) is a new cryptocurrency
written in [Zig](https://ziglang.org), derived from Peercoin.

## Download

There are two programs. **pecd** is the node itself. **pec-cli** lets you ask
a running pecd what it is doing, and stop it. Download both for your system
(these links always point to the newest release):

| System | Node | Commands |
|---|---|---|
| Linux (Intel/AMD, 64-bit) | [pecd-x86_64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-linux) | [pec-cli-x86_64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-x86_64-linux) |
| Linux (ARM 64-bit, e.g. Raspberry Pi 4/5) | [pecd-aarch64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-aarch64-linux) | [pec-cli-aarch64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-aarch64-linux) |
| Windows (64-bit) | [pecd-x86_64-windows.exe](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-windows.exe) | [pec-cli-x86_64-windows.exe](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-x86_64-windows.exe) |

macOS is not available yet. Older versions are on the [Releases](../../releases) page.

Each download is the program itself; there is nothing to unpack. You can
rename them to `pecd` and `pec-cli` (`pecd.exe` and `pec-cli.exe` on
Windows) if you like; the examples below use those names.

## Running pecd

1. Put both files in the same folder, of their own.
2. On Linux, make them runnable: `chmod +x pecd pec-cli`.
3. Run `pecd` once. It creates `pecd.toml` (its settings file) in the same
   folder, with an explanation of every setting, and a `data` folder for the
   list of other nodes it finds.
4. pecd joins the testnet straight away, but it needs at least one other
   node to start from. Add the seed addresses shared by the team to `seeds`
   under `[peers]` in `pecd.toml` (for example
   `seeds = ["node.example.org", "203.0.113.7"]`), then run it again. After
   that it remembers the nodes it has found.
5. To stop it, press **Ctrl+C** in its window, or run `pec-cli stop`. It
   saves what it has learned before it exits.

## Checking on a running node

While pecd is running, open another window in the same folder and use
pec-cli:

| Command | Shows |
|---|---|
| `pec-cli getinfo` | Version, network, number of connections, uptime |
| `pec-cli getpeerinfo` | The nodes you are connected to and the software they run |
| `pec-cli getnodeaddresses 5` | Some of the other nodes your node knows about |
| `pec-cli stop` | Stops pecd cleanly |
| `pec-cli help` | All commands |

Only programs on the same computer can send these commands. pecd writes a
fresh password to `data/testnet/.cookie` each time it starts, and pec-cli
reads it from there, so nothing needs setting up.

pecd keeps itself (and pec-cli) up to date: it checks for a new version
when it starts and every 6 hours, installs it and restarts. The previous
versions are kept next to them with `.old` added to their names. To turn this off, set `auto_update = false`
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

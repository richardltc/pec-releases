# Participate Engage Contribute (PEC): releases

Downloads of **pecd**, the PEC node.

> **Early test software.** PEC is in early development and there is no live
> network yet. The first network will be a **testnet**: its coins have no
> value. Expect frequent updates and occasional resets.

PEC ("Participate Engage Contribute", working name) is a new cryptocurrency
written in [Zig](https://ziglang.org), derived from Peercoin.

## Download

Get the latest version from the [Releases](../../releases) page. Choose the
file for your system (Linux, Windows or macOS) and unpack it.

## Running pecd

1. Put `pecd` in a folder of its own.
2. Run it once. It creates `pecd.toml` (its settings file) in the same
   folder, with an explanation of every setting, and a `data` folder for the
   list of other nodes it finds.
3. Fill in `pecd.toml` with the network details shared by the team, then run
   `pecd` again.
4. Press **Ctrl+C** to stop it. It saves what it has learned before it exits.

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

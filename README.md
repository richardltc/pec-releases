# Participate Engage Contribute (PEC): getting started

Downloads of **pecd**, the PEC node, and a guide to running it, mining and sending coins.

> **Test network.** PEC is in early development. The network running now is
> the **testnet**: its coins have **no value**. Expect frequent updates and
> occasional resets, when the chain starts again from nothing.

PEC ("Participate Engage Contribute", working name) is a new cryptocurrency
written in [Zig](https://ziglang.org), derived from Peercoin.

## Download

There are two programs. **pecd** is the node: it keeps a copy of the chain,
talks to other nodes, holds your wallet and can mine. **pec-cli** sends
commands to a running pecd. Download both for your system (these links
always point to the newest release):

| System | Node | Commands |
|---|---|---|
| Linux (Intel/AMD, 64-bit) | [pecd-x86_64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-linux) | [pec-cli-x86_64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-x86_64-linux) |
| Linux (ARM 64-bit, e.g. Raspberry Pi 4/5) | [pecd-aarch64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-aarch64-linux) | [pec-cli-aarch64-linux](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-aarch64-linux) |
| Windows (64-bit) | [pecd-x86_64-windows.exe](https://github.com/richardltc/pec-releases/releases/latest/download/pecd-x86_64-windows.exe) | [pec-cli-x86_64-windows.exe](https://github.com/richardltc/pec-releases/releases/latest/download/pec-cli-x86_64-windows.exe) |

macOS is not available yet. Older versions are on the [Releases](../../releases) page.

Each download is the program itself; there is nothing to unpack. Renaming
them to `pecd` and `pec-cli` (`pecd.exe` and `pec-cli.exe` on Windows) makes
typing easier, and the examples below use those names; updates work with
either name. On Windows, type commands in Command Prompt
or PowerShell, opened in the folder with the programs (in PowerShell, start
them with `.\`, for example `.\pec-cli status`).

## 1. Start your node

1. Put **both** programs in a folder of their own. pecd keeps its settings
   and data next to itself, and pec-cli must be in the same folder.
2. On Linux, make them runnable: `chmod +x pecd pec-cli`.
3. Run `pecd`. The first time, it creates:
   - `pecd.toml`, its settings, with an explanation of every setting;
   - a `data/testnet` folder, with the chain, your wallet (once you make one) and its log.
4. It connects to the testnet by itself and downloads the chain. The log
   ends with a line like:

   ```
   The chain is up to date at block 1,234 (downloaded 1,234 block(s) from 2 node(s) in 9 seconds).
   ```

5. Leave it running. To stop it, press **Ctrl+C** in its window, or run
   `pec-cli stop`. It saves what it needs before it exits.

Check on it any time from a second window in the same folder:

```
pec-cli status
```

```
PEC testnet node, pecd 0.3.1, running for 2 h 14 min

Chain     block 1,234, up to date
Network   3 connection(s): 2 outgoing, 1 incoming
          other nodes can reach us at 203.0.113.7:48334
Mining    off (start with: pec-cli setmining on)
Wallet    none yet (create one with: pec-cli createwallet)
Updates   installed automatically when pecd starts
```

## 2. Make a wallet

Your wallet lives inside pecd, in `data/testnet/wallet.dat`.

```
pec-cli createwallet
```

- It asks you to choose a **passphrase**, which encrypts the wallet file.
  Without one, anyone who can read your computer's files can take the coins.
- It then shows **24 recovery words**, once. **Write them down**, in order,
  and keep them somewhere safe and offline. They are the only way to get your
  coins back if this computer or its files are lost, and anyone who sees them
  can take your coins.

To receive coins, give people an address:

```
pec-cli getnewaddress
```

Addresses start with `tpec1` on the testnet. A new one each time is best,
but they all belong to the same wallet. You can give it a label, to remember
who you gave it to: `pec-cli getnewaddress "from Kay"`.

For an address you can give out once and reuse, more privately:

```
pec-cli getsilentaddress
```

This is a **Silent Payments** address (`tpecsp1…`). Every payment to it lands
at a brand-new one-off address that only your wallet can recognise, so
nobody looking at the chain can see what you received or link the payments
together. pecd checks each new block for them (an encrypted wallet starts
checking after you unlock it once with `pec-cli walletpassphrase`).

Other wallet commands:

| Command | What it does |
|---|---|
| `pec-cli walletpassphrase` | Unlocks the wallet (asks for the passphrase) for 5 minutes, so it can send. `pec-cli walletpassphrase 600` unlocks it for 10 minutes. |
| `pec-cli walletlock` | Locks it again at once. |
| `pec-cli listaddresses` | Every address the wallet has given out, with its label and the amount at it now. |
| `pec-cli setlabel <address> "label"` | Labels one of your addresses (an empty label `""` removes it). |
| `pec-cli getwalletinfo` | Whether it is encrypted or unlocked, and how many addresses it has given out. |
| `pec-cli restorewallet` | Recreates a wallet from its 24 words (asks for them). |

## 3. Mine

Mining uses your computer's processor to make new blocks. Each block you
find pays you its reward: on the testnet now **3,890 PEC**, falling slowly
over the years.

1. Make a wallet first (step 2): the rewards go to a new address from it.
2. Start mining:

   ```
   pec-cli setmining on
   ```

   This lasts until pecd stops. To mine every time pecd starts, set
   `mine = true` under `[mining]` in `pecd.toml` and restart pecd.
3. Watch it:

   ```
   pec-cli getmininginfo
   ```

   or `pec-cli status`. The log says when you find a block:

   ```
   Found block 238! Reward 3,890 PEC, spendable from block 338.
   ```

4. Stop with `pec-cli setmining off`.

Things to know:

- **Mined coins wait 100 blocks** (about 1 hour 40 minutes) before they can
  be spent. Until then `getbalance` shows them as "immature".
- **Blocks come about once a minute** across the whole network. How many
  you find depends on your share of everyone's mining power.
- **Memory:** with at least 2.5 GB of free memory, pecd mines in *fast mode*
  (about 2.1 GB). Otherwise it uses *light mode* (256 MB), several times
  slower. The log says which.
- **Threads:** pecd chooses how many processor threads to use (one per 2 MB
  of the processor's cache) and runs them at low priority, so the computer
  stays usable. To choose yourself: `pec-cli setmining on 2`, or
  `threads = 2` under `[mining]`.
- **Mining on a server:** you do not need a wallet there. Make the wallet at
  home, get an address with `pec-cli getnewaddress`, and put it on the server
  in `pecd.toml`:

  ```
  [mining]
  mine = true
  mining_address = "tpec1p..."

  [wallet]
  wallet = false
  ```

  The rewards go to your home wallet, and the server holds no keys.
- **Mining with several computers (pec-miner):** run pecd on one computer
  and `pec-miner` on the others; they all mine through that pecd, into its
  wallet (or its `mining_address`). On the computer with pecd, turn the
  mining port on in `pecd.toml` and restart pecd:

  ```
  [mining]
  server = true
  server_password = "choose a long password"
  ```

  On each other computer (pec-miner is in the release downloads):

  ```
  pec-miner --node 192.168.1.20
  ```

  with pecd's local network address; it asks for the password. The mining
  port (48337) only hands out blocks to mine, never wallet commands, but it
  is not encrypted: keep it on your home network or a VPN, not open to the
  internet. On pecd's own computer, `pec-miner` in pecd's folder needs no
  options (it uses the command port and the `.cookie` password).

  If pec-miner says pecd does not answer, the computer with pecd may be
  blocking the port. Windows asks whether to allow pecd the first time the
  mining port is on: allow it for private networks. On Linux with a firewall,
  open it, for example `sudo ufw allow 48337/tcp` or
  `sudo firewall-cmd --add-port=48337/tcp --permanent && sudo firewall-cmd --reload`.

## 4. Send coins

```
pec-cli getbalance
```

shows what you can spend now, what is mined but not yet spendable, what is
leaving in a payment waiting for a block, and what others have sent you that
is not in a block yet ("arriving").

To pay someone (unlock the wallet first if it has a passphrase):

```
pec-cli send tpec1p... 12.5
```

pec-cli shows exactly what will happen and asks before sending:

```
Send 12.5 PEC to tpec1p...
  fee:      0.000155 PEC
  total:    12.500155 PEC
  balance:  354,880 PEC now, 354,867.499845 PEC after sending
Send it? Payments cannot be undone. [y/N]
```

The payment is in the next block, usually within a minute. You don't need to
wait for it: your change can be spent straight away, so you can send another
payment at once. Coins others send you can be spent once they are in a block.

To see what happened:

| Command | Shows |
|---|---|
| `pec-cli listtransactions` | Your last 10 transactions: mined, received, sent, and names bought, changed or given away (`listtransactions 50` for more; `listtransactions 10 10` for the 10 before the newest 10). Payments to your names say which name they came through. |
| `pec-cli gettransaction <txid>` | One transaction: in which block, amounts, addresses and fee |
| `pec-cli listunspent` | Each of your coins separately, and when mined ones become spendable |

## 5. Buy a name

You can buy names such as **@kay**, so people can pay you without long
addresses. Names are 3 to 20 letters, digits and hyphens; capitals don't
matter (`@Kay` is `@kay`).

```
pec-cli registername kay
```

```
Buy @kay for 10,000 PEC
  locked with the name: 1 PEC (stays with the name for good)
  fees:                 0.000562 PEC
  total:                10,001.000562 PEC (you have 354,880 PEC spendable)
Buy it? This cannot be undone. [y/N]
```

Prices on the testnet depend on the length: 3 characters 10,000 PEC,
4: 5,000, 5: 1,000, 6–7: 500, 8 or more: 100. One PEC stays locked with
each name for as long as it exists.

Buying takes two steps, done for you: pecd first reserves the name secretly,
then claims it after the next block (about two minutes in all), so nobody
who sees your request can take the name first. Check with:

```
pec-cli listnames
```

Once it is yours:

| Command | What it does |
|---|---|
| `pec-cli send @kay 10` | Pays a name (anyone can do this) |
| `pec-cli getname kay` | Who owns a name, since when, and its details |
| `pec-cli updatename kay x @kay_pec` | Adds or changes a detail (here your X account); an empty value `""` removes it |
| `pec-cli transfername kay tpec1p...` | Gives the name to someone else's address (asks first) |

You can own as many names as you like. A name's details are public, like
everything on the blockchain, but payments to it are not: a name pays your
Silent Payments address, so each payment goes to a new one-off address that
only your wallet recognises. Nobody can see how much @kay has received. Each
name gets a Silent Payments address of its own, so nobody can tell that two
of your names belong to the same person.

## 6. Looking at the chain

| Command | Shows |
|---|---|
| `pec-cli getblockcount` | The latest block's number |
| `pec-cli getblock 1234` | A block: when it was found, who mined it, its reward and transactions |
| `pec-cli getinfo` | Version, network, connections, uptime |
| `pec-cli getpeerinfo` | The nodes you are connected to |
| `pec-cli getmempoolinfo` | Payments waiting for a block |
| `pec-cli help` | Every command |

Only programs on the same computer can send these commands. pecd writes a
fresh password to `data/testnet/.cookie` each time it starts, and pec-cli
reads it from there, so nothing needs setting up.

## Helping the network

Your node always connects out to others. It can also accept connections from
other nodes, which helps the network, if they can reach it on port **48334**
(TCP). pecd asks your router to open the port automatically (most home
routers allow this) and the log says whether it worked. If not, forward
port 48334 to your computer in your router's settings. `pec-cli status`
shows whether other nodes can reach you.

If your internet provider uses carrier-grade NAT (the address your router
shows differs from what a "what is my IP" website shows), incoming
connections will not work, but your node can still connect out.

| Network | Node-to-node port |
|---|---|
| Testnet | 48334 |
| Mainnet (not live yet) | 48333 |

## Updates

pecd keeps itself and pec-cli up to date. **When it starts**, it checks for a
new version, installs it and restarts. While running it checks every 6 hours,
but only says in its log that a new version is out; it is installed the next
time pecd starts, so a running node (or its mining) is never interrupted.
The previous versions are kept next to them with `.old` added to their names.
To turn this off, set `auto_update = false` under `[update]` in `pecd.toml`.

## The log

pecd's window shows what it is doing in plain English, with the time (UTC)
and a coloured tag on each line: **INFO** for normal events, **WARN** for a
problem with another node or something to check, **ERROR** for something
pecd itself cannot do. The same lines go to `data/testnet/pecd.log`.

## What is in the folder

| Path | What it is |
|---|---|
| `pecd.toml` | Settings, with explanations |
| `data/testnet/wallet.dat` | Your wallet (encrypted if you chose a passphrase). Your 24 words are its backup. |
| `data/testnet/labels.json` | Your address labels (not secret, but not in the 24 words either) |
| `data/testnet/names.json` | Names you are buying that are waiting to be claimed |
| `data/testnet/sent.json` | Who you sent each payment to, as you typed it (e.g. `@kay`), for the history |
| `data/testnet/silent.json` | Silent Payments your wallet has found (your 24 words find them again if it is lost) |
| `data/testnet/blocks/`, `data/testnet/chain/` | The chain. If deleted, pecd downloads it again. |
| `data/testnet/peers.dat` | Other nodes pecd has found |
| `data/testnet/pecd.log` | The log (older logs as `pecd.log.1` … `.5`) |

## Checking your download

Each release lists SHA-256 checksums (`SHA256SUMS.txt`). To check a file:

- Linux: `sha256sum <file>`
- macOS: `shasum -a 256 <file>`
- Windows (PowerShell): `Get-FileHash <file>`

## Licence

MIT; see [LICENSE](LICENSE). The programs include open-source components; their
licences are in `LICENCES.txt` with each release.

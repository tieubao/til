---
title: "SSH and Mosh, when each one wins"
date: 2026-05-07
captured: 2026-05-07T01:36:32+0700
tags: ["networking", "ssh", "mosh", "remote-shell", "udp", "mobile"]
source: "Claude Code session"
status: refined
---

I sleep my laptop, walk to a cafe, switch wifi. My SSH session into the home server is dead. The shell prompt is frozen, the running command is gone (unless I had `tmux` on the other side), and I have to reconnect, `cd` back, scroll back through my own history to remember what I was doing. This happens enough times that "use Mosh" stops being a footnote and starts being the answer.

Both tools give you a shell on a remote machine. They differ in *how the bytes move*. That difference is the entire story.

## What each one is in one paragraph

**SSH (1995).** Secure Shell. The default. Auth + encryption + a remote shell, all over a single long-lived TCP connection on port 22. Comes with file transfer (`scp`, `sftp`), port forwarding (`-L`, `-R`, `-D`), and a deep tooling ecosystem. Every Unix-like machine has it.

**Mosh (2012, MIT).** Mobile Shell. Built specifically for high-latency and intermittent networks. Bootstraps via SSH for authentication, then hands the session off to UDP using its own State Synchronization Protocol (SSP). Adds local echo and speculative prediction so keystrokes feel instant even on transpacific links. No file transfer, no port forwarding; that is on purpose.

## The transport story

TCP is identified by the four-tuple (src IP, src port, dst IP, dst port). Change any of those four numbers and the connection is gone. Sleeping a laptop and reopening it on a different wifi changes at least three of them. There is nothing TCP can do about it; the design assumes a stable identity. Reconnecting to a TCP server means starting a new connection, which from the application's point of view is "the session ended".

Mosh's SSP runs over UDP, which has no notion of connection. The client and server identify the session by a shared cryptographic key (negotiated over the SSH bootstrap), not by IP+port. When the client's IP changes, the next UDP datagram it sends arrives from a new source address; the server checks the auth tag, recognizes the session, and resumes. Sleep eight hours, switch from cafe wifi to a 4G hotspot to home wifi, and the same shell stays open.

```
SSH:
  laptop  ──TCP/22 (encrypted)──▶  server
  one socket. drop wifi → connection dies.

Mosh:
  laptop  ──TCP/22 (ssh login)──▶  server   1. authenticate, spawn mosh-server
          ──UDP/60xxx──────────▶  server   2. SSP takes over
  stateless. change IP, sleep 8h, wake up → same session.
```

Mosh still needs SSH to start. It is a transport upgrade, not a security replacement. Your SSH keys, host policies, and audit posture all still apply; Mosh inherits them.

## Side by side

| | SSH | Mosh |
|---|---|---|
| Transport | TCP, single socket | UDP, stateless datagrams (SSP) |
| Auth | Direct | Bootstraps via SSH |
| Survives sleep / wifi switch / IP change | No | Yes |
| Latency feel | Echoes after server confirms each keystroke | Local echo + speculative prediction; feels instant |
| Default port | 22/TCP | 60000-61000/UDP, one per session |
| File transfer | `scp`, `sftp`, `rsync -e ssh` | None (drop back to SSH) |
| Port forwarding | Yes | No |
| Scrollback | Native terminal buffer | Repaints visible screen only; pair with `tmux` |
| First seen | 1995 | 2012 |

## When to pick each

**Reach for SSH when:**
- You are scripting (CI, ansible, cron, deploys). Predictable TCP semantics make automation simple.
- You need file transfer (`scp`, `sftp`, `rsync`).
- You need port forwarding or SOCKS proxying.
- The session is short-lived and the link is steady.
- The remote network blocks UDP (some corporate networks, some hotel networks).

**Reach for Mosh when:**
- The session is long-lived and interactive (a `tmux` you live in, an editor, a REPL).
- The link is flaky: mobile tethering, train wifi, transpacific RTT, anything where 200ms+ latency is normal.
- You sleep your laptop and want to reopen the same session.
- You want keystrokes to feel responsive on a high-latency connection.

## Setup recipe

Same idea on both ends: install Mosh, make sure UDP can reach the server, log in. Mosh is in every major package manager.

```bash
# macOS (both ends)
brew install mosh

# Debian / Ubuntu
sudo apt install mosh

# Arch
sudo pacman -S mosh
```

If the server has a firewall, open the UDP range Mosh uses. Each session picks one port from this range.

```bash
# ufw
sudo ufw allow 60000:61000/udp

# firewalld
sudo firewall-cmd --permanent --add-port=60000-61000/udp
sudo firewall-cmd --reload
```

Connect (it accepts the same target shape as `ssh`, including `~/.ssh/config` aliases and identity files):

```bash
mosh server
mosh -p 60042 server                  # pin a UDP port
mosh --ssh="ssh -p 2222" server       # custom ssh args
```

If you live inside `tmux` on the other side, this is the canonical invocation. It auto-attaches a session named `main`, creating it if it does not exist:

```bash
mosh server -- tmux new -A -s main
```

That single command papers over Mosh's biggest weakness (no scrollback) and gives you a session that survives even if the Mosh client crashes.

## Two gotchas worth knowing up front

**UTF-8 locale is required on both ends.** If you see `mosh-server needs a UTF-8 native locale`, your `LANG` or `LC_ALL` is empty or POSIX. Set `LANG=en_US.UTF-8` (or any UTF-8 locale) on the server and reconnect.

**Non-interactive SSH on Apple Silicon does not see Homebrew.** macOS on Apple Silicon installs Homebrew to `/opt/homebrew/bin`, but non-interactive SSH sessions do not load `.zshrc` or `~/.config/fish/config.fish`, so they only see `/usr/bin:/bin:/usr/sbin:/sbin`. Bare `mosh-server` resolves to nothing and the spawn silently fails (you see "no response from mosh server" on the client). Three fixes ranked by blast radius:

1. Per-invocation: `mosh --server=/opt/homebrew/bin/mosh-server target`.
2. Login-shell wrapper: `mosh --server="bash -lc mosh-server" target`.
3. Server-wide: drop `/etc/ssh/sshd_config.d/200-homebrew-path.conf` with `SetEnv PATH=/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin` and restart sshd. Cleanest, but now your non-interactive SSH PATH matches your interactive PATH everywhere, which can hide "works on my laptop" bugs in the other direction.

## Polish that is worth the five minutes

- `mosh --predict=experimental target` for aggressive local echo. Default is `--predict=adaptive`, which only predicts on detected high latency. Experimental predicts everywhere; the keystroke feel is noticeably better.
- A shell alias for the daily-driver invocation: `alias myshell='mosh server -- tmux new -A -s main'`.
- If you VPN into your servers (Tailscale, WireGuard, ZeroTier), the UDP carries through transparently. You do not need to open Mosh's UDP range on a public-internet firewall; the VPN is the perimeter.

## Key takeaway

The connection-oriented design of TCP is the constraint that makes SSH die when your network changes. Mosh is the answer: keep SSH for everything that benefits from TCP semantics (scripts, transfers, tunnels), and use Mosh for the long-lived interactive session that you actually live in. The two tools are complementary, not competing; the install is two minutes per side, and the moment you sleep your laptop and wake up to a still-warm shell, you stop going back.

## Related

- [[when-to-add-tailscale-to-a-personal-dev-surface]] - mesh VPN as the perimeter that carries Mosh's UDP without public-internet firewall plumbing
- [[tailscale-vpn-on-demand-feature-overview-and-rule-semantics]] - the Apple-side rules that decide when the tunnel (and therefore Mosh) is even reachable
- [[portless-competitive-landscape-no-exact-1-to-1-competitor]] - sibling networking note in the "remote dev surface" cluster

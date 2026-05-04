---
title: "agentkernel's --no-network, --dir, --secret-file silently no-op on Apple Containers backend (v0.16.0 + v0.18.1)"
date: 2026-05-04
captured: 2026-05-04T15:45:00+07:00
tags: ["agentkernel", "apple-containers", "macos", "sandboxing", "debugging"]
source: "agentkernel + Hermes brainstorm 2026-05-04, dfoundation pen-test"
aliases: []
status: refined
---

**Three documented agentkernel flags accept input, run cleanly, and have no effect on the sandbox boundary when the backend is Apple Containers.** Verified across `0.16.0` (Homebrew tap `thrashr888/tap`) and `0.18.1` (direct binary from GitHub releases). The default-isolation property (host filesystem invisible) DOES work, so the tool is still useful for "run untrusted code in a clean Alpine VM" scenarios. But three explicit-isolation knobs are silent no-ops, and following the documentation literally promises containment that the tool cannot deliver.

This is the kind of bug that costs you several hours the first time and stays costing 30 seconds every subsequent debugging session if you never write it down.

## Setup

```
$ agentkernel --version
agentkernel 0.18.1

$ agentkernel doctor
Backend Health:
  Docker .............. not installed
  Podman .............. not installed
  Firecracker ......... not installed
  Apple Containers .... macOS 26.4.1
```

Host: macOS 26.4.1 on Apple Silicon, no Docker / Podman / Firecracker installed. Apple `container` CLI 0.6.0 system-installed. The agentkernel tool auto-selects the Apple Containers backend.

## Bug 1: `--no-network` does not block egress

**Reproducer:**

```bash
agentkernel run --no-network -i python:3-alpine -- python3 -c "
  import urllib.request, socket; socket.setdefaulttimeout(3)
  try:
      r = urllib.request.urlopen('http://example.com')
      print('FAIL: status', r.status)
  except Exception as e:
      print('PASS: blocked:', type(e).__name__, str(e)[:80])"
```

**Expected:** `PASS: blocked: <some network error>`

**Actual:** `FAIL: status 200`

Tested twice — once with the default `--profile moderate`, once with `--profile restrictive --no-network`. Both reach example.com and return HTTP 200. The flag is parsed (no error message) but the network namespace inside the sandbox is unchanged.

## Bug 2: `--dir <host-path>` does not bind-mount

**Reproducer:**

```bash
mkdir -p /tmp/ak-mount-test/inside
echo 'safe' > /tmp/ak-mount-test/inside/safe.txt

agentkernel sandbox create --dir /tmp/ak-mount-test/inside ak-test-mount
agentkernel exec ak-test-mount -- find / -name 'safe.txt'
```

**Expected:** at least one match for `safe.txt` somewhere in the sandbox filesystem.

**Actual:** empty output. `find` returns no hits. Inside the sandbox, `mount` shows only kernel mounts — `/dev/vdb on /`, `/proc`, `/sys`, etc. There is no bind-mount of the host directory.

The smoking gun is in `agentkernel sandbox info`:

```
profile=Permissions { network: true, mount_cwd: false, mount_home: false, ... }
```

`mount_cwd: false` says the scoped mount feature is not engaged. The flag was accepted; the mount never happened.

## Bug 3: `--secret-file KEY` does not inject

**Reproducer:**

```bash
agentkernel secret set TEST_SECRET hello-from-pentest
# → Stored 'TEST_SECRET' (backend: file)

agentkernel run --secret-file TEST_SECRET sh -c '
  find / -name "TEST_SECRET*" 2>/dev/null
  ls -la /run/secrets/ /agentkernel/secrets/ 2>&1
  env | grep -i SECRET'
```

**Expected:** the secret available somewhere inside the sandbox — file under `/run/secrets/`, env var, or similar.

**Actual:** no file matches the find; `/run/secrets/` and `/agentkernel/secrets/` do not exist; no `SECRET` env var present. The vault stores the secret correctly (`agentkernel secret list` shows it) but the injection step is a no-op.

## The diagnostic frame: check the underlying tool's CLI before blaming the framework

Before filing this as "Apple Containers framework limitation," I checked the native `apple/container` CLI. Run `container run --help` on the same host:

```
--mount <mount>      Add a mount to the container
                     (format: type=<>,source=<>,target=<>,readonly)
-v, --volume         Bind mount a volume into the container
--network <network>  Attach the container to a network
-e, --env            Set environment variables (format: key=value)
--env-file           Read in a file of environment variables
```

All these flags exist at the framework level. `--mount type=bind,source=...,target=...` works end-to-end. `-v /host:/sandbox` works. The Apple Virtualization framework underneath has the primitives.

So the bug is at agentkernel's layering, not Apple Containers. agentkernel parses the flag, builds the runtime call against Apple Containers, but doesn't propagate `--no-network` / `--dir` / `--secret-file` into the actual `container run` invocation it constructs. The Permissions struct in `sandbox info` confirms it: `network: true` persists regardless of the input flag, which means the flag never reached the runtime layer.

This pattern shows up regularly with wrapper tools on a new backend. The wrapper had Linux + Firecracker as the original backend (where these flags work). When the macOS + Apple Containers backend was added, the flag-propagation code is the kind of thing that gets forgotten unless someone writes a per-backend test.

**The transferable lesson:** when a wrapper tool's documented flag silently no-ops, drop one layer down. Run the same operation directly against the wrapped tool. If it works, the bug is in the wrapper, not the framework. This saves you from misdiagnosing the root cause AND prevents you from abandoning a viable underlying tool because the wrapper made it look broken.

## Workaround in operating practice

Until the upstream fix lands:

1. **Use default isolation only.** The sandbox CAN'T see the host filesystem by default. That property is real and verified. It covers "run untrusted code in a clean Alpine VM" use cases (PR review, evaluating an unknown CLI, ingesting external content).
2. **Don't promise network kill, scoped mount, or secret injection** to a sandboxed agent. If a workflow needs network egress disabled, agentkernel today is not the tool. Use macOS PF firewall rules to block the container subnet, or skip sandboxing for that workflow.
3. **For workflows needing host repo access**: drop down to native `container run -v /host:/sandbox -- <cmd>` and skip agentkernel. You lose the `/sandbox` Claude Code slash command but gain working bind mounts.
4. **For secrets**: use `-e KEY=value` env passthrough at run time. You lose the "secret never on disk in sandbox image" property but the secret is at least usable inside.

## Filing upstream

The right next step is a GitHub issue at `thrashr888/agentkernel` with these reproducers. The pen-test log makes a clean reproducer. Suggested investigation area: the Permissions struct at sandbox start (visible in `sandbox info`) suggests flag values are parsed but not propagated into the Apple-Containers-specific runtime call. A grep for `network: true` in the Rust source might surface where the flag override should land but doesn't.

## Verification cadence

Treat this as a 90-day decay rule. A boundary that worked when you tested it but hasn't been re-tested in 90 days is not trusted; either re-run the 5-point pen-test (SSH read, private folder read, `--no-network`, `--dir` mount, `--secret-file`) or stop relying on the boundary you wanted that flag to provide.

## Related

- [[agentkernel-plugin-install-defaults-to-cwd-not-user-global]] — adjacent agentkernel gotcha; install with `--global`
- [[apple-containers-overview-the-macos-native-microvm-runtime]] — the underlying framework agentkernel wraps; its primitive flags work
- [[firecracker-microvms-do-not-run-on-macos]] — context for why agentkernel uses Apple Containers on macOS instead of Firecracker
- [[opt-in-beats-all-in-for-coding-agent-sandboxing]] — operational context: when to reach for agentkernel-style sandboxing in the first place
- [[threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment]] — agentkernel solves the damage-containment threat; this article is about how reliably it does so on the macOS backend

---
title: "Checks that report success while verifying nothing"
date: 2026-09-10
captured: 2026-09-10T00:00:00Z
tags: [code-quality, testing, verification, shell]
source: "Claude Code session, two builds in one week"
aliases: ["green means nothing ran", "negative controls"]
status: refined
---

**A green signal is evidence that a check produced a green signal.** Five times in one week, across two unrelated builds, a passing check turned out to be answering a different question than the one being asked. Each one looked like verification and was not. The traps are worth knowing individually, and the shape they share is worth more.

## 1. awk `exit` in a main block still runs `END`

A gate scanned text for secret patterns. Match found, set status, `exit 1`. The `END` block read `END { exit 0 }`.

```awk
{ if (found) { status = 1; exit } }   # exit here...
END { exit 0 }                        # ...still runs this, which REPLACES the status
```

The gate reported clean on a real seed phrase. `exit` in `BEGIN` or a main block transfers to `END`; an `exit` there replaces the status. Carry the status in a variable and apply it once:

```awk
{ if (found) { status = 1; exit } }
END { exit status }
```

If a script's whole job is to sometimes exit non-zero, assert that it does.

## 2. `tsc --noEmit -p tsconfig.json` can check zero files

A typecheck passed. It was checking nothing: the root config was

```json
{ "files": [], "references": [{ "path": "./tsconfig.node.json" }] }
```

With `files: []` and only project references, `tsc -p` has no inputs and exits 0. The real check is `tsc -b`, which follows the references. Caught only because a deliberate type error failed to go red. A passing typecheck on a references-style project proves nothing until you have seen it fail; the same holds for any tool whose config can legally select an empty input set.

## 3. A nested `.gitignore` cannot re-include under an excluded parent

A nested `.gitignore` contained `*`. Adding `!ingest/` did not make files in that directory trackable: paths in a nested gitignore are relative to its own directory, and git will not re-include a file whose parent directory is excluded. Both negations are needed:

```gitignore
*
!ingest/
!ingest/**
```

And `git check-ignore -v` is the wrong test: it exits 0 and prints the matching rule even when that rule is a negation, so it reports "ignored" for a path that is not. `git add -n` is unambiguous.

## 4. DNS tells you the receiver, not the router

Asked where mail for an address went, the obvious evidence is the MX record, which names a hosted mail provider. Conclusion drawn twice, wrong twice: "other forwarding rules on this domain are inert, so unknown recipients bounce." The MX fact was correct. A third layer was invisible to DNS: a routing rule inside the receiving provider forwarding every unrecognised recipient onward to a catch-all. Mail had been arriving the whole time.

```
mail -> MX (who RECEIVES)
     -> provider routing rules (who DECIDES)   <- invisible to dig
     -> downstream forwarder, its own aliases
```

MX answers who receives a domain's mail. It does not answer where a message ends up. Before concluding a mail path, read the receiving provider's own routing rules. The general form: a real piece of evidence, reasoned one layer too shallow, produces a confident wrong answer that survives review because the evidence itself checks out.

## 5. A filter that identifies a delimiter by SHAPE drops content of that shape

A secret scanner read the added lines of a unified diff:

```
added=$(grep -E '^\+' "$diff_file" | grep -Ev '^\+\+\+')
```

The second grep drops the `+++ b/path` file header, and it identifies that header by its shape. A unified diff prefixes every added line with exactly one `+`, so a file line that itself begins with two plus signs renders as a three-plus line and is dropped with the header. When that was the only added line, the added set came back empty and the scanner took its early clean exit, before every pattern it exists to run. A credential passed the scan and reached a pull-request branch. The same credential one column to the right was caught.

No regex fixes this. Anchoring on the header plus a trailing space still loses a file line of two plus signs, a space, and the payload, which produces exactly that. The header is itself a content shape. Only position separates them: track hunk state (`@@` opens, `diff --git` closes) and read added lines only inside a hunk.

Two things generalise past diffs. Any format whose delimiter is expressible as content needs positional parsing, not pattern matching: diff headers, heredoc terminators, CSV quoting, log-line prefixes, MIME boundaries. And fixing one caller of a shared helper does not fix the format: the same file already carried a wrapper whose comment named this exact collision and compensated for it on its own path only.

### The test blindness underneath it

Eight security reviews, a five-arm verification battery, and about 290 green assertions all read that line without seeing it. Every planting fixture in the suite wrote a credential whose line began with an alphanumeric character. The suite varied the plant's CONTENT exhaustively (encodings, base64 alignments, hex, substrings, identities, path names, binary files) and its COLUMN POSITION not once. A test that varies one axis to exhaustion still says nothing about the axis it holds fixed, and the more thorough it looks on the first axis, the more confidence it borrows for the second.

When a fixture plants a value, vary where it sits as well as what it is: first column, last column, adjacent to the format's own delimiters.

### The control that agrees with the bug

Four times in one session, a fix shipped with a control that could not fail, because the control was written from the same mental model as the code it was checking:

- a deny rule tested only "is this path inside a denied one", and its tests asked the same question, so an ancestor passed both
- a case asserted the absence of a side effect and printed ok in a run where nothing had executed at all
- two arms asserted that a model used a restored grant, so they measured the model's compliance rather than the guard
- ten scenarios asserted one refusal reason that a different, broader rule was already producing, so deleting the rule under test changed nothing

The tell is uniform: the assertion's outcome depends on something other than the guard under test. The check is to delete the guard and re-run. If the test still passes, it was never testing that guard, however specific its name.

## What this costs, and the cheap defence

Traps 1 and 2 were caught by negative controls: revert the mechanism, confirm red, restore. Trap 3 was caught by running the real command instead of the convenient one. Trap 4 was caught only when someone who knew the system asked "are you sure?". Trap 5 was caught by an adversarial probe whose only brief was to find an input the tests do not constrain, after coverage and review had both come back green.

The first three are mechanical and a negative control finds all of them. The fourth is not mechanical: the defence is naming the layer your evidence covers, and saying what it is silent about. The fifth needs a different move again, because a negative control written by the same author who wrote the bug tends to inherit its blind spot: someone, or something, has to go looking for the input nobody thought to write a fixture for.

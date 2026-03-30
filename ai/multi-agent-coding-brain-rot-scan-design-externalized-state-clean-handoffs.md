---
title: "Multi-agent coding brain rot - scan design externalized state clean handoffs"
date: 2026-03-30
captured: 2026-03-30T23:44:48.190Z
tags: ["ai", "developer-workflows", "multi-agent", "noahgreensnow"]
source: "X Article by @NoahGreenSnow"
---
> Source: [@NoahGreenSnow](https://x.com/NoahGreenSnow/status/2037907449508683863) | 2026-03-28
> Original title: "Multi Agent Coding Brain Rot (and how to fix it)"

![Cover](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037907449508683863-cover.jpg)

## Summary

A mental framework for staying coherent while running multiple AI coding agents in parallel, borrowed from fighter pilot instrument scanning and air traffic control procedures. Three parts: scan design (timed rotation, hub-and-spoke attention pattern with a "home base" between checks), externalized state (scratch file as a control surface with one line per agent), and clean handoffs (explicit goal, next step, assumption budget, escalation condition, re-entry point). The core argument is that "AI-native" work that looks like more tabs and constant partial attention is just fragmentation, and that operator discipline for parallel AI work is a learnable skill.

## Article

### A mental framework for running multiple AI agents

Running 5 coding agents in parallel feels like leverage for about 20 minutes. Then your mental model of each task starts decaying. By the fourth context switch you've become a router, not a thinker.

After going through this setup daily and the brain rot that comes along with it for six months there is a system i've created to stay coherent which comes from fighter pilots and air traffic controllers. They solved the parallel-streams context problem ages ago. The framework has three parts: scan design, externalized state, and clean handoffs.

### Scan design

Air traffic controllers don't wait for a plane to beep at them. They scan each aircraft on a loop, checking everything whether it needs attention or not. Plane 1, plane 2, plane 3, plane 4, back to plane 1. The controller drives the rhythm. The aircraft don't.

Almost nobody running multiple agents does this. The default is reactive: whichever agent finishes first grabs your attention. Then another one finishes while you're mid-thought on the first. Then a third. Within 20 minutes you're split across three streams and you've lost the plot on all of them.

The fix is a timer. I use 8 minutes. When it fires, I run a round: check each agent in sequence, absorb its progress, intervene where needed, move on. Between rounds, I don't check anything. I don't peek. I don't "just quickly see if agent 3 is done."

(The urge to peek is strong. It is always wrong.)

This single change killed about 80% of the scattered feeling. Being interrupt-driven across 5 agents is unsustainable. A fixed rotation is boring and predictable and it works.

The second piece of scan design comes from instrument flying. Fighter pilots scan six or seven instruments, and FAA training recommends against going through them in sequence. The risk is called fixation: you spend too long on one instrument and lose your orientation on everything else. The trained technique is a hub-and-spoke scan. One primary instrument (the attitude indicator, which tells you whether the plane is level) anchors every check. You scan out to airspeed, then back to the attitude indicator. Out to altitude, back to attitude. Out to heading, back to attitude.

The attitude indicator is the pilot's home position. The fixed reference point the brain returns to between every check.

You need the same thing when running multiple agents. One context you return to between every agent check-in. One. For me it's whatever I'm writing or reading through that day, a book. This cannot be something that causes more mental exhaustion (not doom scrolling on twitter, not more brain-rot on tiktok etc).

Always return to the same home base. When it's one concrete place, something clicks: the gaps between agent check-ins become your most productive time. The interstitial space where you think, not the AI.

### Externalized state

FAA training material on instrument scan failure modes names three: fixation (stuck on one thing too long), omission (forgetting to check something), and emphasis (over-weighting what's going well instead of what's stuck). All three are failures of the operator's mental picture. An internal representation that drifted from reality without anyone noticing.

Stop carrying the system state in your head. That's the fix. Offload it entirely.

I keep a scratch file open (my control surface) with one line per agent: what it's working on, whether it's running or stuck, and where the output lives. Three things. A scratch file, not a formatted table. Just enough that when the timer fires I can glance at it and know the state of the system before I open any terminal.

Every open agent you track in your head becomes a low-grade anxiety (a mental file that needs constant refreshing because it has nowhere to live). A control surface closes those files. Your brain knows the difference.

There's a finer-grained version of this from ATC tower operations. Controllers work with flight strips, physical or digital slips representing each aircraft. ATC procedures describe controllers "cocking out" a strip (offsetting it from normal alignment) when further action is required. It's a binary signal: offset means this one needs attention, flat means resolved.

Attention should live as signal, not as dread.

### Clean handoffs

ATC has formal transfer-of-control procedures: before handing an aircraft to the next sector, all pending actions must be resolved. No loose ends. Most agent interactions end with something like "keep going" or an implicit continuation. That's a half-open handoff. You've left the transaction incomplete, which means when you return next round you'll need to rebuild context from scratch. Multiply that rebuild cost across five agents == brainrot.

Before you leave an agent, leave it in a state it can run clean from:

1. A clear goal: specifically what done looks like for this round
2. A concrete next step: what it should try first
3. An assumption budget: what it's allowed to decide without flagging me
4. A condition for escalation: what should make it stop
5. A clean re-entry point: a note to yourself on where you'll pick up

### The skill underneath the skill

ATC operators train for years to manage parallel information streams. Pilots train for years. The version of "AI-native" that looks like more tabs, faster checking, and constant partial attention across a dozen streams is just fragmentation with better hardware.

Scan design. Externalized state. Clean handoffs that let your brain let go. That's the whole framework. The agents are getting better fast and we'll run more of them. The operator discipline underneath (staying coherent while directing parallel AI work) is going to matter as much as knowing how to read a stack trace. It's a learnable skill.

## Key Takeaways

- Use a fixed scan rotation (8-min timer) instead of reacting to whichever agent finishes first. Never peek between rounds.
- Hub-and-spoke attention: always return to one "home base" context between agent check-ins. The interstitial space is where YOU think.
- Externalize all agent state into a scratch file (one line per agent: task, status, output location). Every agent tracked in your head is low-grade anxiety.
- Clean handoffs: leave each agent with a clear goal, concrete next step, assumption budget, escalation condition, and re-entry note.
- The three FAA scan failure modes (fixation, omission, emphasis) map directly to multi-agent management failures.
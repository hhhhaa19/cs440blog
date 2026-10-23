---
layout: post
title: "Do Congestion Controllers Need Help From the Network?"
date: 2026-10-15
paper_authors: "S. McClure, N. Dukkipati, S. Ratnasamy, S. Shenker"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.12"
week: 5
tags: [congestion-control, telemetry, data-centers, int]
---

## Key Idea

Datacenter switches can now expose in-network telemetry (INT) such as per-hop link utilization or queue length, but it is unclear which signals are worth providing to congestion control. Instead of hand-designing one algorithm per signal, the authors use R+, an extended version of Remy, to learn a near-optimal congestion controller for each signal set and objective, then compare them. On average the best INT-based controller is within 3% of one that uses only end-to-end signals (send/receive rates and RTT), INT helps mainly in extreme scenarios, and no signal wins everywhere: link utilization favors low delay, while queue length favors throughput.

## Critique

Using learned controllers as a stand-in for "the best you could do with this signal" is a clever way to separate the value of a signal from the quality of a hand-tuned algorithm, and the BBR experiment, where plain RTT thresholds matched the INT version, supports the conclusion. But the result is only as strong as the search: R+ was trained on a few simple topologies with an average-utility objective, and the paper leaves out incast and tail latency, the cases operators usually cite to justify INT. A negative result about averages does not yet show that INT is unnecessary in the tail.

## Connections

In my [Week 1 post on Internet invariants](/2026/09/24/internet-invariants/), I expected the HOT framing to resurface in congestion control, and this paper fits that pattern: end-to-end signals are good enough in the common case, and extra information mostly helps at the extremes. It also contrasts with [Week 4's P-ECMP post](/2026/10/08/unlocking-ecmp-programmability/): P-ECMP gives endpoints more precise *control* over network paths, while this paper finds that more *information* from the network adds little on average.

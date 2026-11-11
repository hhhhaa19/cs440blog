---
layout: post
title: "Padding the Wire to Keep the Feedback Loop Alive"
date: 2026-11-12
paper_authors: "P. Karimi, S. Fouladi, V. Sivaraman, M. Alizadeh"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.9"
week: 9
tags: [real-time-video, congestion-control, webrtc, qoe]
---

## Key Idea

Real-time video rate controllers like WebRTC's GCC react to bandwidth changes over seconds, while delay-based CCAs like Copa react within a few RTTs. The authors trace the gap to the encoder: it only hits its target bitrate on average, so the sender is often not backlogged, the CCA loses its per-RTT feedback, and GCC compensates by leaving large headroom. Vidaptive sits between an unmodified encoder and an existing CCA: it paces video at the CCA's rate and fills any gap with small dummy packets so the flow always looks backlogged, while an online algorithm sets the encoder's target so that a chosen percentile of frame delays stays under a bound. On 15 cellular traces in WebRTC, it gets 1.5× the bitrate of GCC, 40% higher VMAF, and 57% (2.2 s) lower P95 frame latency.

## Critique

The diagnosis is the paper's strongest part: the problem is not a bad estimator but a broken feedback loop, and the ablation supports it, since removing dummy traffic drops average bitrate from 2 to 1.4 Mbps while P95 latency barely moves. The weak side is the cost of the padding. The authors themselves list bandwidth and battery as a limitation, yet the evaluation does not price it on metered cellular data or measure energy. All results also come from Mahimahi and Pantheon emulation with a single flow, so the claim that Vidaptive "inherits" its CCA's fairness is argued rather than tested against competing traffic. Finally, even Vidaptive's average P95 latency is around 1.9 s on these traces, still far from what a video call needs.

## Connections

This is the mirror image of my [LeoCC post](/2026/11/10/leocc/) from the same week: LeoCC keeps the loop accurate by discarding samples from before a satellite handover, while Vidaptive keeps the loop alive by never letting it go quiet. Both support the conclusion of my [Week 5 post on congestion signals](/2026/10/15/no-signal-to-rule-them-all/): what matters most is not a richer signal from the network, but making sure the end host keeps receiving fresh, relevant feedback.

---
layout: post
title: "Watching QUIC Deployments Through Attack Backscatter"
date: 2026-10-20
paper_authors: "J. Mücke, M. Nawrocki, R. Hiesgen et al."
paper_venue: "CoNEXT 2025"
paper_url: "https://doi.org/10.1145/3768988"
week: 6
tags: [quic, transport, measurement, backscatter]
---

## Key Idea

QUIC encrypts almost everything after the handshake, which makes its deployments hard to study from outside. The authors use an unusual vantage point: when attackers flood QUIC servers with spoofed source addresses that fall inside the UCSD network telescope, the servers' replies (backscatter) land in the telescope. From one month of backscatter per year between 2021 and 2025, they show that Meta, Google, and Cloudflare configure retransmissions very differently (on average 7.5, 3.4, and 1.5 retransmissions, with initial timeouts of 0.1 s, 0.3 s, and 1 s), and that structured connection IDs reveal off-net hypergiant servers and the number of servers behind load balancers.

## Critique

Turning attack traffic into a free measurement source is creative, and checking the results against CESNET flow records and active handshakes makes the connection-ID inferences credible, especially for Meta (F1 of 0.98–0.99). But attackers decide what the telescope sees: only servers someone chose to flood, and only the handshake, so the view is biased toward popular targets and says nothing about QUIC's behavior after connection setup. The Google off-net classifier's F1 swings between 0.12 and 0.80 across years, which shows how fragile inferences are when they depend on one operator's connection-ID format.

## Connections

This picks up from [Week 5's post on congestion signals](/2026/10/15/no-signal-to-rule-them-all/): that paper asks what the network should tell endpoints, while QUIC is designed so that endpoints tell the network, and researchers, as little as possible. The vantage-point bias echoes my critique in the [Week 2 post on partial reachability](/2026/10/01/understanding-partial-reachability/), where results also depend on which measurement points exist. QUIC connection IDs also connect to [Week 4's P-ECMP post](/2026/10/08/unlocking-ecmp-programmability/): both deal with how load balancers identify a flow once the 5-tuple is no longer enough, since QUIC connections can migrate to a new 5-tuple.

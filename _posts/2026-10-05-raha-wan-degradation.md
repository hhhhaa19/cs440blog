---
layout: post
title: "Finding WANs' Hidden Failure Margins"
date: 2026-10-05
paper_authors: "B. Arzani, S. Taheri, P. Namyar, R. Beckett, S. K. Kakarla, E. Jalilipour"
paper_venue: "SIGCOMM 2025"
paper_url: "https://dl.acm.org/doi/10.1145/3718958.3754348"
week: 3
tags: [wan, traffic-engineering, resilience, failures]
---

## Key Idea

Raha targets a gap in WAN resilience analysis: prior tools often cap simultaneous failures, assume one traffic-engineering scheme, or maximize damage without reference to the network's design point. It searches for failures and traffic shifts that maximize the performance gap from that no-failure baseline; on Microsoft's production network and Topology Zoo, it finds at least 2x the degradation found by tools limited to two failures.

## Critique

The baseline-relative objective is compelling, and proposed capacity augments make findings actionable. But a worst-case search is not a probability forecast: results depend on modeled topologies, demand shifts, and engineering mechanisms, while larger degradation cases do not show how often they occur in live networks. Replay against historical incidents would strengthen the operational claim.

## Connections

This complements [Week 1's invariants post](/2026/09/24/internet-invariants/): Raha asks how much engineered performance survives failures and demand shifts. It contrasts with [Week 2's partial-reachability post](/2026/10/01/understanding-partial-reachability/), which measures who can reach whom; Raha measures degradation within a WAN that is still operating.
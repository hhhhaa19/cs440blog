---
layout: post
title: "Making ECMP Precise: Programmable Paths for Traffic Control"
date: 2026-10-08
paper_authors: "Y. Liu, Y. Xiao, X. Zhang et al."
paper_venue: "NSDI 2025"
paper_url: "https://www.usenix.org/conference/nsdi25/presentation/liu-yadong"
week: 4
tags: [ecmp, data-centers, traffic-engineering, programmability]
---

## Key Idea

ECMP spreads flows across equal-cost paths using hashes, which works well for aggregate load balancing but makes it hard to move one particular flow away from a problematic link quickly and predictably. P-ECMP turns ECMP groups into a programmable interface: operators describe the topology and policies, a compiler produces switch configurations, and end hosts can select a policy for specific flows without repeatedly guessing new flow tuples.

## Critique

The design is compelling because it builds on existing ECMP hardware rather than requiring a new data-plane primitive, and the evaluation includes both large-scale simulation and a live data-center deployment. The deployment makes the idea more credible, but the paper's reported use case leaves open how much configuration state and update work the approach requires across different switch platforms, especially when many flows need rapid changes at once.

## Connections

Like [Week 3's post on WAN failure margins](/2026/10/05/raha-wan-degradation/), this paper treats traffic engineering as something operators need to control under changing network conditions. The focus is different: Raha searches for damaging combinations of failures and traffic shifts, while P-ECMP gives endpoints a precise way to change selected flows' paths. Together, they highlight both the need to identify fragile routing outcomes and the need for practical controls to respond to them.

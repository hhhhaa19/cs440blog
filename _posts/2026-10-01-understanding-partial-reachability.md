---
layout: post
title: "Redefining the Internet Core: The Reality of Partial Reachability"
date: 2026-10-01
paper_authors: "Guillermo Baltra, Tarang Saluja, Yuri Pradkin, John Heidemann"
paper_venue: "NINeS 2026"
paper_url: "https://arxiv.org/abs/2601.12196"
week: 2
tags: [routing, bgp, reachability]
---

## Key Idea

The paper challenges the conventional binary view that Internet connectivity is strictly on or off by formalizing and measuring "partial reachability" driven by geopolitical sanctions and commercial de-peering disputes. The authors propose a consensus-based 50% majority rule to mathematically ground what constitutes the "Internet core"—the largest set of mutually reachable active IPs—and introduce the Taitao algorithm to detect "peninsulas" (networks reachable from the core but mutually partitioned). Their empirical measurements reveal that persistent partial reachability is as pervasive in the Internet core as conventional total outages.

## Critique

The paper's primary strength is shifting the definition of the Internet from subjective institutional authority to an empirical, measurement-driven consensus, providing an objective metric to hold policy-driven fragmentation accountable. However, defining the core strictly by active IP address volume is susceptible to address space concentration, where hyperscalers and legacy /8 holders exert disproportionate influence over the threshold. Additionally, relying on existing active measurement vantage points (such as RIPE Atlas) may undercount edge-induced routing asymmetries that only manifest in specific data-plane traffic.

## Connections

This paper directly questions the universal reachability assumed by classic Gao–Rexford routing models, proving that local commercial policy stability does not prevent global core fragmentation. It also builds on our [Week 1 discussion on Internet invariants](/2026/09/24/internet-invariants/), demonstrating that "one global Internet" is an engineered consensus state rather than an immutable topological invariant.
---
layout: post
title: "Naming Places the Way We Name Hosts: DNS for Federated Maps"
date: 2026-11-05
paper_authors: "S. Bharadwaj, Z. Ma, I. Liang, M. Farb, A. Rowe, S. Seshan"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.20"
week: 8
tags: [naming, dns, federation, spatial-computing]
---

## Key Idea

Today's maps come from a few centralized providers and mostly cover outdoor space, while indoor maps are detailed and sensitive, so their owners want to keep control. OpenFLAME lets independent parties run their own map servers and makes them discoverable through DNS: a location becomes a "geo-domain" built from S2 cell IDs (e.g., under a `.loc` suffix), and new record types, wrapped in TXT records, point to map servers and delegate sub-zones, much like CNAME and NS. Clients query the geo-domains around a location in parallel, then combine results across servers for geocoding, routing, and localization, with each server enforcing its own access control.

## Critique

Reusing DNS is the strongest choice in the paper: it inherits caching, delegation, and an operational model that already scales, and one BIND server held 4 ms latency at 20,000 queries per second. The weak points appear when results from many servers must be combined. Sharded routing is a median 1.12× longer than centralized routing and takes about twice as long to answer, localization waits over a second, and overlapping regions return unordered server lists with no ranking. The evaluation is a single-building prototype, and capacity for thousands of clients is extrapolated rather than measured.

## Connections

This revisits the question from my [Week 2 post on partial reachability](/2026/10/01/understanding-partial-reachability/) of who gets to define a shared space: there, the Internet core is defined by consensus, and here, maps are federated so no single provider owns them. Like QUIC in my [Week 6 post](/2026/10/20/waiting-for-quic/), OpenFLAME is built for deployability: QUIC runs over UDP to get through middleboxes, and OpenFLAME hides its records in TXT so unmodified DNS servers can serve them.

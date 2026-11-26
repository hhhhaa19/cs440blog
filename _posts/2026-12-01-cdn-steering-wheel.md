---
layout: post
title: "Your Resolver Is Driving, and Nobody Told You"
date: 2026-12-01
paper_authors: "R. Kumar, F. E. Bustamante, M. Flores"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.23"
week: 12
tags: [cdn, dns, anycast, measurement]
---

## Key Idea

Every CDN request ends with a choice of which replica serves you. The paper argues that this choice is a hidden source of control: CDNs do not say how they make it, and when it is made through DNS, whoever runs your resolver effectively decides. The authors infer the mechanism from the outside using a latency fingerprint. A RIPE Atlas probe in each country resolves the same objects through five resolvers at growing distances (local, another metro, the same region, a neighboring region, a non-neighboring region) and pings the replica each one returns. The coefficient of regionalization (CRV) is the KS distance between the latency distribution from the local resolver and each farther one. DNS-based steering follows the resolver, so CRV is near 1. Anycast ignores the resolver, so CRV is near 0. Regional anycast stays flat within a region and jumps across regions. The fingerprint matches the known designs of Akamai, Cloudflare, and Edgio. Applied to the landing pages of the top 1,000 sites in 19 countries (about 66% of Internet users) and 17 CDNs, it finds DNS steering in 12 of them, carrying 41% of bytes worldwide and 82% in Ghana. Google is split: its own services use DNS while hosted content uses anycast. Ten CDNs honor ECS. The authors conclude that resolver consolidation (8.8.8.8, 1.1.1.1) quietly shifts replica selection, and with it resilience and sovereignty, to a few operators.

## Critique

I like the method. It needs no help from the CDN, it is cheap, and it reduces a messy question to one interpretable number. My concerns are mostly about how far the results stretch. First, each country gets a single probe and a single resolver per scope, justified by "CDN operator guidance" that selection is consistent within a country. That may hold for Belgium. It is a strong assumption for the US, India, or Brazil, and a probe hosted in a well-connected network could see something different from a mobile user. Second, there is no explicit classification threshold. Fastly at 0.2–0.4 is called anycast and Google at 0.52–0.85 is called mixed, and the mixed bucket ends up holding about 30% of resources. A smaller CDN with few PoPs could produce mid-range values just because its sites are far apart, so latency alone may confuse sparse deployment with a different mechanism. The paper names deployment density as a limitation, but it affects the classification itself. Third, the headline concern, consolidation, is argued but never measured. The study does not estimate how many clients in each country use public resolvers, and does not show a case where a resolver choice actually sent traffic to a worse or foreign replica. Fourth, the scope is narrower than the bytes argument suggests. Only landing pages are measured, while video, software updates, and logged-in content dominate traffic. The ECS test also uses just two spoofed /24s through Google Public DNS, and one example prefix, 203.0.113.0/24, is a documentation range that a CDN may not geolocate at all. Finally, the regional-anycast ground truth is Edgio, which has since stopped operating, so that validation step cannot be repeated.

## Connections

The background paper, Edge Fabric (Schlinker et al., SIGCOMM 2017), shows Facebook taking control of the egress side by overriding BGP so its own traffic avoids congested peers. This paper looks at the ingress side and finds that for most content the steering wheel is held by BGP (anycast) or by the client's resolver (DNS), not by the user and often not even by the network the user pays. Like my [HiDe post](/2026/11/24/hide/), it uses latency as a signal that cannot be faked to infer something operators do not disclose: there, a hijack; here, a steering policy. Its sovereignty argument also echoes [VaulTor](/2026/11/19/vaultor/). Both treat where traffic physically goes, and who decides that, as a property that encryption alone does not give you.

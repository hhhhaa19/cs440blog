---
layout: post
title: "Keeping the Headers When Dropping the Packets: Lossy RDMA Done Right"
date: 2026-10-27
paper_authors: "W. Li, X. Liu, Y. Zhang et al."
paper_venue: "SIGCOMM 2025"
paper_url: "https://doi.org/10.1145/3718958.3750480"
week: 7
tags: [rdma, data-centers, transport, reliability]
---

## Key Idea

RDMA has relied on PFC to make Ethernet lossless, but PFC brings head-of-line blocking, congestion spreading, and deadlocks. Current lossy RDMA NICs use a simplified selective repeat that still suffers from ECMP hash collisions and long retransmission timeouts, and breaks under packet spraying because it mistakes reordering for loss. DCP splits the network into a lossy data plane and a lossless control plane: when a queue is congested, the switch trims the payload and forwards only the header in a prioritized queue, so the NIC learns exactly which packet was lost and retransmits it without a timeout, while a per-packet memory address lets the receiver accept packets out of order. With a P4 switch and an FPGA NIC prototype, it improves performance by 2.1× over IRN and 1.6× over MP-RDMA in large-scale simulation.

## Critique

The design is elegant: one cheap guarantee in the switch, that headers are never lost, removes the need for timeouts, reordering buffers, and per-packet bitmaps, and the FPGA prototype needs only about 1–2% more resources than a Go-Back-N NIC. The weakness is deployment: DCP needs new switch behavior, a new RDMA header extension, and new NICs at the same time, and its main evidence comes from a small testbed (two switches, 100 Gbps links) and NS3 simulations, which the authors note understate end-host retransmission costs. I would also like to see how the "lossless" header queue behaves under extreme incast, where even trimmed headers can pile up.

## Connections

This is the opposite answer to [Week 5's congestion-signal post](/2026/10/15/no-signal-to-rule-them-all/): there, richer in-network signals added little on average, while here a very simple in-network signal, the trimmed header, is what makes the whole transport work. It also extends [Week 4's P-ECMP post](/2026/10/08/unlocking-ecmp-programmability/): P-ECMP makes ECMP paths controllable, while DCP sidesteps ECMP collisions by making packet spraying safe. Its 42% lower completion time for AI workloads ties it to [this week's DistTrain post](/2026/10/29/disttrain/) and to my FAST project, where incast during all-to-all is exactly the kind of congestion DCP is designed to recover from.

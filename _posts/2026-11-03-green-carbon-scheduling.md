---
layout: post
title: "Scheduling ML Jobs for When the Grid Is Green"
date: 2026-11-03
paper_authors: "K. Xu, D. Sun, H. Tian, J. Zhang, K. Chen"
paper_venue: "NSDI 2025"
paper_url: "https://www.usenix.org/conference/nsdi25/presentation/xu-kaiqiang"
week: 8
tags: [sustainability, ml-clusters, scheduling, carbon]
---

## Key Idea

ML cluster schedulers optimize job completion time but ignore that each job turns energy into training progress at a different rate, and that the grid's carbon intensity changes over the day. GREEN adds a carbon tracker and two optimizers on top of a multi-level feedback queue: one gives more GPUs to jobs that make more progress per unit of energy, and the other shifts high-power jobs into low-carbon hours while keeping each job's daily share of resources. Replaying 791 real jobs on a 32-node, 64-GPU testbed with carbon data from four regions, it cuts the cluster's carbon footprint by up to 41.2% and peak power by 12%, at a 3.6–5.9% cost in average JCT.

## Critique

The framing is convincing because it uses a property ML jobs already have: long training jobs are paused and resumed anyway, so moving them within a day costs little. The evaluation is also stronger than usual, replaying real user jobs rather than only a synthetic trace. But the testbed uses RTX 3090 GPUs and the savings depend on how much carbon intensity varies in a region, so the 41.2% headline will not transfer evenly to H100 clusters or to flat, already-clean grids. The approach also assumes jobs can be paused, which is hard for large synchronous training runs spanning thousands of GPUs, and it leaves out embodied carbon, which the authors acknowledge.

## Connections

Like [Week 7's DistTrain post](/2026/10/29/disttrain/), GREEN rejects one policy for every workload: DistTrain gives each model module its own resources, and GREEN treats each job's energy efficiency separately. The two optimize different goals, though: DistTrain maximizes throughput, while GREEN accepts a small JCT loss to save carbon. It also connects to my FAST project: time GPUs spend waiting on all-to-all communication still burns power, so faster communication is also a carbon saving.

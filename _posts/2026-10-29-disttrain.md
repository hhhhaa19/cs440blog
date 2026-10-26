---
layout: post
title: "One Plan Doesn't Fit All: Disaggregating Multimodal LLM Training"
date: 2026-10-29
paper_authors: "Z. Zhang, Y. Zhong, R. Ming et al."
paper_venue: "SIGCOMM 2025"
paper_url: "https://arxiv.org/abs/2408.04275"
week: 7
tags: [ml-training, gpu-clusters, parallelism, multimodal]
---

## Key Idea

Systems like Megatron-LM apply one parallelization plan to the whole model, but a multimodal LLM combines a vision encoder, an LLM backbone, and an image generator with very different compute needs, and its training samples vary widely in image and text size. This causes pipeline bubbles and stragglers. DistTrain gives each module its own GPU allocation and parallelism, chosen by solving an optimization problem, moves data preprocessing onto separate CPU nodes, and reorders samples and microbatches to balance load. On a production cluster it reaches 51.8–54.7% MFU and up to 2.2× Megatron-LM's throughput.

## Critique

The evaluation on a production cluster with over a thousand A800 GPUs makes the results credible, and the core insight, that modules with different costs should not share one plan, is simple and convincing. But the gains shrink as the backbone dominates: for the 72B model, throughput improves only about 1.3×, which suggests the benefit depends on how unbalanced the modules are. The paper also tests only image and text, and says little about the cost of the cross-module traffic its communication brokers add on the network.

## Connections

Like [Week 5's congestion-signal post](/2026/10/15/no-signal-to-rule-them-all/), this paper gets its gains by scheduling smarter at the endpoints rather than by asking more of the network. It also connects to [Week 4's P-ECMP post](/2026/10/08/unlocking-ecmp-programmability/): both argue that one uniform policy, whether ECMP hashing or a single parallelism plan, wastes resources when the workload is uneven. DistTrain's data heterogeneity also mirrors my course project on FAST, where uneven expert routing makes all-to-all traffic skewed and changes every batch.

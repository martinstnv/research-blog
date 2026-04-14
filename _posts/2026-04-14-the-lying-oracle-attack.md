---
layout: single
title: Delegated Refusal Oracle Attack
date: 2026-04-14
classes: wide
tags:
  - prompt injection
---

If you can extract information from an LLM using an oracle-style attack, but its outputs are tightly supervised, monitored, and heavily filtered, this technique is designed for exactly that scenario.

## TL;DR

If you’re dealing with extremely strict guardrails where even basic oracles like true/false or yes/no are detected and blocked, this simple trick can still work. For example, if the guardrails replace sensitive outputs with phrases like “I cannot do that” or “This information is confidential” you can repurpose those responses as your oracle signals. By crafting prompts that force the model into choosing between these allowed fallback phrases based on whether a guess is correct, you effectively encode binary feedback without triggering detection. Repeating this process lets you infer the target data step by step, even under heavy supervision, by turning the guardrails’ own masking behavior into a covert communication channel.
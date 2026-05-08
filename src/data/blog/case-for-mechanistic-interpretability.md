---
author: Rahul Kumar
pubDatetime: 2026-05-08T11:30:00Z
title: The case for mechanistic interpretability
slug: case-for-mechanistic-interpretability
featured: true
draft: false
tags:
  - interpretability
  - deep-learning
  - notes
description:
  We've built models that can write essays and prove theorems, and we mostly can't say why. Mechanistic interpretability is the small but growing field trying to fix that.
---

We've built systems that can write essays, prove theorems, and hold a conversation about almost anything. And yet, when you ask the simple question — *how does this thing actually work?* — the honest answer is mostly: we don't know.

That should bother us more than it does.

## Table of contents

## The black box problem

A modern transformer might have seventy billion parameters. Each is a number. Stack them in the right order and you get something that produces fluent English. Why? In what sense?

We can run experiments. We can perturb a weight and see what breaks. We can probe the network with carefully chosen inputs. But most of what's inside is fog. The network is doing *something* — and the something is a high-dimensional dance that no one can quite read.

Mechanistic interpretability (often shortened to "mech interp") is the field of people who think this is unacceptable, and have decided to do something about it. The plan is unfashionably ambitious: take a neural network, open it up, and figure out what it computes — not statistically, but circuit by circuit.

## A concrete win: induction heads

Here's a result I find delightful.

Early in a transformer's training, a specific kind of attention head shows up. It's called an **induction head**, and its job is dead simple. If it sees a pattern like `[A][B] ... [A]`, it predicts `[B]` next. That's it.

That sounds trivial. It isn't. This little circuit is one of the basic building blocks behind in-context learning — the strange ability of language models to pick up patterns from a prompt without any weight updates. And we can *see* it form during training. There's a moment in the loss curve where the model suddenly gets noticeably better, and the timing lines up with these heads coming online.

![Loss curve during training showing a phase transition where induction heads emerge](/posts/case-for-mechanistic-interpretability/induction-heads-emergence.svg)

That's not a black box. That's a story with a beginning and a middle.

## The weirder side: superposition

Here's the part that messes with my head.

You'd hope each neuron in a network corresponds to one clean concept — *this neuron fires for cats, that one fires for blue.* Reality is rarely so kind. Most neurons in a real model are **polysemantic**. A single neuron will fire for cats *and* the color blue *and* the letter Q. The network is packing in more features than it has neurons, and the features are sharing space.

Anthropic calls this phenomenon **superposition**, and it's the central reason mech interp is hard.[^toy] You can't just open a neuron and ask what it does — the *what* is smeared across many neurons, in patterns we have to learn to read.

![A 2D plane with six labelled feature directions; two of them ("cat" and "blue") nearly overlap, illustrating polysemanticity](/posts/case-for-mechanistic-interpretability/superposition.svg)

The good news is we have tools now — sparse autoencoders are the loudest example — that can pull these tangled features apart and let us look at concepts one at a time. Recent work has scaled this up to real production-scale models. Things that were illegible a year or two ago are starting to come into focus.

## Why this matters

A few reasons stack:

1. **Safety.** If we want to know whether a model is being deceptive, harboring goals we didn't intend, or hiding shortcuts that will fail in the wild, we need to look inside. Behavioral testing won't catch what we can't see.
2. **Science.** Networks that work are worth understanding. There's something genuinely deep about *why* gradient descent on this particular architecture produces the structures it does.
3. **Trust.** "It just works, trust me" doesn't scale. If a model is making medical or legal decisions, knowing *how* it gets there starts to feel non-optional.

## What I'm reading next

If you want to go deeper, the canonical entry points are:

- *[A Mathematical Framework for Transformer Circuits](https://transformer-circuits.pub/2021/framework/)* — Anthropic's paper that started a lot of this
- *[Toy Models of Superposition](https://transformer-circuits.pub/2022/toy_model/)* — accessible, beautifully visualized
- Neel Nanda's *[200 Concrete Open Problems in Mechanistic Interpretability](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj)* — if you want to actually contribute

I'll write more on this as I work through it. Mech interp is in a phase where genuinely interesting things are getting figured out every few months. Feels like a good time to be paying attention.

[^toy]: Elhage et al., *Toy Models of Superposition*, 2022.

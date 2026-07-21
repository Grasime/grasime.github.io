---
title: "SecureBank: Building a Vulnerable Banking API to Learn AppSec Properly"
date: 2026-07-21
categories: ["Writeups"]
tags: ["appsec", "python", "flask", "ci-cd"]
---

I wanted to actually learn AppSec by building something, not just reading about it — so I built [SecureBank](https://github.com/Grasime/vulnerablewebapp), a deliberately vulnerable Flask banking API. The twist: instead of just finding bugs in someone else's intentionally-broken app, I built the whole thing myself — the app, the vulnerabilities, and a real CI/CD security pipeline to try and catch them.

## Why a banking API

Money handling is a good stress test for security thinking. It forces you to deal with things like:

- Authentication and authorization done properly (or not)
- Race conditions when multiple requests touch the same balance
- Precision issues with how you store currency
- Access control — can User A see or touch User B's account?

I used **pence-based integer storage** for all monetary values instead of floats, specifically to avoid the classic floating-point rounding problems that show up in real financial software. Small decision, but the kind of thing that matters once you start thinking like someone building production fintech rather than a toy CRUD app.

## The pipeline

The part I'm actually proudest of isn't the vulnerabilities — it's the CI/CD pipeline built around them. Every commit runs through:

- **Bandit** — static analysis for common Python security issues
- **Semgrep** — pattern-based scanning for deeper logic and security anti-patterns
- **Trivy** — dependency and container vulnerability scanning

The idea was to build the app the way a real engineering team might, complete with the automated guardrails you'd expect in production — and then deliberately reintroduce vulnerabilities on a separate branch to see whether the pipeline actually caught them, or just looked good on paper.

## A concrete example: the IDOR

One of the vulnerabilities I reintroduced was a classic **IDOR** (Insecure Direct Object Reference) — an endpoint that let you access account or transaction data by ID without properly checking that the requesting user actually owned that resource. It's one of the most common real-world issues in APIs, and exactly the kind of bug that's easy to introduce quietly and easy to miss in review if you're not looking for it.

I'm saving the full technical breakdown — how it was introduced, how it could be exploited, and what the pipeline did (or didn't) catch — for a dedicated write-up. For now, the short version: it's a good reminder that authorization checks need to happen at the resource level, not just the authentication level. "Are you logged in" and "do you own this" are two different questions, and conflating them is how IDORs happen.

## What's next

This post is the overview. I'm planning a follow-up that goes step-by-step through the IDOR — how I found it, how I'd exploit it, and whether Bandit, Semgrep, or Trivy stood a chance of catching it automatically (spoiler: not all vulnerability classes are equally visible to static analysis).

If you want to poke around yourself in the meantime, [the repo's public](https://github.com/Grasime/vulnerablewebapp) — vulnerable branch included.

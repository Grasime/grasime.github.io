---
title: "SecureBank"
date: 2026-07-21
---

A deliberately vulnerable Flask banking API, built as a hands-on portfolio project to demonstrate both offensive and defensive security skills in a single codebase.

## What it includes

- A vulnerable branch containing intentionally introduced flaws (including IDOR issues and race conditions around money handling) for demonstration and testing
- A full CI/CD security pipeline using **Bandit**, **Semgrep**, and **Trivy** to catch vulnerabilities automatically on every commit
- JWT-based authentication and a SQLAlchemy ORM data layer
- Pence-based integer storage for monetary values, avoiding floating-point rounding issues common in financial applications
- Row-level locking to mitigate race conditions on concurrent transactions
- A `SECURITY.md` documenting the project's threat model and known vulnerable areas
- A full README covering setup and architecture

## Why I built it

I wanted a project that went beyond "spot the vulnerability" — something that also showed how to catch these issues automatically in a pipeline, the way a real engineering team would. Building both the vulnerable version and the CI/CD guardrails around it in the same repo let me practice thinking like both an attacker and a defender.

## Stack

Python, Flask, SQLAlchemy, GitHub Actions, Bandit, Semgrep, Trivy

[View on GitHub →](https://github.com/Grasime/)

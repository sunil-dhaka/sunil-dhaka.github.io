---
layout: post
title: "Test-Driven Agentic Software"
date: 2026-02-12
categories: ['ai']
tags: ['llm', 'ai', 'eval', 'testing']
description: "Agents can build complex software now. The bottleneck is proving it works. Tests written before the code -- classical and LLM-judged -- close that gap."
author: Sunil Dhaka
---

![Diagram showing the LLM-judged eval pipeline: Test Suite defines YAML cases and criteria, Agent runs via claude -p subprocess with skills and MCP tools, LLM Judge scores output against a multi-dimension rubric, and Results produce pass/fail scores with markdown reports. A server-side Runner orchestrates the flow and recomputes scores to prevent self-inflation.](/assets/images/llm-judged-evals.svg)

## The bottleneck shifted

Agents got good fast. Between late 2024 and now, they went from unreliable autocomplete to building non-trivial features across multiple files, running tools, and iterating on their own output. Generation is essentially solved for a wide class of problems.

What didn't change: someone still has to verify the output works. That someone is you. And reading through a 1,000-line diff an agent produced is slow, error-prone, and scales terribly. The bottleneck in software development is no longer writing code -- it's confirming the code is correct.

This is the core insight behind [verifiability](https://karpathy.bearblog.dev/verifiability/) as a framework: tasks where correctness can be checked automatically are the ones agents will eat first. And writing software happens to be one of those tasks -- if you have tests.

## Tests first, then agents

The idea is old. TDD has been around for decades. But it fits agents perfectly: write a test suite that defines what "correct" looks like, then let the agent loop until the tests pass.

This isn't theoretical. Emil Stenstrom built [JustHTML](https://github.com/EmilStenstrom/justhtml), a full Python HTML5 parser (~3,000 lines), by pointing coding agents at the [html5lib-tests](https://github.com/html5lib/html5lib-tests) suite -- 8,500+ language-independent test cases that define correct parsing behavior. The agent did the typing; Stenstrom did the thinking. The existing test suite *was* the spec.

Drew Breunig took this further with [whenwords](https://github.com/dbreunig/whenwords) -- a library that ships no code at all. Just a specification, test cases in YAML, and installation instructions. You paste the instructions into any agent and it generates a working implementation in whatever language you want, validated against the tests.

The pattern: a robust test suite turns an agent from an unreliable assistant into a brute-force problem solver that iterates until green.

## Classical tests and their limits

For deterministic outputs -- functions, parsers, API endpoints -- classical `pytest` assertions work perfectly. The human writes the tests (with agent help if needed), the agent writes the implementation, and CI confirms it all passes.

But agents increasingly produce output where multiple valid answers exist. SQL queries, data analysis, generated documents, code review comments. You can't assert against a single expected string. You need fuzzier evaluation.

## LLM-judged tests

This is where an LLM judge fits. Instead of `assert output == expected`, you define correctness criteria and have a model score against them.

The setup: YAML test cases with a question, correctness criteria, and acceptable variations. A runner spawns one agent subprocess per case. The agent produces output, then a judge (same or different model) scores it against the criteria. The runner recomputes scores server-side so the agent can't inflate its own grades.

We've been using this at work for evaluating agent-built skills against business requirements. The hard part is writing good criteria -- specific enough to catch real errors, flexible enough to accept valid variations. But once the suite exists, it compounds. Every improvement gets validated automatically. Every regression gets caught.

## Non-interactive mode for automation

Both Claude Code (`claude -p`) and Codex CLI (`codex --quiet`) offer non-interactive modes that make this practical to automate.

```bash
echo "Your eval prompt" | claude -p \
  --output-format json \
  --model sonnet
```

From Python, it's `subprocess.run`. Full tool access -- MCP servers, file reads, whatever the agent needs. You control the system prompt, restrict tools, and cap the budget. No API keys required beyond your existing subscription.

## The hard part is the test suite

Writing the tests is the real work. It forces you to think precisely about what "correct" means before any code exists. That clarity is the point -- it's the spec, the acceptance criteria, and the regression safety net in one artifact.

The higher up the automation ladder you go, the more this matters. If both code and tests come from the same agent, you have a circularity problem -- the agent can "cheat and assert true." External test suites, held out from the agent's context, are the fix. Think of them like validation sets in ML: data the model never trains on.

Classical tests for deterministic behavior. LLM-judged criteria for fuzzy output. Both written before the code. That's the setup that lets agents build with confidence.

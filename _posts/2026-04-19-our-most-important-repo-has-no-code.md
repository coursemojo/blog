---
layout: post
title: "Our Most Important Repo Has No Code"
date: 2026-04-20
author: Jamie Forrest
description: "Markdown and git are the foundation of AI-powered engineering. Here's how two old tools became the new primitives of agentic coding."
---

![A futuristic illustration showing human workers and glowing blue AI avatars collaborating in a high-tech workspace. They are gathered around a large central server pillar covered in floating digital panels that resemble markdown documents and code snippets, but with placeholder lines instead of readable text. Glowing data streams connect the workstations to the central pillar.]({{ '/assets/images/git_and_md.jpg' | relative_url }})

In the past 6 months, the software engineering world has undergone a shift unlike any I've witnessed in my career. At Coursemojo, AI coding agents have quickly become a daily part of how we build software, and not just for engineers. But as AI handles more of the implementation for us, the human work concentrates somewhere else entirely. It concentrates on thinking clearly, documenting decisions, and maintaining the context that makes building good software possible.

That work needs a home. And the medium, it turns out, matters a lot. As we have begun to adapt to this new paradigm, we are converging on two boring, old tools that have quietly become the foundation of how we work: markdown files and git. In a world of agentic coding, they've become primitives — atomic building blocks on top of which everything else gets built.

Our most important repository has no application code in it. It's a filesystem of markdown files: product requirements docs, technical specs, process frameworks, root-cause analyses, API schemas. It's the repo where we think, alongside our agentic helpers.

## Why Markdown and Git?

The argument for markdown is simple: it's the one format that both humans and LLMs can read and write fluently, with zero friction. No proprietary format. No rendering engine required. No tooling to install. A markdown file diffs cleanly in a pull request, renders nicely on GitHub, opens in any text editor, and, critically, can be dropped straight into an AI agent's context window without any conversion layer. It is simultaneously a document you'd hand to a colleague and a prompt you'd hand to an agent.

Why not just keep documentation in Google Docs or Confluence and copy it into an agent's context? Why change what already works?

When you copy a Google Doc into an agent's context, the agent works from a snapshot. The code it produces evolves; the doc sits unchanged in Google Drive. Within days, the doc no longer reflects reality. In theory, the agent could use the Google Docs API to update the document, but that's an extra integration layer on top of a format that wasn't designed for programmatic access. With markdown in git, the agent reads and writes to the same repo where the code lives. No round trip, no translation, no drift.

Markdown is also more efficient for LLMs to process. PDF and HTML files carry substantial formatting overhead — font definitions, page coordinates, CSS, rendering instructions — that consume tokens without adding meaning. Markdown's formatting syntax is minimal by design, which means more of the context window is spent on actual content. And when you copy-paste between formats, formatting often breaks in the round trip. Markdown *is* its own formatting — there's no lossy conversion step.

And then there's scale. One doc in Google Drive is fine. But a documentation system — where specs reference other specs, a style guide feeds into agent instruction files, a governance framework references the style guide — needs to be searchable and traversable as a unit. When an agent is building something, it can search the entire docs repo for relevant context: prior decisions, related specs, applicable conventions. When those same documents are scattered across Google Drive folders, Confluence spaces, and Notion pages, that search becomes fragmented or impossible.

That covers the format. The other half is where it lives.

The argument for git is different but complementary. Git gives you non-destructive history: every change is recorded, every decision is preserved, and you can always get back to where you were. In an agentic world, this matters more than it used to. When an AI agent is generating and modifying code at high velocity, the ability to see what changed, when, and why becomes essential — not just for rollback, but for understanding. Agents can read git history to understand why something is the way it is. Humans can review what agents did. Neither has to take the other's work on faith.

Git also gives you something that document tools don't: workflow semantics as a primitive. An open pull request *is* a proposal. Commits against review comments *are* the iteration. An approval *is* buy-in from the team. A merge *is* the transition from draft to published. The state of the document and the state of the decision are the same thing. Google Docs and Confluence can track changes and accept comments, but the workflow around a document — who has signed off, what's changed since they did, whether it's a draft or a decision — is something you have to manage manually or bolt on with external tools.

Together, markdown and git create a substrate that both humans and AI agents can operate on: reading, writing, reviewing, and reasoning without translation layers or context loss.

## What the Old World Looked Like

Before we recognized this pattern, our workflow looked like most engineering teams' workflows. Documentation lived in multiple places: some in the codebase (README files, inline comments), some in Confluence, some in Jira tickets, some in Google Docs, some in people's heads. These sources of truth drifted apart immediately, because keeping them in sync was an annoying manual chore that nobody had time for.

Design decisions were made in meetings and Slack threads, then partially captured in tickets. Technical specs, when they existed, were out of date almost as soon as they were approved — a rationalization of what we thought the implementation would look like, rather than what it ended up looking like. The IDE was the center of the engineering universe, and everything orbited around it.

This worked well enough when humans were the only ones writing code. But when AI agents entered the picture, the cracks became obvious. Agents could read Confluence, but the rich formatting of the documents often got in the way. There were a lot of round trips involving downloading PDFs, sending them to the agent, applying the agent's edits by hand in Confluence. They could read code, but code alone doesn't tell you why — why this feature, why this architecture, why this tradeoff, why this approach instead of three other reasonable ones.

We needed to externalize all of that thinking into a format agents could consume. And we needed to put it somewhere with version control, review workflows, and history. We already had both tools. We just hadn't recognized them yet as primitives.

## What This Looks Like in Practice

Three patterns have emerged at Coursemojo that illustrate how this works.

### The spec is the prompt

When we need to build something, we start by writing a technical spec (a markdown document in our documentation repo). The spec describes the problem, the proposed solution, the design decisions and their rationale, the API contracts, the edge cases, the rollout plan. It goes through review as a pull request: engineers comment on the approach, suggest alternatives, push back on assumptions. Once it's approved, it doesn't gather dust in a wiki somewhere. (We often use agentic coding tools for this step too, because the tools can read product requirements documents, our existing codebase, and other pieces of context that can help our engineers think through the technical solution.)

The spec then becomes the building prompt.

The same document that received design review from the engineering team is the document that gets handed to an AI coding agent as its instructions. One artifact serves three purposes: it's the design review, the historical record, and the agent's directive. And because it's in git, the history of how it evolved — what was proposed, what was challenged, what changed — is preserved alongside it.

This also means that design feedback happens before code exists, when it's cheap to change direction. We've all experienced the pull request where an engineer raises a fundamental architectural concern on a 2,000-line diff. That's an expensive place to discover a disagreement. Moving the substantive design discussion to the spec, which is a one-or-two-page markdown document, makes those conversations earlier, faster, and less painful.

### The style guide that governs (and is governed by) agents

Our engineering style guide is a markdown file in the documentation repo. It codifies our conventions: how we handle imports, logging, error handling, naming, architecture patterns. It draws from what the codebase already does, what the team has formally agreed on, and what we aspire to.

That style guide gets distilled into agent instruction files that live in each code repository. When an AI coding agent works in one of our repos, it reads those instructions and follows the conventions. The guide governs the agents.

But the loop can close in the other direction too. Because the guide, the instruction files, and the codebase are all machine-readable text in git, an automated agent can, in principle, crawl the codebase, detect where patterns have drifted from the guide, and propose updates. We haven't fully built that feedback loop yet, but the architecture makes it possible precisely because everything is markdown in git. If our conventions lived in Confluence and our agent instructions were hardcoded config files, this kind of self-correcting system would be much harder to imagine.

### A framework for non-engineers who build things

Here's where it gets interesting. AI coding tools have lowered the barrier to building software to the point where non-engineers across our company regularly build useful tools: data analysis scripts, workflow automations, internal utilities. This is a genuinely new capability, and it's powerful. But it creates a governance challenge: how do you maintain security, reliability, and code quality when the pool of builders suddenly expands beyond the engineering team?

Our answer is, you guessed it, a markdown file, in the same documentation repo, that describes how non-engineers should approach tool development. It covers when to involve engineering, how to think about what they've built (is it personal or shared? internal or external? interactive or automated?), and what the process looks like for graduating a personal script into shared infrastructure.

The beautiful thing is that the same AI coding agent that helps a non-engineer build a tool can also read the framework that governs how tools should be built. The governance document is in the same format and the same repo as everything else. It's not a policy living in an HR wiki that nobody checks. It's a markdown file that an agent can reference in the same session where it's writing code.

## The Job Changes

This shift has a corollary that's worth stating directly: when agents handle more of the implementation, the engineer's job doesn't disappear. It changes.

Engineers spend more time on the thinking that surrounds the code. Problem definition, project scoping, architectural judgment, security and privacy analysis, process design, and reviewing AI-generated work. All of that work is writing. Specs, RFCs, frameworks, review comments, root-cause analyses. The engineer's output increasingly is the documentation — not as a chore that follows the real work, but as the real work itself.

And because that writing lives in markdown in git, it's versionable, reviewable, and searchable. A new team member can read the git history of our documentation repo and understand not just what we decided, but how our thinking evolved. An agent can read the same history and carry that context into its work. That's something no amount of Slack archaeology or meeting recordings can replicate.

The other side of this coin is that non-engineers become participants in the same workflow. When a product manager writes a requirements doc as a pull request, engineers can review it with the same tools they use for code. Comments, suggestions, approvals — it's all the same interface. The shared medium lowers the barrier to collaboration across roles in a way that separate tools for separate functions never could.

## Where This Breaks

I don't want to oversell this. There are real limitations.

Git is notoriously hostile to non-technical users. AI coding agents abstract away a lot of the complexity (non-engineers at Coursemojo work with git through agents and rarely need to touch the command line, but the abstraction isn't perfect, and has caused friction). Merge conflicts, branching strategies, and the general mental model of distributed version control remain stumbling blocks.

Markdown can't do everything. It doesn't handle rich media well. You can't embed an interactive prototype in a markdown file. For some kinds of documents (charts, visual designs, data model diagrams), other formats are simply better.

And frankly, GitHub wasn't designed for this workflow. It was designed for code, and it works well for code. Markdown gets decent treatment. There's a preview renderer, and you can comment on prose diffs, but if you were building GitHub today with the assumption that half the pull requests would be prose documents governing how AI agents should behave, you'd probably design it differently. There's an opportunity for tooling that takes markdown-in-git as a first-class primitive and builds workflows around it specifically.

## The Bet

The tools will keep changing. The IDEs, the agents, the models — all of it will look different in a year. But some things feel durable.

Clear human thinking, captured in a format that both humans and machines can read, versioned so that history is preserved, reviewable so that decisions are transparent — that's not going away. If anything, it becomes more important as the pace of AI-generated output accelerates. The faster agents can build, the more it matters that someone thought carefully about what to build and why.

Markdown and git aren't glamorous. They've been around for decades. But they might be the most important tools in an AI-powered engineering organization, not despite their simplicity, but because of it.

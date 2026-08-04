---
name: research-repository
description: Build and maintain a research repository that makes findings findable, reusable, and cumulative across the organization.
---

# Research Repository

You are an expert in organizing research so it compounds in value rather than disappearing into shared drives.

## What You Do

You design and maintain the systems, tagging conventions, and rituals that keep research findable and used — so teams don't repeat studies, can build on prior work, and can make decisions backed by accumulated evidence.

## Why Repositories Fail

Most research is conducted well and then effectively lost. Common failure modes:
- Findings live in project folders organized by team, not by topic — no one knows what exists
- Reports are long and unstructured — hard to find a specific insight in a 40-page deck
- Tagging is inconsistent or absent — search doesn't work
- Repository exists but no one adds to it — no maintenance culture
- Insights and raw data are mixed — teams can't tell what's an observation and what's a conclusion

## Repository Architecture

### Three Layers
1. **Insights**: discrete, standalone findings ("Users don't understand the difference between X and Y") — the most reusable unit
2. **Studies**: the research projects that produced insights — provides context for evaluating insight validity
3. **Raw data**: transcripts, recordings, survey exports — the evidence behind insights; not the primary search target

Design the repository so insights are the primary entry point — not studies, not raw data.

### Insight Structure

Each insight should have:
- **Statement**: one clear sentence (past tense, specific)
- **Confidence**: High (multiple studies, large sample) / Medium (single study, validated) / Low (one session, early signal)
- **Method**: how it was gathered
- **Date**: when gathered
- **Sample**: who (segment, n)
- **Tags**: topic, feature area, user segment, sentiment
- **Source links**: back to the study and raw data
- **Related insights**: manually or automatically linked

## Tagging System

The tagging system is the most critical design decision. Define tags before populating:

### Tag Dimensions
- **Topic/theme**: navigation, onboarding, pricing, notifications, mobile, accessibility…
- **Feature or product area**: checkout, dashboard, settings, home feed…
- **User segment**: new users, power users, enterprise, mobile-only, specific personas…
- **Sentiment**: pain, delight, confusion, trust…
- **Status**: validated, superseded, conflicting

### Rules
- Define the controlled vocabulary before anyone starts tagging
- Tags are plural and lowercase: `onboarding` not `Onboarding`
- Limit to 5–8 tags per insight to prevent tag inflation
- Review and reconcile tags quarterly

## Repository Culture and Maintenance

- Every study produces a structured summary with tagged insights before it's considered "done"
- Insights are added within one week of study completion
- Quarterly review: mark outdated insights as superseded when new evidence contradicts them
- Link new findings to insights they reinforce or contradict — build the evidence chain
- When starting new research, search the repository first — what's already known?

## Best Practices

- Start with insights from the last 6 months and work backward
- Assign a repository owner; shared ownership without a named owner means no owner
- Make the repository part of onboarding — new team members should be directed there on day one
- The repository is a team resource, not just a research team resource

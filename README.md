# Swiss TIP

<p align="center">
  <img src="federal.png" alt="The Federal Palace in Bern drawn as circuit-board traces" width="720">
</p>

**A Trusted Information Platform for Swiss public information.**

Ask an assistant about a Swiss rule and it will usually produce something
plausible. Plausible is the problem. Swiss public information is spread over
federal, cantonal and municipal publishers in four languages, the rule that
applies depends on the canton and the commune, and a generic answer silently
mixes jurisdictions, quotes an outdated page or reproduces a neighbouring
country's rule.

Swiss TIP is being built to move that risk out of the model: an MCP (Model
Context Protocol) server that publishes a curated, versioned knowledge base
in which every fact is a short statement tied to an **exact excerpt of an
official page**, with the publisher, the URL, the access date and content
hashes, and confirmed by a person against that excerpt before release.

Four tools - `get_coverage`, `search`, `resolve` and `get_evidence` - will
let a calling assistant discover what is covered, find the concepts that fit
a question, resolve them for a stated place, date and situation, and read
the original excerpts. The assistant writes the answer; the server returns
facts, excerpts, citations and a typed status. And when a question falls
outside what is published, the server says so by name instead of guessing.

## What it must guarantee

- **Evidence first.** No fact without an excerpt. The excerpt travels with
  the fact, so any citation can be checked against the release itself or
  against the live page. A build fails when an excerpt no longer matches its
  source.
- **Explicit scope.** Jurisdiction, date and situation are part of a
  request. Missing context comes back as a named gap, never as a guess, and
  a rule for one canton is never served for another.
- **Honest limits.** Out-of-coverage and stale results carry a status the
  caller can act on. One correct "not covered" beats a plausible guess.
- **Reproducible and offline.** A release is sealed: one file, hashed end to
  end, that either verifies or does not. Serving it needs no model, no
  credentials and no network; semantic search is an optional local addition.

## How it works

```text
official page -> saved text -> quoted excerpt -> reviewed fact -> versioned release -> four MCP tools
```

Two separate programs stand on either side of that arrow, and the sealed
release is the only thing passing between them. The **knowledge builder**
turns source pages into a release and refuses to publish one until its
acceptance and readiness checks pass. The **MCP server** answers from a
release and does nothing else: it never crawls, never builds and never
writes. Neither imports the other, so serving needs no build tooling, no
model, no credentials and no network, and whoever builds a release need not
be whoever runs it.

Code and data are published separately, which keeps the two release cycles
apart. The code is released when the server changes; a pack is released when
its facts are re-checked against their sources, which happens far more often
and needs a reviewer rather than a developer. The server is knowledge-base
agnostic and serves any pack in the release format.

## Rules, not live data

Several Swiss MCP servers already put live sources in front of an assistant:
timetables and departures, open government datasets, current population and
other statistics, the full text of federal law. They answer *what is the
value right now* by querying, at request time, the system that owns the
answer. For data that changes by the hour, that is the right design, and
Swiss TIP does not compete with it.

Swiss TIP takes on the other half of the question: rules rather than
readings. A rule is prose, not a value. It is spread across several
publishers' pages, it reads differently depending on canton, commune and
situation, it has no endpoint to query, and nothing will tell an assistant
that it has just got it wrong. So instead of querying a source at request
time, Swiss TIP quotes it beforehand, has the quote confirmed by a person,
and serves that confirmed statement with its evidence, its scope and its
age.

The two kinds of server complement each other, and a client can hold both -
one for today's number, one for the rule that says what the number means.

## A platform, not one knowledge base

Nothing in the server or the release format is tied to a subject area, or to
public information at all. A pack is any body of statements whose evidence
can be quoted from source documents and confirmed by someone accountable for
it: a regulator's guidance, an insurer's product rules, a company's internal
policies, a supplier handbook. The guarantees then apply unchanged -
excerpts, citations, applicability by place and date, a named reviewer, a
sealed release, and a plain "not covered" outside the pack.

Nor does a pack have to be public. A sealed release is one file, and serving
it needs no network, so a knowledge base can stay inside an organisation,
ship with a product, or be licensed to subscribers, while the server that
reads it stays the same. The MVP will start with Swiss public information
because that is where the cost of a plausible answer is easiest to show.

## The hackathon

Swiss TIP is being built for the **Swiss {ai} Weeks** hackathon in Zurich,
24 and 25 September 2026, for the challenge **Swiss Grounding MCP**, set by
Swisscom's myAI team:
<https://zh.ai-weeks.ch/challenges/swiss-grounding-mcp>

## Licence

Apache License 2.0. Quoted official texts remain the property of their
publishers and are reproduced only as cited evidence.

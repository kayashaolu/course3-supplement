# Challenge 3 Part 2: Technical Design Document - Collaboration Expansion

**Student Name**: [Your Name]
**Submission Date**: [Date]
**Challenge**: TeamFlow Team Collaboration Platform - Part 2 Collaboration Expansion

---

## Context - what happened

Your Part 1 platform launched and grew into tens of thousands of teams. Customers stopped describing TeamFlow as a chat app and started calling it their collaboration platform. The new requests are not about more messaging - they are about everything that surrounds messaging.

Teams want to share files in conversations with inline previews. They want to edit documents together inside a channel without leaving for another tool. They want to find old messages by keyword, not just scroll. And legal wants retention policies that move old content out of the hot path.

This document is your **evolution** of the Part 1 design, not a redesign. Part 2 should clearly build on the architecture you submitted for Part 1, adding components and modifying connections to address four new requirements. The Part 1 messaging core stays intact.

## IMPORTANT: Technology-Agnostic Design Required

Use building block names, not technologies. See the Part 1 template for the full list. Named techniques and concepts (indexing strategies, snapshotting, convergence algorithms, archive tiers) are **patterns** - they describe what happens inside building blocks, not new primitives. Name the pattern; do not name a vendor.

## Flow notation: how to write flows

Every flow in this document uses building-block notation. Here is the format, illustrated with a system this course does not cover - a city library's book-reservation system:

```
Reserve a book: User → Reservation Service → Relational Database (availability check) → Queue → Notification Worker → External Service (SMS)
Browse the catalog: User → Catalog Service + Key-Value Store → Relational Database (on cache miss)
```

Your flows should look like this in notation - the architecture is yours to design.

---

## Part 1 Architecture Recap

[Briefly summarize your Part 1 architecture in 2-3 sentences. Name the major components and how they connect. This sets the baseline for what you are evolving.]

---

## Requirement 6: File Sharing with Inline Previews

*Users drag files into a conversation. Images, PDFs, and short clips appear inline with a generated preview. Uploading must not block the user's send, and previews can appear a moment after the file lands.*

### User Flow Design

**Your file-sharing flows:**
[Write 3-5 specific flows covering how a file gets uploaded, where its bytes and its metadata live, how the preview gets generated, and how the inline preview reaches the conversation when it is ready]

### Building Blocks Added

- **[Block 1]**: [What it does in this requirement and why this block]
- **[Block 2]**: [Same]
- **[Block 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [Where do file bytes live versus file metadata, and why split it that way (or not)?]
- **[Decision 2]**: [How does the conversation find out that the preview is ready?]
- **[Decision 3]**: [What happens when preview generation fails? Does the file still show up?]

---

## Requirement 7: Collaborative Document Editing

*Inside any channel, users can open a shared document and edit it together. Multiple people can type at once. Edits propagate to everyone with the doc open within a heartbeat. The doc has a durable version that survives every editor disconnecting.*

### User Flow Design

**Your collaborative-editing flows:**
[Write 3-5 specific flows showing how a user opens a doc, how concurrent edits from multiple users are handled, how edits reach other open editors, and how the durable state is kept up to date]

### Building Blocks Added

- **[Block 1]**: [What it does in this requirement and why this block]
- **[Block 2]**: [Same]
- **[Block 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [How do concurrent edits from multiple users converge to the same document? Name the concept-level approach you chose and justify it.]
- **[Decision 2]**: [What sits between a keystroke and durable storage, and why?]
- **[Decision 3]**: [How does a user joining mid-session catch up to the current state efficiently?]

---

## Requirement 8: Full-Text Search Across Message History

*Users search messages by keyword and get sub-second results across years of history. The search index must stay current as new messages arrive, without slowing the live send path.*

### User Flow Design

**Your search flows:**
[Write 3-5 specific flows covering how new messages become searchable and how a query gets answered. Include what happens on a message edit.]

### Building Blocks Added

- **[Block 1]**: [What it does in this requirement and why this block]
- **[Block 2]**: [Same]
- **[Block 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [Which block holds the searchable representation of messages, and why that block over the alternatives?]
- **[Decision 2]**: [How fresh are search results relative to the live message stream, and how does your design keep the send path unaffected?]
- **[Decision 3]**: [How does a search hit become a full result the user can see, and where do permissions get checked?]

---

## Requirement 9: Time-Based Retention and Archive Policies

*Teams configure retention windows. After the configured window, messages and files move to a cold archive or get deleted, depending on team policy. Retention runs continuously without operator intervention.*

### User Flow Design

**Your retention flows:**
[Write 3-5 specific flows showing what triggers retention work, how per-team policies get applied, and how data actually moves or gets deleted]

### Building Blocks Added

- **[Block or entity 1]**: [What it does in this requirement and why]
- **[Block or entity 2]**: [Same]
- **[Block or entity 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [What triggers retention work, and why that trigger rather than the alternatives?]
- **[Decision 2]**: [Where does the actual delete/archive work run, and why there rather than on a live path?]
- **[Decision 3]**: [What happens when a user searches for content that has been archived or deleted?]

---

## Foundation Preserved

Walk through your Part 1 paths and confirm they are intact. The grader looks for the Part 1 core surviving under the Part 2 additions.

- **[Part 1 path 1]**: [Still in place? What, if anything, changed?]
- **[Part 1 path 2]**: [Same]
- **[Part 1 path 3]**: [Same]
- **[Part 1 path 4]**: [Same]

---

## Cross-Cutting Trade-offs

A strong Part 2 names where the new requirements collide with the Part 1 design. Identify at least three tensions between the new capabilities and the existing messaging core, and state how your architecture resolves each:

**[Tension 1]**: [What pulls in each direction, where you landed, and why]

**[Tension 2]**: [Same]

**[Tension 3]**: [Same]

---

## Failure Mode Analysis

Name what still breaks and how the system degrades:

- **[Scenario 1]**: [What breaks and how does the architecture handle it?]
- **[Scenario 2]**: [Same]
- **[Scenario 3]**: [Same]
- **[Scenario 4]**: [Same]

---

## Trade-offs Explicitly Accepted

- **[Trade-off 1]**: [What you gave up to add these capabilities]
- **[Trade-off 2]**: [What you gave up to add these capabilities]
- **[Trade-off 3]**: [What you gave up to add these capabilities]

---

## What This Evolution Intentionally Does NOT Address

[Anything you are deferring to Part 3 - explicitly. The grader rewards designs that know their boundaries.]

---

## Submission

Save this document as markdown and paste the full content into the **Challenge Part 2** submission form at [systemthinkinglab.ai](https://systemthinkinglab.ai/protected/course3/challenge2.html). Part 1 must be graded before Part 2 can be submitted.

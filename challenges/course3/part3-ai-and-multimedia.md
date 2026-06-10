# Challenge 3 Part 3: Technical Design Document - AI and Multimedia

**Student Name**: [Your Name]
**Submission Date**: [Date]
**Challenge**: TeamFlow Team Collaboration Platform - Part 3 AI and Multimedia

---

## Context - what users are asking for

Your Part 2 platform serves hundreds of thousands of teams. File sharing, collaborative documents, search, and retention are all in production. The next wave of requests is reshaping what a collaboration platform is:

> *"I need to find the message where we agreed on the launch date, but I do not remember the exact words."*
> *"Can you summarize what happened in this channel while I was on vacation?"*
> *"We do video calls in other tools and copy the notes back here - can we just stay in TeamFlow?"*

This document is the **final evolution** - layering AI capabilities and live video on top of your Part 2 architecture. The patterns from Parts 1 and 2 stay intact. AI is added, not bolted on.

## IMPORTANT: Classification Matters Here

Part of the grade is classifying third-party AI and media capabilities into the correct building blocks and external entities. Apply the classification rules from the lessons carefully - a capability described with the wrong block costs points.

One naming note: WebRTC is allowed as the name of an open *protocol*. It is not a vendor name, and it is not a building block - do not name a Service "WebRTC", a Queue "WebRTC", or storage "WebRTC".

## Flow notation: how to write flows

Every flow in this document uses building-block notation. Here is the format, illustrated with a system this course does not cover - a city library's book-reservation system:

```
Reserve a book: User → Reservation Service → Relational Database (availability check) → Queue → Notification Worker → External Service (SMS)
Browse the catalog: User → Catalog Service + Key-Value Store → Relational Database (on cache miss)
```

Your flows should look like this in notation - the architecture is yours to design.

---

## Part 1 + Part 2 Architecture Recap

[Briefly summarize the architecture after Part 2 (2-3 sentences). Name the major components and how they connect. This is the foundation that survives.]

---

## Requirement 10: Semantic Search Across Messages and Documents

*Users describe what they remember, not what was written. "The post about hiring the new designer" should find the message that announced the hire, even if it never said "designer". Documents are searchable the same way.*

### User Flow Design

**Your semantic search flows:**
[Write 3-5 specific flows showing how messages and documents become findable by meaning, how a query gets answered, and how semantic results relate to the keyword search from Part 2]

### Building Blocks Added

- **[Block or entity 1]**: [What it stores or does, and why this block is the right classification]
- **[Block or entity 2]**: [Same]
- **[Block or entity 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [What you chose to build versus consume for this capability, and why]
- **[Decision 2]**: [How do semantic results and keyword results relate in your design? Why?]
- **[Decision 3]**: [How does this capability stay consistent with deletes from the Part 2 retention pipeline?]

---

## Requirement 11: AI Assistant in Every Channel

*Inside any channel, users can ask the assistant questions: "summarize today", "what action items came out of this thread", "what did we decide about the Q3 roadmap". The assistant grounds its answers in the actual channel content, not in generic web knowledge.*

### User Flow Design

**Your assistant flows:**
[Write 3-5 specific flows showing how a user's question becomes an answer grounded in the channel's actual content]

### Building Blocks Added

- **[Block or entity 1]**: [What it does, and why this block is the right classification]
- **[Block or entity 2]**: [Same]
- **[Block or entity 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [How does the assistant get the right channel content into its answer, and why that approach over the alternatives?]
- **[Decision 2]**: [How does the system handle the AI capability being unavailable or rate-limited? Does the channel degrade gracefully?]
- **[Decision 3]**: [Where do you check permissions? An assistant must not surface content from messages the asking user cannot see.]

---

## Requirement 12: Video Calls with Recording and Transcription

*Inside any channel, users start a video call. Calls can be recorded. Recordings are transcribed automatically so the spoken content is searchable like text messages. Transcripts are available to the assistant for channel summaries.*

### User Flow Design

**Your video and transcription flows:**
[Write 3-5 specific flows showing how a call is set up, how the recording is captured and stored, how the transcription happens, and how the transcript becomes searchable]

### Building Blocks Added

- **[Block or entity 1]**: [What it does, and why this block is the right classification]
- **[Block or entity 2]**: [Same]
- **[Block or entity 3]**: [Same]

### Architecture Decisions & Trade-offs

- **[Decision 1]**: [What carries the actual audio and video, and why did you classify it the way you did?]
- **[Decision 2]**: [When and how does transcription happen relative to the call, and why?]
- **[Decision 3]**: [How do transcripts become findable through the search capabilities your design already has?]

---

## Cross-Cutting Trade-offs

A strong Part 3 names the hard trade-offs explicitly. Identify at least three tensions that adding AI and live video to a collaboration platform creates, and state how your architecture lands on each:

**[Tension 1]**: [What pulls in each direction, the choice you made, and the mechanism that implements it]

**[Tension 2]**: [Same]

**[Tension 3]**: [Same]

---

## Graceful Degradation: Designing for AI Failure

AI and media services fail. TeamFlow cannot go down when AI does.

For each AI and video capability, define the fallback:

| Capability | Primary path | Fallback when unavailable |
|---|---|---|
| Semantic search | [Your primary path] | [Your fallback] |
| Channel summary | [Your primary path] | [Your fallback] |
| Channel Q&A | [Your primary path] | [Your fallback] |
| Video call | [Your primary path] | [Your fallback] |
| Transcription | [Your primary path] | [Your fallback] |

**Architectural principle**: [State explicitly what your design guarantees about messaging when AI and video capabilities are unavailable.]

---

## Foundation Preserved

Walk through your Parts 1 and 2 paths and confirm they survive:

- **[Part 1/2 path 1]**: [Still in place? What, if anything, changed?]
- **[Part 1/2 path 2]**: [Same]
- **[Part 1/2 path 3]**: [Same]
- **[Part 1/2 path 4]**: [Same]

---

## Complete End-to-End Architecture

Provide a complete architecture diagram (or detailed text description) showing:

1. All Part 1 components (still present)
2. All Part 2 additions (still present)
3. All Part 3 additions (new)
4. The connections between them

[Include diagram or detailed text walkthrough]

---

## Trade-offs Explicitly Accepted

- **[Trade-off 1]**: [What you gave up to add AI and video]
- **[Trade-off 2]**: [What you gave up to add AI and video]
- **[Trade-off 3]**: [What you gave up to add AI and video]

---

## What This Architecture Intentionally Does NOT Address

[Be honest about what is out of scope. The grader rewards designs that know their boundaries.]

---

## Submission

Save this document as markdown and paste the full content into the **Challenge Part 3** submission form at [systemthinkinglab.ai](https://systemthinkinglab.ai/protected/course3/challenge3.html). Parts 1 and 2 must be graded before Part 3 can be submitted. This is the capstone of Course 3 - make it count.

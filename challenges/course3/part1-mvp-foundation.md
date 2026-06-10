# Challenge 3 Part 1: Technical Design Document - MVP Foundation

**Student Name**: [Your Name]
**Submission Date**: [Date]
**Challenge**: TeamFlow Team Collaboration Platform - Part 1 MVP Foundation

---

## IMPORTANT: Technology-Agnostic Design Required

This technical design document must focus on **building blocks and architectural patterns**, not specific technologies.

**Use:**
- Building block names: Service, Worker, Queue, Key-Value Store, File Store, Relational Database, Vector Database
- External entities: User, External Service, Time
- Technology-agnostic terms that describe patterns (e.g., cache, acknowledgement, presence state, push gateway, async processing)

**Do NOT use:**
- Specific technologies: Redis, Memcached, Kafka, RabbitMQ, NATS, PostgreSQL, MongoDB, DynamoDB, Pusher, Ably
- Vendor names: AWS, Google Cloud, Azure, Firebase, Twilio, SendGrid, APNs as a branded product
- Programming languages or frameworks: Node.js, Express, Django, Socket.io, SignalR, Phoenix Channels

The grader will look for pattern recognition and clear reasoning, not technology brand-recall. Senior engineers think in patterns that transcend specific technologies.

## Recommended approach

1. **Draw your architecture diagram** using the 7 building blocks + 3 external entities. Use [this Google Drawing template](https://docs.google.com/drawings/d/1hbx9r8NCBNjMDZv9tAXzfvLR3-XPsOgHm9zrX0h_cO8/edit?usp=sharing) to get started.
2. **Use your diagram as reference** while writing your user flows and technical explanations.
3. **Ensure consistency** between what you draw and what you write.

## Flow notation: how to write flows

Every flow in this document uses building-block notation. Here is the format, illustrated with a system this course does not cover — a city library's book-reservation system:

```
Reserve a book: User → Reservation Service → Relational Database (availability check) → Queue → Notification Worker → External Service (SMS)
Browse the catalog: User → Catalog Service + Key-Value Store → Relational Database (on cache miss)
```

- Use EXACT building block names
- Use `+` for combinations (e.g., Queue + Worker)
- Start each flow with the external entity that triggers it
- Annotate a step's purpose in parentheses when it is not obvious

Your flows should look like this in notation — the architecture is yours to design.

---

## Scenario

TeamFlow is a new team collaboration platform launching its MVP. Teams need to message each other in direct conversations and group channels, see who is online, get notified of messages they missed, and trust that messages arrive in the right order. You are designing the MVP architecture that has to support real-time messaging without breaking when a user has a flaky connection or has the app closed on their phone.

---

## Architecture Overview

**High-Level Description**:
[Provide a 2-3 sentence overview of your overall architecture approach for the MVP.]

**Core Building Blocks Used** (check all that apply):
- [ ] Service (Blue Rectangle)
- [ ] Worker (Blue Trapezoid)
- [ ] Key-Value Store (Pink Diamond)
- [ ] File Store (Pink Pentagon)
- [ ] Queue (Pink Stacked Rectangles)
- [ ] Relational Database (Pink Cylinder)
- [ ] Vector Database (Pink Cube)
- [ ] User (Green Smiley)
- [ ] External Service (Green Cloud)
- [ ] Time (Green Hourglass)

---

## Requirement 1: Direct and Group Messaging with Low Latency

*Users send direct messages and post in group channels. Messages must arrive at every active recipient within a perceptible heartbeat, with no full page reload, no manual refresh.*

### User Flow Design

**Your messaging flows:**
[Write 3-5 specific flows for direct messages, group messages, and how a recipient who is online receives a message in real time]

### Architecture Decisions & Trade-offs

**Key architectural decisions:**
- **[Decision 1]**: [How does one sent message reach every recipient, and why did you structure that path the way you did?]
- **[Decision 2]**: [How does the system know where each recipient can be reached?]
- **[Decision 3]**: [Where in the flow does the message become durable, and why there?]

### Technical Implementation Details

**Live connection ownership**: [Which component holds the user's open connection? What happens when a user moves between two app instances?]

**Delivery pattern**: [How does one send become N delivered messages? Where does the work happen?]

**Persistence boundary**: [At what point in the flow is the message durably stored? What happens if a component on the delivery path is down?]

---

## Requirement 2: Message Ordering and Delivery Confirmation

*Inside a conversation, messages must appear in the order they were sent, on every device. Senders need to see when their message was delivered to the recipient's app.*

### User Flow Design

**Your ordering and confirmation flows:**
[Write 2-4 specific flows showing how ordering is established and preserved end to end, and how delivery confirmations travel back to the sender]

### Architecture Decisions & Trade-offs

**Key architectural decisions:**
- **[Decision 1]**: [Where does ordering come from in your design, and why is that the right place to establish it?]
- **[Decision 2]**: [Where does delivery-confirmation state live, and why?]
- **[Decision 3]**: [What happens to ordering when the recipient is offline and messages are delivered later?]

### Technical Implementation Details

**Ordering mechanism**: [Name the concrete pattern your design uses to keep messages in order]

**Acknowledgement flow**: [What does a delivery confirmation look like end-to-end? Where is it persisted?]

**Out-of-order handling**: [What does the client do if two messages arrive in the wrong sequence?]

---

## Requirement 3: Presence Indicators Across Devices

*Users see whether their teammates are online, offline, or actively typing. A user signed in on their laptop and phone counts as online if either device is connected.*

### User Flow Design

**Your presence flows:**
[Write 2-4 specific flows for connection events, presence queries, and the typing indicator across multiple devices]

### Architecture Decisions & Trade-offs

**Key architectural decisions:**
- **[Decision 1]**: [Which storage block holds presence, and what characteristics of presence data drove that choice?]
- **[Decision 2]**: [How is multi-device presence aggregated into a single answer?]
- **[Decision 3]**: [How does typing state stop showing after a user closes the tab without explicitly going offline?]

### Technical Implementation Details

**Presence data shape**: [What does a presence entry look like?]

**Multi-device aggregation**: [How do reads combine multiple device entries into a single "Alex is online" answer?]

**Typing-state lifecycle**: [How does typing state appear, refresh while the user keeps typing, and disappear?]

---

## Requirement 4: Cache Active Conversation Lists

*Every time a user opens the app, the home screen shows their list of active conversations with last-message previews and unread counts. This list is hit on every app open. Hitting the Relational Database every time would not survive even moderate growth.*

### User Flow Design

**Your conversation-list flows:**
[Write 2-4 specific flows showing how the list is served fast, what happens when the fast path cannot answer, and how the list stays current as new messages arrive]

### Architecture Decisions & Trade-offs

**Key architectural decisions:**
- **[Decision 1]**: [What caching pattern governs this list, and why that one?]
- **[Decision 2]**: [How does the cached list stay correct as new messages arrive?]
- **[Decision 3]**: [What exactly is in the cached value?]

### Technical Implementation Details

**Cache keys and values**: [What is the key? What is the shape of the value? How big is the value per user?]

**Freshness strategy**: [When does the cached list get updated, and by what?]

**Cold-start cost**: [What does the first read after a miss look like in terms of load on the system of record?]

---

## Requirement 5: Notification Delivery for Missed Messages

*If the recipient is offline, asleep, or has the app closed, they need a push notification on their phone within seconds of the message being sent. The notification path must not block the live send path: a slow push gateway must not slow down delivery to online recipients.*

### User Flow Design

**Your notification flows:**
[Write 2-4 specific flows showing how an offline recipient gets detected, how the notification work is handed off, and how the push gateway gets called]

### Architecture Decisions & Trade-offs

**Key architectural decisions:**
- **[Decision 1]**: [How does your design keep a slow push gateway from slowing the live send path?]
- **[Decision 2]**: [How do you detect "this recipient was offline" at send time?]
- **[Decision 3]**: [What happens when the push gateway is rate-limiting you or returning errors? How are notifications not lost?]

### Technical Implementation Details

**Detection trigger**: [What signal tells the system "this needs a push notification"?]

**Retry behavior**: [How does notification work get retried on failure, and what is the policy?]

**Gateway contract**: [What does the interaction with the push gateway look like? Token in, delivery acknowledgement out?]

---

## Overall Architecture Analysis

### Key design decisions (whole-system level)

1. **[Decision 1]**: [Rationale]
2. **[Decision 2]**: [Rationale]
3. **[Decision 3]**: [Rationale]

### Building block combinations used

- **[Pattern 1]**: [Which building blocks combined, where, and why]
- **[Pattern 2]**: [Which building blocks combined, where, and why]
- **[Pattern 3]**: [Which building blocks combined, where, and why]

### Trade-offs explicitly accepted

- **[Trade-off 1]**: [What you gave up and what you gained]
- **[Trade-off 2]**: [What you gave up and what you gained]

### What this MVP intentionally does NOT address

[Anything you are deferring to Part 2 or Part 3 - be explicit about what is out of scope. The grader rewards designs that know their boundaries.]

---

## Submission

Save this document as markdown and paste the full content into the **Challenge Part 1** submission form at [systemthinkinglab.ai](https://systemthinkinglab.ai/protected/course3/challenge1.html). You will receive AI-graded feedback within 24 hours.

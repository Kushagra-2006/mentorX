# MentorX — Master Instructions for Claude

## 1. PROJECT IDENTITY

You are the primary AI coding agent for MentorX.

MentorX is an AI-powered professional mentorship marketplace where access to expert time is scarce and valuable.

The core idea:

- Mentors provide limited mentorship sessions.
- AI analyzes a mentor's professional evidence and recommends a starting price.
- Students discover mentors using natural-language AI search.
- Mentorship sessions can be sold through time-limited auctions.
- Students bid for scarce mentor slots.
- The winning student funds the session using USDC.
- USDC is held in blockchain escrow.
- After the mentorship session is completed, payment is released to the mentor.
- Mentor reputation and session history are recorded and surfaced to users.

Core product statement:

> "MentorX — The market for expert time."

Important product principle:

> AWS provides the AI and application infrastructure.
> Arc provides blockchain-based money movement, escrow, and settlement.
> The Graph provides blockchain indexing.
> Privy provides wallet functionality.
> MentorX connects all of these into one user experience.

---

# 2. YOUR PRIMARY GOAL

Your job is NOT simply to write code.

Your job is to build a working, secure, maintainable MentorX product while preserving the architecture and product decisions defined in this file.

Before making significant changes:

1. Understand the existing code.
2. Understand the relevant README/documentation.
3. Check whether the functionality already exists.
4. Plan the smallest correct change.
5. Implement it.
6. Test it.
7. Run type checking/linting where applicable.
8. Verify that existing functionality still works.

Do not rewrite large portions of the application unless there is a clear technical reason.

Prefer incremental improvements over unnecessary rewrites.

---

# 3. SOURCE OF TRUTH HIERARCHY

When deciding how MentorX should behave or be implemented, use this priority:

1. Explicit user instruction in the current task
2. `CLAUDE.md`
3. Relevant documentation in `/docs`
4. Relevant module README
5. Existing implementation
6. Your own assumptions

If the existing code conflicts with the architecture described here, do not silently redesign the system.

Explain the conflict and choose the smallest change necessary unless the user explicitly asks for an architectural change.

---

# 4. CORE ARCHITECTURE

MentorX uses the following architecture:

```text
                         MENTORX
                            |
                +-----------+-----------+
                |                       |
             WEB APP                BLOCKCHAIN
                |                       |
          Next.js / React             Privy
                |                       |
          API Gateway                  Arc
                |                       |
             Lambda              Smart Contracts
                |                       |
        +-------+-------+         USDC / Escrow
        |       |       |
   DynamoDB    S3    Bedrock
                    +
                  Strands
                        |
                  AI Intelligence
                        |
              The Graph / Indexing
# MentorX — Claude Development Instructions

## IMPORTANT

Before making any changes to MentorX, Claude MUST read:

frontend/README.md

The frontend/README.md file is the complete source of truth for the MentorX frontend product, user flows, pages, UI, features, auction experience, AI experience, wallet experience, Google Meet scheduling, payment flow, and review/reputation flow.

## Core Product

MentorX is an AI-powered professional mentorship marketplace.

The core journey is:

Student registers
→ Student selects professional/job domain
→ AI recommends mentors
→ Mentor registers
→ Mentor uploads professional evidence
→ AI analyzes mentor expertise
→ AI recommends hourly starting price
→ Mentor creates a limited mentorship session
→ Session becomes an auction
→ Students bid
→ Market determines final price
→ Winner is selected
→ USDC is secured in Arc escrow
→ Private Google Meet is scheduled
→ Student and mentor receive email
→ Session happens
→ Student confirms completion
→ USDC is released to mentor
→ Student reviews mentor
→ Mentor reputation updates

## Architecture

Use the existing project architecture whenever possible.

Expected high-level architecture:

Frontend:
Next.js + React + TypeScript + Tailwind

Backend:
AWS API Gateway
AWS Lambda
DynamoDB
S3

AI:
Amazon Bedrock
Strands Agents

Blockchain:
Arc
Solidity smart contracts
USDC

Wallet:
Privy

Blockchain indexing:
The Graph

Meeting:
Google Calendar / Google Meet

Email:
Backend email service

## Important Architecture Rule

AWS handles application and AI infrastructure.

Arc handles blockchain transactions, USDC, escrow and settlement.

The Graph handles blockchain indexing.

Privy handles wallet/authentication.

Do NOT replace this architecture with Supabase or another unrelated backend unless explicitly instructed by the user.

## Frontend Rule

Before implementing frontend functionality:

1. Read frontend/README.md.
2. Inspect the existing frontend.
3. Reuse existing components.
4. Do not create duplicate components.
5. Do not add unnecessary features.
6. Prioritize the P0 end-to-end journey.
7. Keep the UI professional and polished.
8. Use strong TypeScript types.
9. Do not use `any` unnecessarily.

## Financial Rule

The frontend is NEVER authoritative for:

- Auction winners
- Auction end
- Bid validity
- Escrow status
- Payment release
- Blockchain confirmation
- Settlement

These must come from the backend/blockchain.

Never fake:

- Blockchain confirmations
- Transaction hashes
- Escrow
- Payment release
- Google Meet creation
- Email delivery
- AI results

If an integration is not implemented yet, create a clean typed service boundary instead of pretending it is real.

## Security

Never expose:

- Private keys
- Seed phrases
- AWS credentials
- Database credentials
- Backend secrets
- Private API keys
- Google service-account secrets

Never expose private CVs or private Google Meet URLs publicly.

Never store private CV contents onchain.

## Money

Always display money as:

$40 USDC
$60 USDC

Clearly distinguish:

- AI Recommended Price
- Mentor Starting Price
- Current Market Price
- Winning Bid
- Escrow Amount
- Released Amount

Do not use unsafe floating-point calculations for real financial values.

## AI

AI recommendations must be evidence-based.

Do not fabricate:

- Skills
- Experience
- Certifications
- Projects
- Match scores
- Pricing justification

AI pricing is a recommendation, not an absolute truth.

## Mock Data

Mock data may be used during development.

Mocks must be isolated.

Do not mix fake financial state with real blockchain state.

Do not show fake blockchain confirmations as real.

## Quality

After implementation:

1. Run type checking.
2. Run linting.
3. Run relevant tests.
4. Run the production build.
5. Check existing functionality.
6. Fix regressions.

## Main Goal

Do not optimize for the number of pages.

Optimize for one complete polished journey:

Register
→ AI Analysis
→ AI Matching
→ Marketplace
→ Auction
→ Bid
→ Win
→ Escrow
→ Google Meet
→ Session
→ Settlement
→ Review

The final product should feel:

Premium
Professional
Modern
Intelligent
Trustworthy
Simple
Fast

The hackathon judge should understand what MentorX does within approximately 30 seconds.


## Folder should look like this
MentorX/
│
├── CLAUDE.md
│
├── frontend/
│   └── README.md
│
├── backend/
│   └── README.md
│
├── ai/
│   └── README.md
│
├── blockchain/
│   └── README.md
│
└── infrastructure/
    └── README.md

    
# MentorX Backend — Complete Implementation Specification

> **MentorX — The market for expert time.**
>
> AI estimates the starting value.  
> The market discovers the actual value.  
> Arc guarantees the transaction.

This document is the **single source of truth for the MentorX backend implementation**.

The coding agent must use this document to implement the backend end-to-end.

---

# 1. Backend Product Vision

MentorX is an AI-powered professional mentorship marketplace where professional mentorship sessions are scarce, time-bound slots that can be priced dynamically through auctions.

Core flow:

```text
Mentor uploads CV / GitHub / LinkedIn / Portfolio
        ↓
AI analyzes professional evidence
        ↓
AI recommends starting hourly price
        ↓
Mentor creates availability slot
        ↓
Student discovers/searches for mentor
        ↓
AI recommends relevant mentors
        ↓
Student bids on scarce session
        ↓
Auction closes
        ↓
Winner funds USDC escrow
        ↓
Private Google Meet scheduled
        ↓
Email sent to mentor + student
        ↓
Session happens
        ↓
Session completed
        ↓
USDC released to mentor
        ↓
Review submitted
        ↓
Reputation updated

The backend is responsible for:

Authentication
Authorization
User management
Student profiles
Mentor profiles
Mentor resource management
S3 file uploads
AI mentor analysis
AI expertise extraction
AI pricing recommendations
AI mentor matching
Session creation
Session lifecycle
Auction creation
Auction lifecycle
Bid validation
Bid processing
Arc blockchain integration
USDC escrow
Winner selection
Google Calendar integration
Google Meet creation
Email notifications
Session completion
Payment release
Reviews
Reputation
Notifications
Transaction history
Blockchain reconciliation
Audit logs
Rate limiting
Validation
Security
Background jobs

# Core Architecture

                    ┌──────────────────────┐
                    │   Next.js Frontend   │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     API Gateway      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │        Lambda        │
                    └──────────┬───────────┘
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
       ┌───────────┐      ┌───────────┐    ┌────────────┐
       │ DynamoDB  │      │    S3     │    │  Bedrock   │
       └───────────┘      └───────────┘    └─────┬──────┘
                                                  │
                                                  ▼
                                           ┌────────────┐
                                           │  Strands   │
                                           │   Agent    │
                                           └────────────┘


                    ┌──────────────────────┐
                    │         Arc          │
                    │       + USDC         │
                    └──────────┬───────────┘
                               │
                               ▼
                         Blockchain
                               │
                               ▼
                       ┌────────────┐
                       │ The Graph  │
                       └────────────┘


4. Technology Stack
Backend

Use:

TypeScript
Node.js
AWS Lambda
API Gateway
DynamoDB
S3
Amazon Bedrock
Strands Agents
AWS SDK
Zod
Vitest or Jest
Blockchain

Use:

Arc
Solidity
USDC
viem or ethers
Smart contracts
The Graph
Authentication

Use:

Privy
External APIs

Use:

Google Calendar API
Google Meet
Email provider
Infrastructure

Use:

IAM
CloudWatch
EventBridge
SQS where required
Step Functions only where genuinely useful

Do not add AWS services merely to make the architecture look complicated.


# Backend Project Structure

backend/
├── src/
│   ├── handlers/
│   │   ├── auth/
│   │   ├── users/
│   │   ├── students/
│   │   ├── mentors/
│   │   ├── resources/
│   │   ├── ai/
│   │   ├── sessions/
│   │   ├── auctions/
│   │   ├── bids/
│   │   ├── escrow/
│   │   ├── meetings/
│   │   ├── reviews/
│   │   └── transactions/
│   │
│   ├── services/
│   │   ├── auth/
│   │   ├── users/
│   │   ├── students/
│   │   ├── mentors/
│   │   ├── resources/
│   │   ├── ai/
│   │   ├── sessions/
│   │   ├── auctions/
│   │   ├── bids/
│   │   ├── blockchain/
│   │   ├── escrow/
│   │   ├── google/
│   │   ├── email/
│   │   ├── reviews/
│   │   └── notifications/
│   │
│   ├── repositories/
│   │   ├── users/
│   │   ├── students/
│   │   ├── mentors/
│   │   ├── resources/
│   │   ├── sessions/
│   │   ├── auctions/
│   │   ├── bids/
│   │   ├── escrow/
│   │   ├── reviews/
│   │   └── transactions/
│   │
│   ├── agents/
│   │   ├── mentor-analysis/
│   │   ├── pricing/
│   │   └── matching/
│   │
│   ├── blockchain/
│   │   ├── contracts/
│   │   ├── abi/
│   │   ├── client.ts
│   │   ├── events.ts
│   │   └── verification.ts
│   │
│   ├── integrations/
│   │   ├── bedrock/
│   │   ├── s3/
│   │   ├── google/
│   │   ├── email/
│   │   └── privy/
│   │
│   ├── middleware/
│   │   ├── auth.ts
│   │   ├── authorization.ts
│   │   ├── validation.ts
│   │   └── rateLimit.ts
│   │
│   ├── models/
│   ├── schemas/
│   ├── config/
│   ├── utils/
│   └── app.ts
│
├── contracts/
├── tests/
├── infrastructure/
├── package.json
├── tsconfig.json
├── .env.example
└── README.md


# Environment Variables

NODE_ENV=development

AWS_REGION=
DYNAMODB_TABLE_NAME=
S3_BUCKET_NAME=

BEDROCK_MODEL_ID=

PRIVY_APP_ID=
PRIVY_APP_SECRET=

ARC_RPC_URL=
ARC_CHAIN_ID=

AUCTION_CONTRACT_ADDRESS=
ESCROW_CONTRACT_ADDRESS=

BLOCKCHAIN_PRIVATE_KEY=

THE_GRAPH_ENDPOINT=

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REDIRECT_URI=

EMAIL_FROM=
EMAIL_PROVIDER_API_KEY=

API_BASE_URL=

MOCK_AI=false
MOCK_BLOCKCHAIN=false
MOCK_GOOGLE=false
MOCK_EMAIL=false

# Authentication

Frontend
   ↓
Privy
   ↓
Authentication token
   ↓
Backend
   ↓
Verify token
   ↓
Resolve user
   ↓
Create AuthContext

interface AuthContext {
  userId: string;
  walletAddress?: string;
  role: "STUDENT" | "MENTOR" | "ADMIN";
}

userId
role
walletAddress

# Authorization

STUDENT
MENTOR
ADMIN

Student permissions

Students can:

Manage their profile
Discover mentors
Search mentors
View auctions
Place bids
Fund winning escrow
View their sessions
View meeting information for their sessions
Complete sessions
Review mentors
View transactions
Mentor permissions

Mentors can:

Manage their profile
Upload professional resources
Request AI analysis
Create sessions
Create auctions
View auction bids
View their sessions
View their earnings
View reviews
Admin permissions

Admins can:

View platform information
Inspect users
Inspect auctions
Inspect transactions
Inspect blockchain reconciliation
Resolve exceptional platform states

Always enforce authorization server-side.

9. User Model
interface User {
  id: string;

  email?: string;

  name: string;

  role:
    | "STUDENT"
    | "MENTOR"
    | "ADMIN";

  walletAddress?: string;

  avatarUrl?: string;

  createdAt: string;

  updatedAt: string;
}
10. Student Profile
interface StudentProfile {
  userId: string;

  professionalDomain: string;

  learningGoals: string[];

  skills?: string[];

  bio?: string;

  createdAt: string;

  updatedAt: string;
}

Student data should help AI matching.

11. Mentor Profile
interface MentorProfile {
  userId: string;

  title: string;

  company?: string;

  yearsOfExperience: number;

  industry?: string;

  domain: string;

  skills: string[];

  topics: string[];

  bio?: string;

  githubUrl?: string;

  linkedinUrl?: string;

  portfolioUrl?: string;

  websiteUrl?: string;

  rating?: number;

  completedSessions: number;

  verified?: boolean;

  aiProfile?: AIExpertiseProfile;

  aiRecommendedPrice?: Money;

  mentorStartingPrice?: Money;

  createdAt: string;

  updatedAt: string;
}
12. Money Model

All financial values must use atomic units.

USDC uses 6 decimals.

interface Money {
  currency: "USDC";

  amountAtomic: string;

  decimals: 6;
}

Example:

1 USDC = 1000000 atomic units

Never use floating point for:

Bids
Prices
Escrow
Settlement
Refunds
Fees

Use BigInt internally where appropriate.

13. Mentor Resources

Supported resource types:

CV
GITHUB
LINKEDIN
PORTFOLIO
CERTIFICATION
PROJECT
PUBLICATION
OTHER

Model:

interface MentorResource {
  id: string;

  mentorId: string;

  type:
    | "CV"
    | "GITHUB"
    | "LINKEDIN"
    | "PORTFOLIO"
    | "CERTIFICATION"
    | "PROJECT"
    | "PUBLICATION"
    | "OTHER";

  url?: string;

  s3Key?: string;

  fileName?: string;

  contentType?: string;

  status:
    | "UPLOADING"
    | "READY"
    | "FAILED";

  createdAt: string;
}
14. S3 Upload Flow

Use presigned S3 URLs.

Flow:

Frontend
   ↓
POST /mentors/resources/upload
   ↓
Backend validates mentor
   ↓
Create resource record
   ↓
Generate S3 presigned URL
   ↓
Return URL
   ↓
Frontend uploads directly to S3
   ↓
POST /mentors/resources/complete
   ↓
Backend verifies upload
   ↓
Resource becomes READY

Recommended key:

private/mentors/{mentorId}/resources/{resourceId}/{filename}
15. Resource Security

Mentor documents are private.

Requirements:

Private S3 bucket
Server-side encryption
Least-privilege IAM
Short-lived presigned URLs
No public bucket access
No raw AWS credentials in frontend
No sensitive document data on blockchain
No sensitive CV data in public API responses
16. AI Expertise Profile
interface AIExpertiseProfile {
  summary: string;

  skills: {
    name: string;

    confidence: number;

    evidence: string[];
  }[];

  experienceAreas: string[];

  strengths: string[];

  recommendedTopics: string[];

  overallConfidence: number;

  generatedAt: string;
}

AI analysis must be based on real evidence.

17. AI Price Recommendation
interface AIPriceRecommendation {
  recommendedHourlyPrice: Money;

  minimumSuggestedPrice: Money;

  maximumSuggestedPrice: Money;

  reasoning: string;

  evidence: string[];

  confidence: number;

  generatedAt: string;
}

The AI can consider:

Professional experience
Seniority
Skills
Domain
Topic complexity
Professional evidence
Reputation
Completed sessions
Scarcity
Market conditions

The AI price is only a recommendation.

The market determines the actual auction price.

18. AI Evidence Rules

The AI must never invent professional information.

Never fabricate:

Skills
Companies
Certifications
Degrees
Projects
Job titles
Years of experience
Publications
Achievements

Bad:

"Mentor is an expert in distributed systems."

Better:

"Distributed systems appears to be a strong expertise area based on
the mentor's documented professional experience and project evidence."

AI claims should reference evidence wherever possible.

19. Strands Agent Architecture

Use Strands Agents for agentic AI workflows.

Recommended agents:

Mentor Analysis Agent
        │
        ├── Read mentor resources
        ├── Extract evidence
        ├── Identify skills
        ├── Identify experience
        ├── Generate expertise profile
        └── Generate confidence


Pricing Agent
        │
        ├── Read expertise profile
        ├── Read professional evidence
        ├── Evaluate experience
        ├── Evaluate topic complexity
        └── Recommend starting price


Matching Agent
        │
        ├── Read student goals
        ├── Read student skills
        ├── Read mentor expertise
        ├── Compare topics
        └── Generate match explanation

All agent outputs must be structured and validated.

20. Bedrock Integration

Create a dedicated Bedrock service.

Example interface:

interface BedrockService {
  generateText(
    input: string
  ): Promise<string>;

  generateStructured<T>(
    input: string,
    schema: unknown
  ): Promise<T>;
}

Do not call Bedrock directly from API handlers.

21. Mentor AI Analysis Endpoint

Endpoint:

POST /mentors/{id}/ai-analysis

Flow:

Authenticate
   ↓
Authorize mentor/admin
   ↓
Load mentor profile
   ↓
Load mentor resources
   ↓
Extract relevant evidence
   ↓
Run Strands Agent
   ↓
Call Bedrock
   ↓
Validate structured output
   ↓
Store AI expertise profile
   ↓
Generate price recommendation
   ↓
Store recommendation
   ↓
Return result
22. AI Matching Endpoint

Endpoint:

POST /mentors/match

Request:

{
  studentId: string;

  goals: string[];

  topics?: string[];

  skills?: string[];
}

Response:

interface MentorMatch {
  mentorId: string;

  score: number;

  reasons: string[];

  matchingSkills: string[];

  matchingTopics: string[];
}
23. Mentor Matching

Use deterministic matching logic plus AI explanation.

Example weighting:

Skill match       35%
Topic match       30%
Experience        15%
Career relevance  10%
Reputation         5%
Availability       5%

Do not return random scores.

Every recommended mentor should have a meaningful reason.

24. Session Model
interface MentorshipSession {
  id: string;

  mentorId: string;

  topic: string;

  description: string;

  startTime: string;

  durationMinutes: number;

  timezone: string;

  startingPrice: Money;

  auctionId?: string;

  status:
    | "DRAFT"
    | "PUBLISHED"
    | "AUCTION_LIVE"
    | "AUCTION_CLOSED"
    | "SCHEDULED"
    | "IN_PROGRESS"
    | "COMPLETED"
    | "CANCELLED";

  createdAt: string;

  updatedAt: string;
}
25. Session Creation

Endpoint:

POST /mentors/{id}/sessions

Only the mentor who owns the profile can create the session.

Validate:

Topic
Description
Start time
Duration
Timezone
Starting price
Availability
Session overlap

Reject sessions in the past.

26. Session Lifecycle

Canonical state flow:

DRAFT
   ↓
PUBLISHED
   ↓
AUCTION_LIVE
   ↓
AUCTION_CLOSED
   ↓
SCHEDULED
   ↓
IN_PROGRESS
   ↓
COMPLETED

Alternative:

PUBLISHED
   ↓
CANCELLED

Never allow arbitrary state changes from the frontend.

27. Auction Model
type AuctionStatus =
  | "UPCOMING"
  | "LIVE"
  | "ENDING_SOON"
  | "CLOSED"
  | "WINNER"
  | "ESCROW"
  | "SCHEDULED"
  | "COMPLETED"
  | "SETTLED";
interface Auction {
  id: string;

  sessionId: string;

  mentorId: string;

  startingPrice: Money;

  currentPrice: Money;

  highestBidId?: string;

  blockchainAuctionId?: string;

  status: AuctionStatus;

  startTime: string;

  endTime: string;

  createdAt: string;

  updatedAt: string;
}
28. Auction Lifecycle

Canonical lifecycle:

CREATE
   ↓
OPEN
   ↓
BIDDING
   ↓
CLOSED
   ↓
WINNER
   ↓
ESCROW
   ↓
SESSION
   ↓
COMPLETED
   ↓
PAYMENT RELEASED

The application state and blockchain state must be reconciled.

29. Auction Creation

Endpoint:

POST /auctions

Only mentors can create auctions.

Validate:

Session ownership
Session state
Starting price
Start time
End time
Time validity
No overlapping sessions
Valid USDC amount

If production blockchain mode is enabled:

Create application auction
        ↓
Create blockchain auction
        ↓
Store blockchainAuctionId
        ↓
Store transaction
30. Bid Model
interface Bid {
  id: string;

  auctionId: string;

  bidderId: string;

  amount: Money;

  transactionHash?: string;

  blockchainBidId?: string;

  status:
    | "PENDING"
    | "CONFIRMED"
    | "FAILED";

  createdAt: string;
}
31. Bid Validation

Before accepting a bid:

Authenticate
   ↓
Verify STUDENT role
   ↓
Load auction
   ↓
Verify auction is LIVE
   ↓
Verify current time < endTime
   ↓
Verify bidder != mentor
   ↓
Validate amount
   ↓
Validate wallet
   ↓
Validate minimum increment
   ↓
Accept bid

Never trust frontend currentPrice.

Always read the authoritative value from backend/database/blockchain.

32. Bid Endpoint

Endpoint:

POST /auctions/{id}/bids

Request:

{
  amountAtomic: string;

  transactionHash?: string;
}

Response:

{
  bidId: string;

  status:
    | "PENDING"
    | "CONFIRMED";

  currentPrice: Money;
}
33. Bid Concurrency

Two users can bid simultaneously.

Example:

Current price = 50 USDC

Student A submits 55 USDC
Student B submits 55 USDC

The backend must guarantee deterministic ordering.

Use DynamoDB conditional writes or transactions.

Only one bid can become the authoritative highest bid at a time.

Never rely on:

frontend state

for concurrency.

34. Bid Idempotency

Financial operations must support idempotency.

Support:

Idempotency-Key: <unique-key>

Required for:

Bid creation
Escrow funding
Settlement
Payment release
Refunds

Repeated requests with the same idempotency key must not create duplicate financial actions.

35. Arc Integration

Arc is the blockchain settlement layer.

Use Arc for:

Auction state
Bids
USDC
Escrow
Winner settlement
Payment release

AWS is not the financial source of truth.

Blockchain is authoritative for financial settlement.

DynamoDB stores application state and indexed transaction state.

36. Smart Contract Structure

Recommended:

contracts/
├── MentorXArena.sol
├── MentorXEscrow.sol
└── interfaces/
    └── IMentorX.sol
37. MentorXArena Contract

Responsibilities:

Create auctions
Track auctions
Accept bids
Track highest bid
Close auctions
Select winner
Emit events

Required events:

event AuctionCreated(
    uint256 indexed auctionId,
    address indexed mentor,
    uint256 startingPrice,
    uint256 startTime,
    uint256 endTime
);

event BidPlaced(
    uint256 indexed auctionId,
    address indexed bidder,
    uint256 amount
);

event AuctionClosed(
    uint256 indexed auctionId
);

event AuctionWon(
    uint256 indexed auctionId,
    address indexed winner,
    uint256 amount
);
38. MentorXEscrow Contract

Responsibilities:

Accept USDC
Lock winner funds
Track escrow
Release funds
Refund funds where required

Events:

event EscrowFunded(
    uint256 indexed auctionId,
    address indexed payer,
    uint256 amount
);

event SessionCompleted(
    uint256 indexed auctionId
);

event PaymentReleased(
    uint256 indexed auctionId,
    address indexed mentor,
    uint256 amount
);

event Refunded(
    uint256 indexed auctionId,
    address indexed payer,
    uint256 amount
);
39. Blockchain Transaction Lifecycle

For every blockchain transaction:

CREATE
   ↓
SUBMIT
   ↓
PENDING
   ↓
CONFIRMED

Failure:

PENDING
   ↓
FAILED

Never mark a transaction CONFIRMED before blockchain confirmation.

40. Blockchain Verification

Backend must verify:

Transaction hash
Chain ID
Contract address
Event type
Event parameters
Sender
Amount
Auction ID
Block confirmation

Never trust a transaction hash submitted by the frontend without verifying it onchain.

41. Blockchain Transaction Model
interface BlockchainTransaction {
  id: string;

  userId?: string;

  auctionId?: string;

  sessionId?: string;

  type:
    | "AUCTION_CREATED"
    | "BID"
    | "ESCROW"
    | "SETTLEMENT"
    | "REFUND";

  txHash?: string;

  status:
    | "PENDING"
    | "CONFIRMED"
    | "FAILED";

  amount?: Money;

  createdAt: string;

  confirmedAt?: string;
}
42. Financial Source of Truth

For financial values:

Blockchain
    ↓
Authoritative

DynamoDB:

Indexed application state

Frontend:

Display only

Never allow:

Frontend → financial truth
43. The Graph Integration

Use The Graph to index blockchain events.

Index:

AuctionCreated
BidPlaced
AuctionClosed
AuctionWon
EscrowFunded
SessionCompleted
PaymentReleased
ReviewSubmitted

Use The Graph for:

Auction history
Bid history
Transaction history
Blockchain-derived reputation
Historical settlement data
44. Blockchain Event Processor

Create a blockchain event processing service.

Flow:

Blockchain
   ↓
The Graph / Event listener
   ↓
Event received
   ↓
Validate event
   ↓
Find application entity
   ↓
Update DynamoDB
   ↓
Write audit record

Event processing must be idempotent.

Do not process the same blockchain event twice.

45. Auction Closing

Auction closing must happen server-side.

Options:

EventBridge scheduled job
Lambda worker
Step Functions where necessary

Flow:

Auction endTime reached
        ↓
Worker detects auction
        ↓
Verify auction still LIVE
        ↓
Close auction
        ↓
Determine highest valid bid
        ↓
Set winner
        ↓
Update state
        ↓
Emit/verify blockchain state
        ↓
Notify winner
        ↓
Notify mentor
46. Winner Selection

Winner is:

Highest valid bid at auction close.

Tie-breaking must be deterministic.

Recommended:

Highest amount
   ↓
Earliest confirmed bid

Never choose winners randomly.

47. Winner Flow

After winner selection:

Auction CLOSED
      ↓
Winner selected
      ↓
Auction WINNER
      ↓
Winner notified
      ↓
Winner funds USDC escrow
      ↓
ESCROW confirmed
      ↓
Schedule session

Losing bidders must be informed where appropriate.

48. Escrow Model

Create an escrow record.

Recommended:

interface Escrow {
  id: string;

  auctionId: string;

  sessionId: string;

  payerId: string;

  mentorId: string;

  amount: Money;

  status:
    | "PENDING"
    | "FUNDED"
    | "RELEASED"
    | "REFUNDED"
    | "FAILED";

  fundingTxHash?: string;

  releaseTxHash?: string;

  createdAt: string;

  updatedAt: string;
}
49. Escrow Funding

Endpoint:

POST /escrow/{id}/fund

Flow:

Authenticate winner
   ↓
Verify winner identity
   ↓
Verify escrow status
   ↓
Verify amount
   ↓
Verify blockchain transaction
   ↓
Confirm USDC transfer
   ↓
Mark escrow FUNDED
   ↓
Create meeting
   ↓
Send notifications

Never mark escrow funded based solely on frontend confirmation.

50. Escrow Invariants

The backend must guarantee:

Escrow amount = winning bid amount

and:

Only winner can fund escrow

and:

Escrow cannot be funded twice

and:

Escrow cannot release before session completion

and:

Released escrow cannot be released again
51. Google Calendar / Meet

Use Google Calendar API to create the private session event.

The meeting must only be created after:

Auction won
      ↓
Escrow funded

Do not expose meeting URLs before the session is properly funded.

52. Meeting Model
interface SessionMeeting {
  sessionId: string;

  calendarEventId?: string;

  meetingUrl?: string;

  status:
    | "PENDING"
    | "CREATED"
    | "FAILED";

  createdAt?: string;
}
53. Google Meet Privacy

Meeting details must be visible only to:

Winning student
Mentor
Authorized administrators

Do not expose private meeting URLs in:

Public mentor profiles
Marketplace API
Auction listings
Public blockchain data
54. Meeting Idempotency

Meeting creation must be idempotent.

If the request is repeated:

Do not create multiple Google Calendar events.

Check:

sessionId
calendarEventId
meeting status

before creating a new event.

55. Email System

Email notifications should be sent for:

Auction created
Bid confirmed
Auction ending soon
Auction won
Auction lost
Escrow funded
Session scheduled
Session reminder
Session completed
Payment released
Review received

Use an email service abstraction.

Example:

interface EmailService {
  send(
    to: string,
    template: string,
    data: Record<string, unknown>
  ): Promise<void>;
}
56. Email Rules

Email sending should not break financial state transitions.

Bad:

Payment succeeds
↓
Email fails
↓
Payment marked failed

Correct:

Payment succeeds
↓
Payment state confirmed
↓
Email attempted separately

Use background jobs where appropriate.

57. Session Completion

Endpoint:

POST /sessions/{id}/complete

Validate:

Authenticated user
Session exists
User is participant
Session time has passed or authorized completion condition
Session is not already completed

Then:

Session COMPLETED
      ↓
Escrow eligible for release
      ↓
Payment release initiated
58. Payment Release

Flow:

Session completed
       ↓
Verify escrow FUNDED
       ↓
Verify settlement eligibility
       ↓
Call Arc escrow release
       ↓
Wait for confirmation
       ↓
Mark escrow RELEASED
       ↓
Record transaction
       ↓
Update mentor earnings
       ↓
Send notification

Never mark settlement complete before blockchain confirmation.

59. Settlement Invariants

Guarantee:

Only funded escrow can settle.
Only completed session can release funds.
Escrow cannot release twice.
Released amount = escrow amount.
Settlement transaction must be verified onchain.
60. Review Model
interface Review {
  id: string;

  sessionId: string;

  reviewerId: string;

  mentorId: string;

  rating: number;

  comment?: string;

  createdAt: string;
}

Rating:

1–5

Validate server-side.

61. Review Authorization

Only the student who won and completed the session can review the mentor.

Rules:

Must be session participant
AND
Session must be COMPLETED
AND
Reviewer must not have already reviewed

Prevent duplicate reviews.

62. Reputation

Mentor reputation should be calculated from real data.

Possible factors:

Average rating
Completed sessions
Review count
Session success rate
AI evidence confidence
Verified professional evidence

Do not fake reputation.

Never randomize ratings.

63. Notification Model
interface Notification {
  id: string;

  userId: string;

  type:
    | "BID_CONFIRMED"
    | "AUCTION_WON"
    | "SESSION_SCHEDULED"
    | "PAYMENT_RELEASED"
    | "REVIEW_RECEIVED";

  title: string;

  message: string;

  read: boolean;

  createdAt: string;
}
64. API Response Format

Use a consistent response format.

Success:

{
  success: true,
  data: {},
  meta?: {}
}

Error:

{
  success: false,
  error: {
    code: string,
    message: string,
    details?: unknown
  }
}

Never expose:

Stack traces
Secrets
Private keys
Internal database errors
Sensitive AI prompts
65. Authentication APIs
POST /auth/verify

Verify Privy authentication.

Response:

{
  success: true,
  data: {
    user: User
  }
}
GET /users/me

Return authenticated user.

66. Student APIs
POST /students/profile
GET /students/profile
PATCH /students/profile

Requirements:

Authenticate
Verify student role
Validate input
Store profile
Return normalized data
67. Mentor APIs
POST /mentors/profile
GET /mentors/{id}
PATCH /mentors/profile
GET /mentors

Mentor profile updates must be validated.

68. Resource APIs
POST /mentors/resources/upload
POST /mentors/resources/complete
GET /mentors/resources

Only the owning mentor or authorized admin can access private resources.

69. AI APIs
POST /mentors/{id}/ai-analysis
GET /mentors/{id}/ai-analysis
POST /mentors/match

AI endpoints must:

Authenticate
Authorize
Validate input
Validate AI output
Store result
Return structured response
70. Session APIs
POST /mentors/{id}/sessions
GET /sessions/{id}
PATCH /sessions/{id}
GET /users/me/sessions
POST /sessions/{id}/complete

Session ownership must be checked.

71. Auction APIs
POST /auctions
GET /auctions
GET /auctions/{id}
POST /auctions/{id}/close
GET /users/me/auctions

Public marketplace data should not reveal private information.

72. Bid APIs
POST /auctions/{id}/bids
GET /auctions/{id}/bids

Bid history must show public auction information only.

Sensitive wallet/payment information must be protected where appropriate.

73. Escrow APIs
POST /escrow/{id}/fund
GET /escrow/{id}

Only authorized participants can access escrow details.

74. Meeting APIs
POST /sessions/{id}/schedule
GET /sessions/{id}/meeting

Meeting creation requires:

Winning auction
+
Funded escrow
75. Review APIs
POST /sessions/{id}/review

Validate:

Reviewer
Session
Completion
Rating
Duplicate review
76. Transaction APIs
GET /users/me/transactions

Return:

Transaction type
Amount
Status
Hash
Date
Related session/auction

Never return private blockchain credentials.

77. DynamoDB Entities

Recommended entities:

User
StudentProfile
MentorProfile
MentorResource
AIExpertiseProfile
Session
Auction
Bid
Escrow
Meeting
Review
Notification
BlockchainTransaction
AuditEvent
IdempotencyRecord
78. Required Database Access Patterns

The database must support:

Get user by ID

Get student profile by user ID

Get mentor profile by user ID

Get mentor by ID

List mentors

Search mentors

Get mentor resources

Get session by ID

List sessions by mentor

List sessions by student

Get auction by ID

List active auctions

List auctions by mentor

Get bids by auction

Get highest bid

Get escrow by auction

Get meeting by session

Get reviews by mentor

Get transactions by user

Get notifications by user

Get audit events

Use indexes rather than scanning entire tables.

79. Time Handling

All backend timestamps must be stored in UTC.

Use ISO 8601:

2026-01-01T12:00:00.000Z

Frontend can convert to local timezone.

Never compare local strings without normalization.

Auction times must use UTC internally.

80. Rate Limiting

Protect:

Authentication
AI endpoints
Mentor search
Auction creation
Bid creation
Review submission
Resource uploads

Higher restrictions should apply to expensive AI endpoints.

Example:

AI analysis:
10 requests/hour/user

Mentor search:
100 requests/minute/user

Bid:
reasonable burst protection

Exact limits can be adjusted.

81. Input Validation

Use Zod or equivalent.

Validate:

Strings
Numbers
Enums
Wallet addresses
URLs
Dates
USDC amounts
IDs
Ratings
Session duration

Never trust frontend validation.

82. Error Handling

Use typed errors.

Example:

class AppError extends Error {
  constructor(
    public code: string,
    message: string,
    public statusCode: number
  ) {
    super(message);
  }
}

Common errors:

UNAUTHORIZED
FORBIDDEN
NOT_FOUND
VALIDATION_ERROR
AUCTION_NOT_ACTIVE
BID_TOO_LOW
AUCTION_CLOSED
ESCROW_NOT_FUNDED
SESSION_NOT_COMPLETED
ALREADY_PROCESSED
BLOCKCHAIN_ERROR
EXTERNAL_SERVICE_ERROR
RATE_LIMITED
83. Service Architecture

Handlers should only:

Parse request
   ↓
Authenticate
   ↓
Validate input
   ↓
Call service
   ↓
Format response

Services should contain business logic.

Repositories should contain database operations.

Integrations should contain external API logic.

84. Repository Architecture

Example:

interface AuctionRepository {
  create(
    auction: Auction
  ): Promise<Auction>;

  getById(
    id: string
  ): Promise<Auction | null>;

  update(
    id: string,
    update: Partial<Auction>
  ): Promise<Auction>;

  listActive(): Promise<Auction[]>;

  getBySessionId(
    sessionId: string
  ): Promise<Auction | null>;
}

Do not put business logic inside repositories.

85. Auction Service

The auction service should handle:

Auction creation
Auction validation
Auction opening
Auction closing
Winner selection
State transitions
Blockchain synchronization

Example:

interface AuctionService {
  createAuction(
    mentorId: string,
    input: CreateAuctionInput
  ): Promise<Auction>;

  closeAuction(
    auctionId: string
  ): Promise<Auction>;

  getAuction(
    auctionId: string
  ): Promise<Auction>;
}
86. Blockchain Service

Create a blockchain abstraction.

interface BlockchainService {
  createAuction(
    input: CreateBlockchainAuctionInput
  ): Promise<BlockchainTransaction>;

  placeBid(
    input: PlaceBlockchainBidInput
  ): Promise<BlockchainTransaction>;

  closeAuction(
    auctionId: string
  ): Promise<BlockchainTransaction>;

  fundEscrow(
    input: FundEscrowInput
  ): Promise<BlockchainTransaction>;

  releasePayment(
    input: ReleasePaymentInput
  ): Promise<BlockchainTransaction>;

  verifyTransaction(
    txHash: string
  ): Promise<boolean>;
}
87. Google Service

Create:

interface GoogleCalendarService {
  createMeeting(
    input: CreateMeetingInput
  ): Promise<SessionMeeting>;

  getMeeting(
    sessionId: string
  ): Promise<SessionMeeting | null>;
}

Must support idempotency.

88. Email Service

Create:

interface EmailService {
  send(
    to: string,
    template: string,
    data: Record<string, unknown>
  ): Promise<void>;
}

Email failures must not corrupt financial state.

89. Mock Mode

For hackathon development, support:

MOCK_AI=true
MOCK_BLOCKCHAIN=true
MOCK_GOOGLE=true
MOCK_EMAIL=true

Mock mode must still use realistic interfaces.

Example:

Frontend
   ↓
Backend
   ↓
Mock Blockchain Service
   ↓
Fake transaction hash

However, mock mode must be clearly separated from production mode.

Do not mix fake and real financial state silently.

90. Production Blockchain Mode

When:

MOCK_BLOCKCHAIN=false

Use real:

Arc RPC
USDC
Smart contracts
Blockchain verification
Transaction receipts
Event verification

Never return a fake transaction hash in production.

91. Background Jobs

Use background processing for:

Auction closing
Auction reminders
Blockchain reconciliation
Email delivery
Notification generation
Meeting reminders
Cleanup
Retry operations

Do not keep Lambda requests waiting for long-running external operations unnecessarily.

92. Auction Worker

Worker responsibilities:

Find auctions where endTime <= now
        ↓
Lock/claim auction
        ↓
Verify auction is LIVE
        ↓
Find highest valid bid
        ↓
Close auction
        ↓
Determine winner
        ↓
Update state
        ↓
Trigger notifications

Worker must be idempotent.

93. Blockchain Reconciliation

Create periodic reconciliation.

Flow:

DynamoDB transaction
        ↓
Compare with blockchain
        ↓
Verify transaction
        ↓
Compare amount
        ↓
Compare status
        ↓
Repair stale application state

Cases:

DynamoDB says PENDING
Blockchain says CONFIRMED
        ↓
Update DynamoDB

Never change blockchain state based solely on DynamoDB.

94. Security

Security requirements:

Validate every request
Authenticate every protected endpoint
Authorize every resource
Encrypt sensitive data
Use private S3 buckets
Protect blockchain private keys
Never expose secrets
Rate limit endpoints
Validate URLs
Validate wallet addresses
Prevent duplicate financial operations
Prevent unauthorized session access
Prevent unauthorized meeting access
Log important financial events
Use least-privilege IAM
95. Sensitive Data

Sensitive data includes:

CVs
Private resources
OAuth credentials
Google tokens
Blockchain private keys
Email credentials
Authentication tokens

Never store sensitive data:

on public blockchain

Never include secrets in logs.

Never include private documents in AI responses returned to unauthorized users.

96. IAM

Use least-privilege IAM.

Lambda should only have permissions it requires.

Examples:

Lambda
  ↓
DynamoDB read/write only required tables

Lambda
  ↓
S3 access only required bucket/prefix

Lambda
  ↓
Bedrock invocation permission

Worker
  ↓
Required DynamoDB/event permissions

Never use unrestricted:

*

permissions unless absolutely necessary.

97. Logging

Use structured logs.

Example:

logger.info({
  event: "AUCTION_CLOSED",
  auctionId,
  winnerId,
  amountAtomic
});

Do not log:

Private keys
Auth tokens
Google OAuth tokens
Full CV contents
Sensitive personal information

Use correlation IDs.

98. Audit Trail

Create an audit event model.

Example:

interface AuditEvent {
  id: string;

  actorId?: string;

  action: string;

  entityType: string;

  entityId: string;

  metadata?: Record<string, unknown>;

  createdAt: string;
}

Important events:

USER_CREATED
PROFILE_UPDATED
RESOURCE_UPLOADED
AI_ANALYSIS_COMPLETED
AUCTION_CREATED
BID_PLACED
AUCTION_CLOSED
WINNER_SELECTED
ESCROW_FUNDED
MEETING_CREATED
SESSION_COMPLETED
PAYMENT_RELEASED
REVIEW_SUBMITTED
99. Testing

Create unit and integration tests.

Minimum test coverage:

Authentication
Valid token
Invalid token
Expired token
Authorization
Student cannot create mentor auction
Mentor cannot access another mentor's private resources
Auctions
Create auction
Reject invalid auction
Open auction
Close auction
Select winner
Bids
Valid bid
Low bid rejected
Late bid rejected
Mentor bid rejected
Duplicate bid handled
Concurrent bids handled
Escrow
Correct winner can fund
Wrong user rejected
Duplicate funding rejected
Invalid transaction rejected
Settlement
Incomplete session cannot settle
Completed session can settle
Duplicate settlement rejected
Reviews
Valid review
Unauthorized review rejected
Duplicate review rejected
Invalid rating rejected
AI
Structured output validation
Evidence requirement
Price recommendation validation
Matching validation
100. Final Backend Success Criteria

The backend is complete only when the following flow works end-to-end:

REGISTER
   ↓
AUTHENTICATE
   ↓
CREATE MENTOR PROFILE
   ↓
UPLOAD CV / PROFESSIONAL RESOURCES
   ↓
AI ANALYSIS
   ↓
AI PRICE RECOMMENDATION
   ↓
CREATE MENTOR SESSION
   ↓
CREATE AUCTION
   ↓
STUDENT DISCOVERS MENTOR
   ↓
AI MATCHING
   ↓
STUDENT VIEWS AUCTION
   ↓
STUDENT CONNECTS WALLET
   ↓
STUDENT PLACES BID
   ↓
BID CONFIRMED
   ↓
AUCTION CLOSES
   ↓
WINNER SELECTED
   ↓
WINNER FUNDS USDC ESCROW
   ↓
ESCROW CONFIRMED ON ARC
   ↓
GOOGLE MEET CREATED
   ↓
EMAIL SENT
   ↓
SESSION OCCURS
   ↓
SESSION COMPLETED
   ↓
USDC RELEASED TO MENTOR
   ↓
TRANSACTION CONFIRMED
   ↓
STUDENT SUBMITS REVIEW
   ↓
MENTOR REPUTATION UPDATED
FINAL IMPLEMENTATION RULES

The coding agent must follow these rules.

Rule 1 — Never trust frontend financial state

Never trust:

currentPrice
auctionStatus
escrowStatus
winner
paymentStatus

from the frontend.

Always verify server-side.

Rule 2 — Never trust frontend roles

Never trust:

role
mentorId
studentId

without authentication/authorization verification.

Rule 3 — Never expose secrets

Never expose:

private keys
API secrets
Google tokens
Privy secrets
AWS credentials

to the frontend.

Rule 4 — Never put private professional data onchain

CVs and private professional resources must remain offchain.

Blockchain should contain only the information necessary for financial/application settlement.

Rule 5 — Never fake financial confirmation

Do not claim:

Escrow funded
Payment released
Bid confirmed
Transaction confirmed

unless the appropriate blockchain state has been verified.

Rule 6 — Never fake AI scores

Do not generate random:

match scores
expertise scores
confidence scores
price recommendations

They must be based on real inputs and deterministic logic/model output.

Rule 7 — Never use floating-point financial calculations

Use:

BigInt
+
atomic USDC units

for financial calculations.

Rule 8 — Use UTC

All server timestamps must use UTC.

Rule 9 — Make financial operations idempotent

Required for:

bids
escrow
settlement
refunds
Rule 10 — Validate external API responses

Validate responses from:

Bedrock
Privy
Google
Arc
The Graph
Email provider

Never blindly trust external responses.

Rule 11 — Keep handlers thin

Handlers should not contain major business logic.

Use:

Handler
  ↓
Service
  ↓
Repository / Integration
Rule 12 — Blockchain and database must reconcile

If:

DynamoDB ≠ Blockchain

the backend must detect and reconcile the difference.

Blockchain financial state takes precedence.

Rule 13 — Prioritize P0 functionality

For the hackathon, prioritize this working path:

Authentication
      ↓
Mentor Profile
      ↓
CV Upload
      ↓
AI Analysis
      ↓
AI Price
      ↓
Session
      ↓
Auction
      ↓
Bid
      ↓
Winner
      ↓
USDC Escrow
      ↓
Google Meet
      ↓
Session Completion
      ↓
Payment Release
      ↓
Review

Do not spend most implementation time on secondary features before this flow works.

PRIMARY HACKATHON DEMO

The final demo should show:

1. Mentor registers

2. Mentor uploads CV

3. AI analyzes mentor

4. AI produces expertise profile

5. AI recommends starting price

6. Mentor creates a $X/hour session

7. Student opens marketplace

8. Student sees AI-recommended mentor

9. Student opens auction

10. Student sees:
    - Mentor
    - Expertise
    - Starting price
    - Current bid
    - Countdown
    - Bid history

11. Student places USDC bid

12. Auction closes

13. Student wins

14. Student funds USDC escrow on Arc

15. Backend verifies blockchain transaction

16. Private Google Meet is created

17. Mentor and student receive notification

18. Session is completed

19. Escrow releases USDC

20. Blockchain transaction is shown

21. Student submits review

22. Mentor reputation updates

The important story is:

Professional evidence
        ↓
AI valuation
        ↓
Market discovery
        ↓
Competitive bidding
        ↓
Blockchain escrow
        ↓
Real mentorship
        ↓
Verified settlement
        ↓
Reputation
FINAL ARCHITECTURE SUMMARY
                         MENTORX
                            │
                            ▼
                  ┌───────────────────┐
                  │    Next.js UI     │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │   API Gateway     │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │      Lambda       │
                  └─────────┬─────────┘
                            │
       ┌────────────────────┼────────────────────┐
       │                    │                    │
       ▼                    ▼                    ▼
  ┌─────────┐          ┌─────────┐        ┌────────────┐
  │DynamoDB │          │   S3    │        │  Bedrock   │
  └─────────┘          └─────────┘        └─────┬──────┘
                                                │
                                                ▼
                                          ┌────────────┐
                                          │  Strands   │
                                          │   Agent    │
                                          └────────────┘

                         FINANCIAL LAYER
                                │
                                ▼
                         ┌────────────┐
                         │    Arc     │
                         │   + USDC   │
                         └─────┬──────┘
                               │
                               ▼
                         Smart Contracts
                               │
                               ▼
                         ┌────────────┐
                         │ The Graph  │
                         └────────────┘

                       EXTERNAL SERVICES
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
             Privy         Google Meet      Email
DEFINITION OF DONE

MentorX backend is considered implemented when:

[✓] Authentication works
[✓] Authorization works
[✓] Student profile works
[✓] Mentor profile works
[✓] S3 resource upload works
[✓] AI mentor analysis works
[✓] AI price recommendation works
[✓] AI mentor matching works
[✓] Session creation works
[✓] Auction creation works
[✓] Auction lifecycle works
[✓] Bidding works
[✓] Bid concurrency is protected
[✓] Arc integration works
[✓] USDC escrow works
[✓] Winner selection works
[✓] Blockchain verification works
[✓] Google Meet creation works
[✓] Email notifications work
[✓] Session completion works
[✓] Payment release works
[✓] Reviews work
[✓] Reputation works
[✓] Notifications work
[✓] Transactions are recorded
[✓] Blockchain reconciliation works
[✓] Sensitive data is protected
[✓] Financial operations are idempotent
[✓] Tests cover critical flows

The final product must support:

REGISTER
→ ANALYZE
→ MATCH
→ DISCOVER
→ AUCTION
→ BID
→ WIN
→ ESCROW
→ MEET
→ SESSION
→ SETTLEMENT
→ REVIEW
→ REPUTATION
END



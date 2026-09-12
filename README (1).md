# MentorX

> **The onchain marketplace for scarce expert time.**

MentorX is a blockchain-powered mentorship marketplace where
professional mentors publish limited one-to-one time slots and students
compete for high-demand slots through transparent auctions.

The core idea is simple:

> **AI estimates the starting value. The market discovers the actual
> price. Blockchain guarantees the transaction.**

MentorX combines AI-powered professional analysis, a marketplace for
expert time, scheduled auctions, USDC escrow, and verifiable reputation.

------------------------------------------------------------------------

## Table of Contents

-   [Problem](#problem)
-   [Solution](#solution)
-   [How MentorX Works](#how-mentorx-works)
-   [Example](#example)
-   [Architecture](#architecture)
-   [Architecture Explained](#architecture-explained)
-   [Technology Stack](#technology-stack)
-   [AI Layer](#ai-layer)
-   [Blockchain Layer](#blockchain-layer)
-   [Data and Storage](#data-and-storage)
-   [Complete User Flow](#complete-user-flow)
-   [MVP](#mvp)
-   [MVP User Journey](#mvp-user-journey)
-   [Main Screens](#main-screens)
-   [Smart Contract](#smart-contract)
-   [Data Model](#data-model)
-   [What Is Onchain vs Offchain](#what-is-onchain-vs-offchain)
-   [Security and Privacy](#security-and-privacy)
-   [Hackathon Strategy](#hackathon-strategy)
-   [Future Improvements](#future-improvements)
-   [What We Are Not Building](#what-we-are-not-building)
-   [Project Pitch](#project-pitch)

------------------------------------------------------------------------

# Problem

Traditional mentorship platforms generally use fixed or manually
determined prices.

This creates several problems:

-   Experienced mentors may be underpriced.
-   Students have difficulty comparing mentors based on real
    professional evidence.
-   High-demand mentors have limited availability, but their scarce time
    is usually not priced dynamically.
-   Payment, completion, refunds, and reputation are controlled by a
    centralized platform.
-   Professional information is spread across resumes, GitHub,
    portfolios, and other sources.

MentorX treats expert mentorship time as a scarce marketplace resource
rather than a simple fixed-price booking.

------------------------------------------------------------------------

# Solution

MentorX creates a complete marketplace loop:

``` text
Professional Evidence
        |
        v
   AI Analysis
        |
        v
Suggested Starting Price
        |
        v
Mentor Publishes Limited Slot
        |
        v
Student Discovers Mentor
        |
        v
Scheduled Auction
        |
        v
Students Compete for Slot
        |
        v
Winner Funds USDC Escrow
        |
        v
Mentorship Session
        |
        v
Completion Confirmation
        |
        v
Payment Released
        |
        v
Reputation Updated
```

The AI does not decide what a mentor is "worth". It provides an
explainable starting price.

The market determines the final price through demand.

Blockchain provides transparent auction state, payment escrow,
settlement, and verifiable reputation events.

------------------------------------------------------------------------

# How MentorX Works

MentorX has two primary users:

## Mentor

A mentor:

1.  Signs in.
2.  Uploads a CV/resume.
3.  Adds GitHub, LinkedIn, and portfolio information.
4.  MentorX analyzes the professional evidence using AWS AI.
5.  The system generates an expertise profile and suggested starting
    price.
6.  The mentor accepts or changes the suggested price.
7.  The mentor publishes limited one-to-one time slots.
8.  High-demand slots can become auctions.
9.  The mentor conducts the session.
10. After completion, payment is released from escrow.
11. The mentor's reputation is updated.

## Student

A student:

1.  Signs in.
2.  Describes what they need help with in natural language.
3.  MentorX uses AI to find relevant mentors.
4.  The student views mentor evidence, reputation, availability, and
    current auctions.
5.  The student selects a slot.
6.  If the slot is being auctioned, the student places a bid.
7.  The highest valid bidder wins.
8.  The winning payment is placed into USDC escrow.
9.  The student attends the session.
10. Completion is confirmed.
11. The mentor receives payment and the session contributes to
    reputation.

------------------------------------------------------------------------

# Example

Imagine Rahul is a senior backend engineer with:

-   7 years of experience
-   Distributed systems experience
-   Go
-   Kubernetes
-   AWS
-   Multiple production projects
-   Relevant GitHub contributions

Rahul uploads his CV and professional information.

MentorX's AI analyzes the evidence and produces:

``` text
Expertise
-----------------------------
Backend Engineering       Expert
Distributed Systems       Expert
Go                         Advanced
Kubernetes                 Advanced
AWS                        Advanced

Experience
-----------------------------
7 years

Suggested Starting Price
-----------------------------
$25 USDC / hour

Confidence
-----------------------------
87%

Reason
-----------------------------
Strong backend experience, relevant distributed
systems projects, and professional evidence.
```

Rahul accepts the \$25 starting price and publishes:

``` text
Friday
7:00 PM - 8:00 PM

Starting Price: $25 USDC
```

Three students want the exact same slot.

``` text
Student A    $27
Student B    $32
Student C    $40
Student A    $43
```

Student A wins at \$43.

The winning amount is funded into a smart-contract escrow.

``` text
Student
   |
   | $43 USDC
   v
Arc Smart Contract
   |
   | Escrow
   v
Mentorship Session
   |
   | Completion confirmed
   v
Mentor receives $43 USDC
```

The blockchain records the auction and settlement events.

The Graph indexes those events so MentorX can display the mentor's
history and reputation.

------------------------------------------------------------------------

# Architecture

MentorX uses AWS as its primary application and AI infrastructure, while
blockchain is used where decentralization and financial guarantees
provide real value.

``` text
                         USER
                           |
                           v
                +--------------------+
                |      NEXT.JS       |
                |    MentorX Web     |
                +---------+----------+
                          |
                          v
                +--------------------+
                |    AWS AMPLIFY     |
                |   / APP RUNNER     |
                +---------+----------+
                          |
                          v
                +--------------------+
                |    API GATEWAY     |
                +---------+----------+
                          |
                 +--------+---------+
                 |                  |
                 v                  v
          +-------------+    +-------------+
          |   LAMBDA    |    |   LAMBDA    |
          | Application |    | AI Workflow |
          |    Logic    |    |             |
          +------+------+    +------+------+
                 |                  |
                 v                  v
          +-------------+    +-------------+
          |  DYNAMODB   |    |   BEDROCK   |
          |             |    |             |
          | Users       |    | AI Models   |
          | Mentors     |    +------+------+
          | Slots       |           |
          | Auctions    |       STRANDS
          | Sessions    |        AGENT
          +-------------+           |
                                    v
                             AI Valuation
                             AI Matching

          +--------------------------------+
          |              S3                |
          |       CV / Documents           |
          +--------------------------------+


                  BLOCKCHAIN LAYER

                         ARC
                          |
             +------------+------------+
             |            |            |
             v            v            v
          Auctions      USDC         Escrow
             |            |            |
             +------------+------------+
                          |
                          v
                  Session Settlement
                          |
                          v
                     THE GRAPH
                          |
                          v
                 Indexed Blockchain
                       Data
                          |
                          v
                    MentorX UI
```

------------------------------------------------------------------------

# Architecture Explained

## 1. Next.js Frontend

Next.js is the user-facing MentorX web application.

It provides:

-   Landing page
-   Authentication interface
-   Mentor dashboard
-   Student dashboard
-   Mentor profiles
-   AI search
-   Auction interface
-   Bidding interface
-   Session management
-   Reputation dashboard

Mentors and students interact with the application through the frontend.

------------------------------------------------------------------------

## 2. AWS Amplify / App Runner

The frontend is deployed using AWS infrastructure and made available
through a public URL.

For the hackathon, the goal is to have a working deployed application
that judges can open and use.

------------------------------------------------------------------------

## 3. API Gateway

Amazon API Gateway acts as the entry point between the frontend and
backend.

Example APIs:

``` text
POST /mentor/analyze
POST /mentor/create-slot
POST /auction/create
POST /auction/bid
GET  /auction/:id
POST /session/complete
POST /review
```

The frontend calls these APIs instead of directly handling all backend
operations.

------------------------------------------------------------------------

## 4. AWS Lambda

Lambda contains the backend logic.

Instead of maintaining one large backend server, MentorX can use
individual serverless functions for specific operations.

Examples:

``` text
analyzeMentor()
createMentor()
createSlot()
createAuction()
getAuction()
completeSession()
createReview()
```

Lambda can communicate with:

-   DynamoDB
-   S3
-   Amazon Bedrock
-   Arc
-   Other application services

------------------------------------------------------------------------

## 5. DynamoDB

DynamoDB is the primary application database.

It stores information such as:

``` text
Users
Mentors
Slots
Auctions
Bids
Sessions
Reviews
Reputation
```

Example mentor record:

``` text
mentorId
name
wallet
skills
experience
github
linkedin
portfolio
suggestedPrice
approvedPrice
rating
completedSessions
```

Example auction record:

``` text
auctionId
slotId
mentorId
startingPrice
currentBid
currentBidder
startTime
endTime
status
winner
```

------------------------------------------------------------------------

## 6. Amazon S3

S3 stores uploaded files such as:

-   CVs
-   Resumes
-   Portfolio documents
-   Profile images where necessary

The CV is kept offchain for privacy and cost reasons.

The basic flow is:

``` text
Mentor
   |
   v
Next.js
   |
   v
S3
   |
   v
Lambda
   |
   v
AI Processing
```

------------------------------------------------------------------------

## 7. Amazon Bedrock

Amazon Bedrock powers the AI layer.

MentorX uses it for:

### Mentor Valuation

The system analyzes:

-   Skills
-   Experience
-   Projects
-   Technologies
-   Professional evidence
-   Certifications where available
-   Public professional information where permitted

It generates:

-   Structured expertise profile
-   Experience summary
-   Skill categories
-   Suggested starting price
-   Confidence
-   Explanation for the recommendation

### Mentor Matching

A student can write:

> "I need someone to help me prepare for a distributed systems backend
> interview using Go and Kubernetes."

The AI extracts the important requirements and helps rank suitable
mentors.

------------------------------------------------------------------------

## 8. Strands Agents

Strands can orchestrate the MentorX AI workflow.

A simplified workflow can be:

``` text
                  MentorX AI Agent
                         |
            +------------+------------+
            |            |            |
            v            v            v
         Resume       GitHub       Portfolio
         Analysis     Analysis      Analysis
            |            |            |
            +------------+------------+
                         |
                         v
                  Evidence Analyzer
                         |
                         v
                  Valuation Agent
                         |
                         v
                  Price Suggestion
```

The goal is not to create unnecessary agents. The agent workflow should
exist where it makes the AI process clearer and more useful.

------------------------------------------------------------------------

# Blockchain Layer

MentorX uses **Arc** for the financial and onchain parts of the
marketplace.

Blockchain is used for actions where transparency and programmable
settlement matter.

## Onchain Operations

``` text
Create Auction
Place Bid
Close Auction
Select Winner
Fund Escrow
Complete Session
Release Payment
Refund
Reputation Events
```

The smart contract acts as the marketplace's rules engine.

------------------------------------------------------------------------

# USDC Escrow

The winning student does not simply transfer money directly to the
mentor.

Instead:

``` text
Student
   |
   | USDC
   v
Smart Contract
   |
   | Escrow
   v
Session
   |
   | Completion condition
   v
Smart Contract
   |
   v
Mentor
```

If a session is cancelled according to the marketplace rules, the
contract can return the appropriate funds to the student.

This provides a programmable settlement mechanism.

------------------------------------------------------------------------

# The Graph

The Graph indexes blockchain events so that the frontend can efficiently
retrieve marketplace history.

Important events include:

``` text
MentorRegistered
SlotCreated
AuctionCreated
BidPlaced
AuctionClosed
AuctionWon
EscrowFunded
SessionCompleted
PaymentReleased
ReviewSubmitted
```

The flow is:

``` text
Arc Smart Contract
        |
        | Events
        v
    The Graph
        |
        | Indexed data
        v
   MentorX Frontend
```

This allows MentorX to display:

-   Auction history
-   Mentor session history
-   Winning prices
-   Completed sessions
-   Reputation information
-   Onchain activity

------------------------------------------------------------------------

# Data and Storage

MentorX deliberately separates application data from blockchain data.

## Offchain

Stored using AWS infrastructure:

``` text
CVs
Private documents
AI processing
Search data
Mentor profile metadata
Meeting links
Application data
Notifications
```

## Onchain

Stored through Arc smart contracts:

``` text
Auction state
Bid state
Winner
Escrow state
Payment settlement
Completion events
Reputation events
```

This keeps sensitive and large data offchain while using blockchain for
the parts that benefit from public verification.

------------------------------------------------------------------------

# Complete User Flow

``` text
                    MENTOR
                       |
                       v
                 Upload CV
                       |
                       v
                       S3
                       |
                       v
                    Lambda
                       |
                       v
               Strands + Bedrock
                       |
                       v
               AI Profile Analysis
                       |
                       v
               Suggested Price
                       |
                       v
                Mentor Approves
                       |
                       v
                 Create Slot
                       |
                       v
                Create Auction
                       |
                       |
                       v
                    STUDENT
                       |
                       v
              Natural Language Search
                       |
                       v
                 AI Matching
                       |
                       v
                Mentor Profile
                       |
                       v
                  View Slot
                       |
                       v
                    Auction
                       |
                       v
                     Bid
                       |
                       v
                 Arc Contract
                       |
                       v
                  Winner
                       |
                       v
                USDC Escrow
                       |
                       v
                 Session
                       |
                       v
             Completion Confirmed
                       |
                       v
                Payment Released
                       |
                       v
                Review Submitted
                       |
                       v
                 The Graph
                       |
                       v
                 Reputation
```

------------------------------------------------------------------------

# MVP

The MVP should focus on one complete working vertical slice rather than
attempting to build every possible feature.

## Must Have

### Authentication

-   Google/social login
-   Wallet connection
-   Embedded wallet support where appropriate

### Mentor

-   Create mentor profile
-   Upload CV
-   Add GitHub/LinkedIn/portfolio
-   AI profile analysis
-   AI suggested starting price
-   Mentor can accept or override price
-   Create one-hour availability slots

### Student

-   Search for mentors
-   Basic AI mentor matching
-   View mentor profile
-   View available slots
-   View active auctions

### Marketplace

-   Create auction
-   Display starting price
-   Display current bid
-   Display bidder count
-   Countdown timer
-   Place bids
-   Close auction
-   Select winner

### Blockchain

-   Real Arc transaction for important actions
-   USDC payment
-   Escrow
-   Winner selection
-   Payment release/refund
-   Blockchain transaction status

### Reputation

-   Session completion
-   Review
-   Basic mentor rating
-   Completed sessions
-   Historical winning prices

### AWS

The deployed MVP should meaningfully use:

``` text
Amazon Bedrock
AWS Lambda
API Gateway
DynamoDB
S3
AWS Amplify / App Runner
```

Strands Agents can be used for the AI orchestration layer.

------------------------------------------------------------------------

# MVP User Journey

The most important demo should work from beginning to end.

``` text
1. Mentor signs in
        |
2. Uploads CV
        |
3. AWS AI analyzes CV
        |
4. AI suggests starting price
        |
5. Mentor approves price
        |
6. Mentor creates a slot
        |
7. Student searches for mentor
        |
8. Student views mentor profile
        |
9. Student enters auction
        |
10. Student places bid
        |
11. Arc records bid
        |
12. Auction closes
        |
13. Winner is selected
        |
14. Winner funds USDC escrow
        |
15. Session takes place
        |
16. Completion is confirmed
        |
17. Payment is released
        |
18. Student leaves review
        |
19. Reputation is updated
```

This complete journey is more important than having dozens of unfinished
features.

------------------------------------------------------------------------

# Main Screens

## 1. Landing Page

``` text
MENTORX

The market for expert time.

AI estimates the starting value.
The market discovers the actual price.
Blockchain guarantees the transaction.

[ Find a Mentor ]
[ Become a Mentor ]
```

------------------------------------------------------------------------

## 2. Mentor Onboarding

``` text
Become a Mentor

CV / Resume        [ Upload ]
GitHub              [ Add ]
LinkedIn            [ Add ]
Portfolio           [ Add ]

[ Analyze My Profile ]
```

------------------------------------------------------------------------

## 3. AI Analysis

``` text
YOUR EXPERTISE

Backend Engineering       Expert
Distributed Systems       Expert
Go                         Advanced
Kubernetes                 Advanced
AWS                        Advanced

Experience: 7 years

AI Suggested Starting Price
$25 USDC / hour

Confidence: 87%

[ Accept Price ]
[ Change Price ]
```

------------------------------------------------------------------------

## 4. Mentor Profile

Display:

-   Professional summary
-   Skills
-   Experience
-   Evidence
-   Rating
-   Completed sessions
-   Historical prices
-   Available slots
-   Active auctions

------------------------------------------------------------------------

## 5. Student AI Search

``` text
What do you need help with?

"I need help preparing for a distributed
systems backend interview."

[ Find Mentor ]
```

Results should show relevant mentors with a match score and supporting
skills.

------------------------------------------------------------------------

## 6. Auction Page

This is the main product screen.

``` text
Senior Backend Engineering
Distributed Systems Interview

Friday
7:00 PM - 8:00 PM

Starting Price       $25 USDC
Current Bid          $43 USDC
Bidders              4
Ends In              08:32

Bid History

Student A             $27
Student B             $32
Student C             $40
Student A             $43

[ Place Bid ]

---------------------------------

Blockchain Status
✓ Bid recorded onchain
✓ Arc transaction confirmed
✓ USDC escrow
```

The auction should be the visual centerpiece of the demo.

------------------------------------------------------------------------

# Smart Contract

The MVP smart contract should remain small and focused.

Possible functions:

``` solidity
createSlot()
createAuction()
placeBid()
closeAuction()
fundEscrow()
completeSession()
releasePayment()
refundBid()
cancelSlot()
```

Important rules:

-   A mentor cannot sell the same slot twice.
-   A bid must satisfy the minimum/current bid requirement.
-   An auction cannot be changed after it closes.
-   Only the winning bid should become the final payment.
-   Losing bidders can reclaim/refund their funds according to the
    contract rules.
-   Payment cannot be released before the required completion condition.
-   Cancelled sessions have an explicit refund path.
-   Unauthorized users cannot release payments.

The exact contract implementation should be tested thoroughly before
using real funds. The hackathon should use testnet funds.

------------------------------------------------------------------------

# Data Model

## Mentor

``` text
mentorId
wallet
ENS
name
photo
summary
skills
yearsOfExperience
github
linkedin
portfolio
aiSuggestedPrice
mentorApprovedPrice
rating
completedSessions
```

## Slot

``` text
slotId
mentorId
startTime
endTime
status
basePrice
auctionId
```

## Auction

``` text
auctionId
slotId
openingTime
closingTime
minimumBid
currentHighestBid
currentHighestBidder
status
winner
```

## Bid

``` text
bidId
auctionId
bidder
amount
timestamp
status
```

## Session

``` text
sessionId
mentor
student
slot
winningPrice
escrowStatus
completionStatus
review
```

------------------------------------------------------------------------

# What Is Onchain vs Offchain

  Data / Action        Location
  -------------------- ---------------
  CV                   AWS S3
  AI analysis          AWS / Bedrock
  Mentor metadata      DynamoDB
  Search               AWS backend
  Meeting link         Offchain
  Auction state        Arc
  Bid state            Arc
  Winner               Arc
  USDC escrow          Arc
  Payment release      Arc
  Completion event     Arc
  Reputation events    Arc
  Blockchain history   The Graph

The general rule is:

> **Put sensitive, large, or frequently changing application data
> offchain. Put financial and trust-critical state onchain.**

------------------------------------------------------------------------

# Security and Privacy

MentorX should follow several important principles:

### Never store CVs or private documents onchain

Use S3 and appropriate access controls.

### Do not expose private professional information unnecessarily

Only process and display information with appropriate user consent.

### Validate bids in the smart contract

Do not rely only on frontend validation.

### Prevent duplicate slots

A mentor should not be able to sell the same one-hour slot twice.

### Make auction closing deterministic

The smart contract should define clear auction start and end conditions.

### Protect escrow

Payment release should only happen when the defined conditions are
satisfied.

### Use testnet funds

The hackathon version should not require real money.

### AI valuation should be explainable

The suggested price should be presented as a recommendation, not an
objective measurement of a person's worth.

------------------------------------------------------------------------

# Hackathon Strategy

MentorX should be built as a **complete vertical slice**.

Do not try to implement every feature from the long-term product
specification.

The strongest hackathon demo is:

``` text
Mentor Onboarding
       |
       v
AWS AI Analysis
       |
       v
Suggested Price
       |
       v
Publish Slot
       |
       v
AI Student Matching
       |
       v
Live Auction
       |
       v
Arc Bid
       |
       v
USDC Escrow
       |
       v
Session Completion
       |
       v
Payment Release
       |
       v
Reputation
```

The important blockchain transactions should be real.

The AI analysis should be real.

The deployed website should be real.

Some surrounding data can be seeded for a reliable demo.

For example, the application can contain 10--20 realistic mentor
profiles while one complete CV analysis flow is fully functional.

This gives the judges a believable marketplace without requiring an
enormous amount of backend data.

------------------------------------------------------------------------

# Suggested Demo

A 5--7 minute demo can follow this sequence.

### 0:00 - Problem

Explain that expert time is scarce, but most mentorship platforms use
fixed pricing.

### 0:30 - Mentor onboarding

Upload Rahul's CV and professional information.

### 1:30 - AI valuation

Show AWS AI extracting expertise and generating the suggested starting
price.

### 2:00 - Publish slot

Rahul publishes a Friday 7 PM slot.

### 2:30 - Student discovery

A student describes a distributed systems interview requirement.

AI matches Rahul.

### 3:30 - Auction

Show multiple students competing for the same slot.

### 4:00 - Blockchain

Place a real bid and show the Arc transaction.

### 4:30 - Escrow

Show the winning USDC payment entering escrow.

### 5:00 - Session completion

Confirm completion and release the payment.

### 5:30 - Reputation

Show the updated mentor reputation and historical winning price.

End with:

> **AI estimates the starting value. The market discovers the price.
> Blockchain guarantees the transaction.**

------------------------------------------------------------------------

# Future Improvements

These features can be added after the core MVP:

-   Advanced AI mentor matching
-   More detailed price intelligence
-   ENS professional identities
-   Session summaries
-   Human verification
-   Advanced mentor analytics
-   More sophisticated reputation
-   Enterprise mentorship
-   Cross-chain payment flows
-   Improved dispute handling
-   Better market analytics
-   Recurring mentorship relationships

These should not delay the core MVP.

------------------------------------------------------------------------

# What We Are Not Building

To keep the hackathon project focused, MentorX should not initially
build:

-   A full social network
-   A custom video conferencing platform
-   A DAO
-   A token economy
-   NFT collections
-   Complex governance
-   A complicated cross-chain system
-   Full LinkedIn private API integration
-   A large decentralized identity protocol
-   A general-purpose dispute-resolution platform

The goal is to prove the marketplace model first.

------------------------------------------------------------------------

# Why Blockchain?

Blockchain is not being used simply because MentorX is a Web3 project.

It solves specific problems:

### Transparent auctions

Bid state and winner selection can be independently verified.

### Programmable escrow

USDC can be held until defined conditions are satisfied.

### Automated settlement

The smart contract can release payment according to marketplace rules.

### Verifiable reputation

Completed sessions and reviews can generate verifiable events.

### Portable transaction history

A mentor's successful sessions and marketplace history do not have to
exist only inside MentorX's private database.

------------------------------------------------------------------------

# Why AWS?

AWS is the primary infrastructure layer for the application.

It provides:

``` text
Bedrock       -> AI
Strands       -> AI orchestration
Lambda        -> Serverless backend
API Gateway   -> APIs
DynamoDB      -> Application database
S3            -> Document storage
Amplify       -> Web deployment
```

This makes AWS a fundamental part of MentorX rather than an additional
integration.

------------------------------------------------------------------------

# Why Arc?

Arc is responsible for the financial marketplace layer:

``` text
Auctions
Bids
USDC
Escrow
Winner selection
Payment settlement
```

This separation gives MentorX a clear architecture:

> **AWS runs the application and AI. Arc guarantees the financial
> transaction.**

------------------------------------------------------------------------

# Why The Graph?

The Graph makes blockchain data easier for the application to query.

It provides indexed access to:

``` text
Auctions
Bids
Winners
Sessions
Payments
Reviews
Reputation
```

This allows the frontend to show meaningful onchain history without
relying entirely on direct contract queries.

------------------------------------------------------------------------

# Why Privy?

Privy simplifies onboarding by supporting:

-   Google/social login
-   Embedded wallets
-   Existing wallet connections
-   User-friendly blockchain transactions

The goal is to let normal users interact with the marketplace without
requiring them to understand blockchain infrastructure.

------------------------------------------------------------------------

# Product Positioning

MentorX should not claim to have invented blockchain mentorship or
time-based auctions.

The differentiation is the combination of:

``` text
Professional Evidence
        +
AI-Assisted Valuation
        +
Scarce Expert Time
        +
Competitive Auction
        +
USDC Escrow
        +
Verifiable Reputation
```

The product is best described as:

> **An onchain marketplace for scarce expert time.**

------------------------------------------------------------------------

# One-Line Pitch

> **MentorX is an onchain marketplace where AI estimates a mentor's
> starting value and students compete for scarce one-to-one time through
> transparent auctions.**

------------------------------------------------------------------------

# 30-Second Pitch

> Most mentorship platforms use fixed hourly prices, even when a
> mentor's time is highly scarce. MentorX changes that model. Mentors
> upload their professional evidence, and AWS-powered AI analyzes their
> experience and suggests a starting price. Mentors publish limited
> one-to-one slots, and when demand exceeds supply, students compete
> through scheduled auctions. The winner pays in USDC through Arc
> escrow, the session is completed, payment is released, and the
> mentor's reputation is updated from verifiable activity.
>
> **AI estimates the starting value. The market discovers the actual
> price. Blockchain guarantees the transaction.**

------------------------------------------------------------------------

# Final Architecture Principle

MentorX should follow one simple rule:

``` text
AWS
|
+-- AI
+-- Backend
+-- Database
+-- Storage
+-- APIs
+-- Hosting

Arc
|
+-- Auctions
+-- Bids
+-- USDC
+-- Escrow
+-- Settlement

The Graph
|
+-- Blockchain indexing
+-- Marketplace history
+-- Reputation data

Privy
|
+-- Authentication
+-- Wallets
```

Together, these technologies create a complete marketplace rather than a
collection of unrelated integrations.

------------------------------------------------------------------------

## MentorX

**AI-powered mentorship. Market-driven pricing. Onchain settlement.**

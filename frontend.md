# MentorX Frontend — Complete Product & Implementation Specification

MentorX is an AI-powered professional mentorship marketplace where professional time is scarce and mentorship sessions can be priced dynamically through auctions.

The frontend must provide one complete, polished end-to-end experience:

Student registers → discovers mentors → receives AI mentor recommendations → views mentorship auction → bids → watches market price change → wins → pays USDC into escrow → receives private Google Meet → attends session → confirms completion → mentor receives payment → student reviews mentor → reputation updates.

This document is the complete source of truth for the MentorX frontend. Claude MUST read this file before implementing or modifying frontend functionality.

---

## 1. Product Vision

MentorX positioning:

"The market for expert time."

Core message:

"AI estimates the starting value. The market discovers the actual value. Arc guarantees the transaction."

MentorX connects students with professional mentors.

Mentors provide professional evidence such as CVs, GitHub, LinkedIn, portfolios, certifications, projects, and other resources.

AI analyzes this evidence and recommends an initial mentorship hourly price.

Mentors can accept or edit the AI recommendation.

Mentors create limited, time-bound mentorship sessions.

Students discover mentors based on their professional domain and learning goals.

Students bid for limited sessions.

The auction determines the market price.

Arc and USDC provide blockchain-based payment and escrow.

After the auction is complete and payment is secured, a private Google Meet session is scheduled.

The student and mentor receive email notifications.

After the session, payment is released to the mentor.

The student can leave a review and the mentor's reputation increases.

---

## 2. Primary Product Flow

The complete product flow is:

Student Registration
→ Professional Domain
→ Learning Goal
→ AI Mentor Discovery
→ Mentor Profile
→ Mentorship Session
→ Auction
→ AI Starting Price
→ Live Bidding
→ Bidding Graph
→ Auction Close
→ Winner Verification
→ USDC Escrow
→ Session Locked
→ Private Google Meet Created
→ Email Student
→ Email Mentor
→ Session
→ Session Completion
→ Payment Release
→ Review
→ Reputation Update

The frontend must make this journey obvious.

---

## 3. Frontend Priorities

### P0 — Must Have

Build these features first:

1. Landing page
2. Student registration
3. Mentor registration
4. Student onboarding
5. Mentor onboarding
6. Mentor professional profile
7. CV upload
8. Professional resource upload
9. AI mentor analysis
10. AI hourly price recommendation
11. Mentor price acceptance/editing
12. Mentor session creation
13. Marketplace
14. AI mentor matching
15. Mentor profile
16. Auction page
17. Live bidding
18. Bid history
19. Bidding graph
20. Auction countdown
21. Wallet connection
22. USDC payment flow
23. Escrow status
24. Winner screen
25. Google Meet scheduling
26. Email notification status
27. Session page
28. Session completion
29. Payment release
30. Review
31. Reputation
32. Student dashboard
33. Mentor dashboard

### P1 — If Time Allows

1. Advanced search
2. Marketplace filters
3. Saved mentors
4. Transaction history
5. Historical market prices
6. Advanced reputation
7. Notifications
8. Mentor earnings history
9. Advanced analytics

### P2 — Do Not Prioritize

Do not spend hackathon time building:

1. Native mobile applications
2. Full video conferencing
3. Internal chat
4. Social feed
5. DAO
6. NFT system
7. Governance
8. Token economy
9. Subscription system
10. Referral system
11. Complex admin dashboard
12. Large social network features
13. Complex CRM
14. Huge filtering system

One complete polished journey is more valuable than many unfinished features.

---

## 4. Technology

Use the existing project architecture whenever possible.

Preferred frontend stack:

- Next.js
- React
- TypeScript
- Tailwind CSS

If the existing project already has a UI library, design system, component library, routing architecture, or state-management approach, reuse it.

Do not rewrite the entire project unnecessarily.

Do not introduce unnecessary dependencies.

Use strong TypeScript types.

Avoid `any` unless absolutely necessary.

---

## 5. Desktop-First Experience

MentorX is a desktop-first web application.

Desktop should receive the most design attention because:

- Dashboards are important.
- Auctions require graphs.
- Wallet interactions are easier on desktop.
- Professional profiles need space.
- Mentors upload documents.
- Blockchain transaction states need clear presentation.

The website must still be responsive on:

- Desktop
- Laptop
- Tablet
- Mobile browser

Do not build a separate mobile application.

---

## 6. Route Structure

Use this route structure where compatible with the existing project:

/
├── /register
├── /login
├── /onboarding
│   ├── /student
│   └── /mentor
├── /marketplace
├── /student
│   ├── /dashboard
│   ├── /discover
│   ├── /auctions/[id]
│   ├── /sessions
│   └── /sessions/[id]
├── /mentor
│   ├── /dashboard
│   ├── /profile
│   ├── /resources
│   ├── /ai-analysis
│   ├── /availability
│   ├── /marketplace
│   └── /sessions
└── /mentor/[id]

If the existing application uses another routing structure, preserve the existing architecture where possible.

---

## 7. Landing Page

Route:

/

The landing page must explain MentorX within approximately 30 seconds.

### Hero

Headline:

"The market for expert time."

Supporting text:

"Find the right mentor. Bid for their time. Learn directly from professionals who have done it."

Primary button:

[ Find a Mentor ]

Secondary button:

[ Become a Mentor ]

Behavior:

If logged out:

Find a Mentor → Student registration.

Become a Mentor → Mentor registration.

If logged in:

Find a Mentor → Student discovery.

Become a Mentor → Mentor dashboard/profile.

### How MentorX Works

Show three simple steps:

01 — Discover

Find mentors matched to your goals.

02 — Bid

Compete for limited mentorship sessions.

03 — Learn

Meet privately and learn directly from the expert.

### Featured Mentors

Show real mentor data when available.

Mentor cards should include:

- Profile image
- Name
- Professional title
- Company if available
- Expertise
- Rating
- Completed sessions
- Upcoming session
- Starting price

Button:

[ View Mentor ]

### Live Marketplace

Show active sessions.

Each card should show:

- Mentor
- Topic
- Starting price
- Current bid
- Number of bids
- Time remaining

Button:

[ View Auction ]

### Trust Section

Show:

- AI Expertise Analysis
- AI Market Price Recommendation
- Marketplace Price Discovery
- USDC Escrow
- Blockchain Settlement
- Private Session Scheduling

### Final CTA

Headline:

"Your next mentor is one bid away."

Buttons:

[ Find a Mentor ]

[ Become a Mentor ]

---

## 8. Registration

Route:

/register

First ask:

"How will you use MentorX?"

Display two role options.

Student:

"Find and bid for mentorship sessions."

Button:

[ Continue as Student ]

Mentor:

"Share your expertise and earn from your time."

Button:

[ Continue as Mentor ]

---

## 9. Student Registration

Student registration must be simple.

Required information:

- Full Name
- Email
- Authentication
- Professional/job domain
- Learning goal

The professional/job domain is important because AI mentor recommendations depend on it.

Example domains:

- Software Engineering
- Backend Engineering
- Frontend Engineering
- Data Science
- Machine Learning
- Product Management
- Finance
- Marketing
- Design
- Cybersecurity
- Cloud
- Entrepreneurship
- Healthcare
- Other

Example learning goals:

- System Design Interview
- Career Growth
- Leadership
- Interview Preparation
- Technical Skills
- Career Transition
- Startup Advice
- Project Guidance

Button:

[ Complete Registration ]

Flow:

Validate form
→ Create student
→ Save professional domain
→ Save learning goal
→ Create profile
→ Redirect to /student/discover

Success:

"You're ready to find your mentor."

Button:

[ Discover Mentors ]

---

## 10. Mentor Registration

Mentor onboarding must collect enough professional information for AI analysis.

### Identity

- Full Name
- Email
- Profile photo

### Professional Information

- Professional title
- Company
- Years of experience
- Professional domain
- Industry
- Skills

Example:

Backend Engineering
System Design
Distributed Systems
AWS
Kubernetes
PostgreSQL
Microservices

### Mentorship Information

- Topics they can teach
- Preferred session duration
- Timezone
- Mentorship format

---

## 11. Mentor Professional Resources

Mentors must provide professional evidence.

Supported resources:

- CV / Resume
- GitHub
- LinkedIn
- Portfolio
- Personal Website
- Certifications
- Projects
- Publications
- Other documents

Buttons:

[ Upload CV ]

[ Replace ]

[ Remove ]

[ Add GitHub ]

[ Add LinkedIn ]

[ Add Portfolio ]

[ Add Certification ]

CV upload flow:

Choose file
→ Validate file
→ Upload to backend/S3
→ Show progress
→ Show uploaded state
→ Make resource available for AI analysis

Possible states:

- Not uploaded
- Uploading
- Uploaded
- Processing
- Analysis ready
- Error

Private documents must remain private.

CV contents must never be stored onchain.

---

## 12. AI Mentor Analysis

Mentor onboarding must provide:

[ Analyze My Expertise ]

Flow:

Validate mentor resources
→ Send request to backend
→ Strands Agent
→ Amazon Bedrock
→ Analyze professional evidence
→ Extract skills
→ Evaluate experience
→ Build expertise profile
→ Recommend hourly price
→ Return structured result

Loading state should communicate meaningful progress.

Example:

"Analyzing your professional experience..."

✓ Reading professional profile

✓ Reviewing uploaded resources

● Evaluating expertise

○ Estimating market value

○ Preparing recommendation

Do not show fake progress if the backend does not actually perform those steps.

---

## 13. AI Analysis Result

Show:

- Professional summary
- Skills
- Evidence
- Confidence when available
- Experience
- Recommended hourly price
- Recommended price range
- Reasoning

Example:

"Senior backend engineer with 8+ years of experience building distributed systems and cloud infrastructure."

AI price:

AI MARKET VALUE

$40 USDC / hour

Recommended range:

$35–$50 USDC / hour

Explain the recommendation using evidence such as:

- Professional experience
- Technical expertise
- Demonstrated projects
- Relevant resources
- Mentorship topic

AI must not fabricate skills, experience, certifications, projects, or other professional evidence.

AI pricing is a recommendation, not an absolute truth.

---

## 14. Mentor Price Selection

After AI analysis show:

[ Accept $40 ]

[ Edit Price ]

If editing:

AI Recommended Price

$40 USDC

Your Starting Price

[ $45 ]

[ Save Price ]

Always clearly distinguish:

AI Recommended Price

Mentor Starting Price

Current Market Price

Winning Bid

Escrow Amount

Released Amount

---

## 15. Mentor Session Creation

Mentors create limited, time-bound sessions.

Required fields:

- Topic
- Description
- Date
- Start Time
- Duration
- Starting Price

Example:

Topic:

Distributed Systems Interview Preparation

Description:

60-minute mock system design interview.

Date:

Friday

Start:

7:00 PM

Duration:

60 minutes

Starting Price:

$40 USDC

Buttons:

[ Create Mentorship Session ]

[ Publish Session ]

Publishing creates the marketplace session/auction.

---

## 16. Marketplace

Route:

/marketplace

Headline:

"Find your next mentor."

Search input:

[ What do you want to learn? ]

Button:

[ Find Mentors ]

Show:

- AI recommended mentors
- Active sessions
- Auction status
- Starting price
- Current market price
- Time remaining

Auction card example:

Mentor:

Senior Backend Engineer

Session:

Distributed Systems Interview Preparation

AI Starting Price:

$40 USDC

Current Market Price:

$57 USDC

Bids:

8

Ends In:

02:41:18

Button:

[ View Auction ]

---

## 17. Student AI Mentor Discovery

Route:

/student/discover

Input:

[ What do you want to learn? ]

Example:

"I need help preparing for a distributed systems backend interview."

Button:

[ Find My Mentor ]

Flow:

Student query
→ AI matching
→ Compare student goal/domain with mentor expertise
→ Rank mentors
→ Return recommendations

Show:

✦ AI understood your goal

Backend Engineering

System Design Interview

Distributed Systems

Then:

"Recommended mentors"

---

## 18. AI Mentor Matching

Mentor cards may show:

92% Match

Also show:

"Why this mentor?"

Example:

"Strong experience in distributed systems. Relevant backend engineering background. Matches your interview preparation goal."

The match score must come from actual matching logic.

Do not generate random match percentages.

If AI matching is not implemented yet, isolate mock data inside a mock service.

---

## 19. Mentor Profile

Route:

/mentor/[id]

Display:

- Avatar
- Name
- Professional title
- Company
- Years of experience
- Skills
- Projects
- Expertise
- Professional links
- Rating
- Completed sessions
- Verification status when real
- AI match insight
- Available sessions

Example:

Alex Morgan

Senior Backend Engineer

8+ years experience

Distributed Systems
AWS
Kubernetes
Backend Architecture

4.9 ★

42 completed sessions

---

## 20. Mentor Session Cards

Each session card should show:

Topic

Duration

Date/time

Starting price

Current bid

Bid count

Time remaining

Button:

[ View Auction ]

Example:

Distributed Systems Interview Prep

60 minutes

Friday · 7:00 PM

Starting price:

$40 USDC

Current bid:

$57 USDC

8 bidders

Ends in:

02:41:18

---

## 21. Auction Page

Route:

/student/auctions/[id]

This is one of the most important pages in MentorX.

The auction page should be visually impressive.

The user must immediately understand:

AI estimated value
→ Market bidding
→ Current market value
→ Scarcity
→ Opportunity to win

---

## 22. Auction Header

Show:

Mentor:

Alex Morgan

Session:

Distributed Systems Interview Preparation

Date:

Friday · 7:00 PM

Duration:

60 minutes

---

## 23. Auction Price Display

Show prominently:

AI STARTING PRICE

$40 USDC

CURRENT MARKET PRICE

$57 USDC

Use a visual comparison:

AI prediction → $40

Market price → $57

This comparison demonstrates the core MentorX concept.

---

## 24. Bidding Graph

The auction page MUST include a price graph.

Example concept:

Price

$60 |                         ●
$55 |                    ●
$50 |               ●
$45 |          ●
$40 |●
    +----------------------------> Time

The graph should show:

- Price
- Time
- Starting price
- Bid progression
- Current market price
- Optional AI reference line

The graph must use actual auction data in production.

Do not randomize production bidding values.

The graph should update as new bids arrive.

---

## 25. Bid History

Show recent bids.

Example:

$57 USDC — 2 min ago

$55 USDC — 4 min ago

$52 USDC — 6 min ago

$47 USDC — 8 min ago

$42 USDC — 10 min ago

If wallet addresses are shown, abbreviate them.

Do not expose unnecessary personal information.

---

## 26. Auction Statistics

Show:

Starting Price

$40 USDC

Current Bid

$57 USDC

Bidders

8

Time Remaining

02:41:18

---

## 27. Auction Countdown

Display a prominent countdown.

Example:

02:41:18

When appropriate:

ENDING SOON

The countdown is only a visual display.

Backend/blockchain is authoritative for:

- Auction end
- Valid bids
- Winner
- Settlement

Never decide the auction winner based only on frontend timer logic.

---

## 28. Bid Form

Show:

Your Bid

[ $60 ]

[ Place Bid ]

Validate that the bid is higher than the current valid bid.

If current bid is:

$57 USDC

then:

$55 USDC

must be rejected.

Show a clear validation message.

---

## 29. Place Bid Flow

When user clicks:

[ Place Bid ]

Flow:

Validate bid amount
→ Check current auction state
→ Check bid > current bid
→ Check wallet
→ Open wallet confirmation
→ Privy wallet
→ Arc transaction
→ Blockchain confirmation
→ Backend/indexer update
→ Refresh auction
→ Show confirmed bid

---

## 30. Bid Confirmation Modal

Show:

Confirm Your Bid

Your Bid:

$60 USDC

Current Bid:

$57 USDC

Wallet:

0x12...89AB

Network:

Arc

Buttons:

[ Confirm Bid ]

[ Cancel ]

---

## 31. Bid Transaction States

Support:

- Place Bid
- Confirming
- Waiting for Wallet
- Waiting for Blockchain
- Bid Confirmed
- Transaction Failed
- Try Again

Never display "Bid Confirmed" before actual confirmation.

Never fake transaction hashes.

---

## 32. Wallet

Use Privy for wallet/authentication where configured.

Logged-out:

[ Connect Wallet ]

Connected:

0x12...89AB

Keep wallet UI simple.

MentorX is not a crypto trading platform.

Blockchain exists to provide trustworthy transactions and settlement.

---

## 33. USDC

All financial values must clearly specify USDC.

Correct:

$40 USDC

$57 USDC

$60 USDC

Avoid ambiguous values such as:

$40

$57

$60

when the currency is not otherwise obvious.

---

## 34. Financial Terminology

Use these terms consistently:

AI Recommended Price

Mentor Starting Price

Current Market Price

Winning Bid

Escrow Amount

Released Amount

Do not mix these concepts.

---

## 35. Financial Precision

Do not use unsafe JavaScript floating-point arithmetic for real financial transactions.

Use appropriate integer/base-unit or typed monetary representations.

The frontend must never be authoritative for financial calculations.

---

## 36. Auction Lifecycle

Represent the auction lifecycle as:

UPCOMING
→ LIVE
→ ENDING SOON
→ CLOSED
→ WINNER
→ ESCROW
→ SCHEDULED
→ COMPLETED
→ SETTLED

Backend/blockchain is authoritative.

---

## 37. Auction Closed

After the auction ends:

Auction Closed

Winning Bid:

$60 USDC

Winner:

Confirmed

Next:

Payment Security

Do not allow new bids after the auction is officially closed.

---

## 38. Winner Screen

Show:

"You won the session 🎉"

Winning Bid:

$60 USDC

Mentor:

Alex Morgan

Session:

Distributed Systems Interview Preparation

Date:

Friday · 7:00 PM

---

## 39. Escrow

Show:

SESSION SECURED

$60 USDC

✓ Payment secured in escrow

✓ Mentor notified

✓ Session confirmed

Timeline:

✓ Bid Won

✓ Payment Secured

○ Session Scheduled

○ Session Completed

○ Payment Released

Never claim escrow is funded until backend/blockchain confirms it.

---

## 40. Escrow Flow

Conceptually:

Student
→ Arc
→ USDC
→ Escrow Smart Contract
→ Session
→ Completion
→ Mentor

Frontend displays authoritative state.

Frontend must not independently determine escrow status.

---

## 41. Google Meet Scheduling

After:

Auction closes
→ Winner verified
→ USDC escrow secured
→ Session locked

the backend creates:

Private Google Calendar event
+
Private Google Meet link

Then:

Meeting URL generated
→ Email sent to student
→ Email sent to mentor
→ Session becomes scheduled

---

## 42. Google Meet Privacy

The Google Meet URL MUST NOT appear publicly in:

- Marketplace
- Mentor profile
- Public auction page
- Public session cards
- Pre-auction screens

The link becomes visible only to authorized participants after the session is locked and scheduled.

---

## 43. Google Meet Session Card

Show:

SESSION SCHEDULED

Friday · 7:00 PM

Payment:

$60 USDC secured

Google Meet:

Private meeting

[ Join Google Meet ]

Below:

"Meeting link sent to your email."

Only display email-sent status when the backend confirms it.

---

## 44. Join Google Meet

Button:

[ Join Google Meet ]

Behavior:

- Open the actual meeting URL in a new tab.
- Do not generate a fake URL.
- Do not hardcode a fake meeting link.
- Do not expose the link before authorization.

---

## 45. Session Page

Route:

/student/sessions/[id]

Display:

Session

Distributed Systems Interview Preparation

Mentor

Alex Morgan

Student

Current User

Date

Friday · 7:00 PM

Duration

60 minutes

Payment

$60 USDC

Status

Scheduled

Show Google Meet card.

---

## 46. Session Completion

After the session show:

[ Confirm Session Completed ]

Clicking opens confirmation:

"Confirm session completion?"

"This will allow the escrowed payment to be released to the mentor."

Buttons:

[ Confirm Session Completed ]

[ Cancel ]

Flow:

Student confirmation
→ Backend validation
→ Session completion
→ Blockchain settlement
→ Payment release
→ Updated status

---

## 47. Payment Release

After successful settlement:

PAYMENT SETTLED

$60 USDC

✓ Escrow released

✓ Mentor paid

✓ Transaction confirmed

Optional:

[ View Transaction ]

Only show transaction details if the transaction is real.

---

## 48. Review

After completion:

"How was your mentorship session?"

Rating:

☆ ☆ ☆ ☆ ☆

Written review:

[ Share your experience... ]

Button:

[ Submit Review ]

Flow:

Validate review
→ Submit review
→ Update mentor reputation
→ Show success

---

## 49. Review Success

Show:

"Thanks for your review."

"Your feedback helps build mentor reputation."

Button:

[ Back to Dashboard ]

---

## 50. Mentor Reputation

Display real reputation data when available.

Example:

4.9 ★

42 completed sessions

38 reviews

Do not fabricate ratings or reviews in production.

---

## 51. Student Dashboard

Route:

/student/dashboard

Summary:

Active Bids

3

Upcoming Sessions

1

Completed Sessions

6

Total Spent

$240 USDC

Active bid card:

Distributed Systems Interview

Your Bid:

$60 USDC

Current Bid:

$62 USDC

Ends In:

00:48:20

[ View Auction ]

Upcoming session:

Friday · 7:00 PM

Payment:

$60 USDC secured

Status:

Scheduled

[ Join Google Meet ]

---

## 52. Mentor Dashboard

Route:

/mentor/dashboard

Summary:

Active Auctions

3

Upcoming Sessions

2

Total Earned

$820 USDC

Average Rating

4.9 ★

Active auction:

System Design Mentorship

Starting Price:

$40 USDC

Current Bid:

$57 USDC

Bidders:

8

[ View Auction ]

Upcoming session:

Friday · 7:00 PM

Escrow:

$60 USDC secured

Meet:

Scheduled

---

## 53. Navigation — Logged Out

Keep navigation small:

MentorX

Explore

How It Works

Find a Mentor

Become a Mentor

Login

---

## 54. Navigation — Student

MentorX

Discover

Marketplace

My Sessions

Wallet

Profile

---

## 55. Navigation — Mentor

MentorX

Dashboard

My Sessions

Marketplace

Create Session

Earnings

Profile

Do not overload navigation.

---

## 56. Button Inventory

Use meaningful buttons only.

Primary actions:

1. Find a Mentor
2. Become a Mentor
3. Continue as Student
4. Continue as Mentor
5. Complete Registration
6. Discover Mentors
7. Analyze My Expertise
8. Accept AI Price
9. Edit Price
10. Save Price
11. Create Mentorship Session
12. Publish Session
13. Find My Mentor
14. View Auction
15. Connect Wallet
16. Place Bid
17. Confirm Bid
18. Join Google Meet
19. Confirm Session Completed
20. Submit Review

Secondary actions:

21. View Mentor
22. Enter Marketplace
23. Upload CV
24. Replace
25. Remove
26. Add GitHub
27. Add LinkedIn
28. Add Portfolio
29. Add Certification
30. View Marketplace
31. View Transaction
32. Back
33. Retry
34. Close
35. Login
36. Logout
37. Cancel
38. Retry Transaction

Do not create unnecessary buttons just to fill the UI.

---

## 57. Button States

Every asynchronous action must support appropriate states:

Default

Hover

Active

Disabled

Loading

Success

Error

Example:

[ Analyze My Expertise ]

then:

[ Analyzing... ]

then:

[ Analysis Complete ]

Never allow duplicate submissions while an action is processing.

---

## 58. Primary Buttons

Primary buttons should visually stand out.

Important primary actions:

- Find a Mentor
- Analyze My Expertise
- Publish Session
- Place Bid
- Confirm Bid
- Join Google Meet

---

## 59. Secondary Buttons

Use secondary styling for:

- View Mentor
- Edit Price
- Add GitHub
- View Auction
- View Session
- Back
- Cancel

---

## 60. Destructive Actions

Actions such as:

Remove CV

Remove Resource

must require confirmation.

Example:

"Remove this resource?"

[ Remove ]

[ Cancel ]

---

## 61. Design System

MentorX should feel:

- Premium
- Professional
- Modern
- Intelligent
- Trustworthy
- Simple
- Fast

Use:

- Clean spacing
- Strong typography
- Subtle borders
- Restrained shadows
- Clear hierarchy
- Professional cards
- Strong empty states
- Good loading states

Avoid visual clutter.

---

## 62. Color System

Use a restrained palette:

- Neutral background
- Neutral surfaces
- One strong brand color
- Semantic success
- Semantic warning
- Semantic error

Do not use many unrelated bright colors.

Do not make the interface look like a casino.

Do not make the interface look like a crypto trading terminal.

---

## 63. Typography

Use a clear hierarchy:

Page title

Section title

Card title

Body

Secondary text

Metadata

Prices should be visually prominent.

Important auction numbers should be easy to scan.

---

## 64. AI Visual Language

Use subtle AI indicators:

✦ AI Insight

✦ AI Recommendation

✦ AI Match

AI should feel embedded into the product.

Do not turn MentorX into a chatbot.

The marketplace remains the core experience.

---

## 65. Financial Visual Language

Make financial states extremely clear.

Example:

AI Recommended

$40 USDC

Current Market

$57 USDC

Winning Bid

$60 USDC

Escrow

$60 USDC

Released

$60 USDC

Users must understand what each number represents.

---

## 66. Blockchain Visual Language

Blockchain should be visible but simple.

Use:

✓ Bid confirmed

✓ Escrow secured

✓ Payment released

Optional:

Transaction

0x12...89AB

[ View Transaction ]

Only show real blockchain information.

Never show fake transaction hashes.

---

## 67. Loading States

Every asynchronous feature requires loading UI.

Registration:

"Creating your profile..."

Marketplace:

"Loading mentorship sessions..."

AI:

"Analyzing expertise..."

Auction:

"Loading live auction..."

Bid:

"Confirming your bid..."

Escrow:

"Securing payment..."

Meeting:

"Scheduling private session..."

---

## 68. Error States

Never expose raw backend errors.

Use friendly messages.

Example:

"Something went wrong."

"We couldn't complete this action."

[ Try Again ]

AI error:

"We couldn't complete the expertise analysis."

"Check your resources and try again."

[ Retry Analysis ]

Wallet error:

"Wallet transaction was not completed."

[ Try Again ]

---

## 69. Empty States

No active bids:

"You don't have any active bids."

"Find a mentorship session and place your first bid."

[ Discover Mentors ]

No sessions:

"No upcoming sessions."

[ Find a Mentor ]

No mentor resources:

"Add professional resources so AI can understand your expertise."

[ Upload CV ]

---

## 70. Security

Never put these in frontend code:

- Private keys
- Seed phrases
- AWS credentials
- Database credentials
- Backend secrets
- Private API keys
- Google service-account secrets

Never expose:

- Private CV contents
- Private documents
- Private Google Meet URLs
- Sensitive personal information

---

## 71. Data Responsibilities

Frontend is responsible for:

- UI
- Forms
- Navigation
- Graph rendering
- Loading states
- Error states
- Wallet interaction
- Displaying backend/blockchain state

Backend is responsible for:

- Authentication
- Authorization
- AI analysis
- AI matching
- AI pricing
- Database
- Auction validation
- Winner verification
- Google Meet creation
- Email delivery
- Business logic

Blockchain is responsible for:

- USDC
- Bids where implemented onchain
- Escrow
- Settlement
- Payment release

The Graph can provide:

- Auction history
- Bid history
- Transaction history
- Settlement history
- Blockchain reputation data

---

## 72. Frontend Must Never Decide

Frontend must NEVER independently decide:

- Auction winner
- Auction end
- Bid validity
- Escrow funded
- Payment released
- Blockchain confirmation
- Session completed

These must come from authoritative backend/blockchain state.

---

## 73. Mock Data

Mock data is allowed during development.

However:

- Keep mocks isolated.
- Clearly separate mock services from real services.
- Never mix fake financial state with real blockchain state.
- Never fake transaction confirmation.
- Never fake escrow confirmation.
- Never claim email was sent if it was not.
- Never claim Google Meet was created if it was not.
- Never present fake AI analysis as real AI analysis.

Recommended service separation:

services/
├── auctionService.ts
├── mockAuctionService.ts
└── arcAuctionService.ts

The UI should interact with a clean service interface.

---

## 74. API Layer

Preferred structure:

lib/
└── api/
    ├── mentors.ts
    ├── students.ts
    ├── auctions.ts
    ├── sessions.ts
    ├── reviews.ts
    └── users.ts

Potential functions:

getMentors()

getMentor(id)

searchMentors(query)

createMentorProfile(data)

analyzeMentor(id)

createAuction(data)

getAuction(id)

placeBid(id, amount)

getSession(id)

completeSession(id)

submitReview(id, data)

Use actual backend contracts when available.

Do not invent incompatible APIs.

---

## 75. Component Structure

Prefer reusable components:

components/
├── ui/
│   ├── Button
│   ├── Card
│   ├── Badge
│   ├── Input
│   ├── Modal
│   ├── Tabs
│   ├── Toast
│   └── Loader
├── mentor/
│   ├── MentorCard
│   ├── MentorProfile
│   ├── SkillList
│   ├── MatchScore
│   ├── MentorAnalysis
│   └── ResourceUpload
├── auction/
│   ├── AuctionCard
│   ├── AuctionHeader
│   ├── AuctionStats
│   ├── BidHistory
│   ├── BidForm
│   ├── BidChart
│   ├── Countdown
│   └── EscrowStatus
├── session/
│   ├── SessionCard
│   ├── SessionDetails
│   ├── SessionStatus
│   └── MeetingCard
├── wallet/
│   ├── WalletButton
│   ├── TransactionStatus
│   └── PaymentStatus
└── layout/
    ├── Navbar
    ├── Footer
    └── PageContainer

Reuse existing components if they already exist.

Do not create duplicate versions of the same component.

---

## 76. Responsive Design

Desktop is primary.

All major UI must remain usable on smaller screens.

Pay special attention to:

- Auction graph
- Auction statistics
- Bid form
- Mentor profile
- Dashboard cards
- Resource upload
- AI analysis
- Session information

Do not allow important information to disappear on smaller screens.

---

## 77. Accessibility

Use:

- Semantic HTML
- Accessible labels
- Keyboard navigation
- Visible focus states
- Correct button semantics
- Form validation
- Meaningful error messages
- Sufficient color contrast

Do not communicate state using color alone.

---

## 78. Performance

Keep the application fast.

Avoid:

- Unnecessary API requests
- Unnecessary re-renders
- Huge bundles
- Huge images
- Unnecessary dependencies

Use appropriate:

- Lazy loading
- Image optimization
- Caching
- Pagination
- Server-side rendering where useful

Do not over-engineer performance.

---

## 79. Real-Time Auction Experience

When auction data changes, update:

- Current bid
- Bid count
- Bid history
- Graph
- Countdown/state

If real-time infrastructure exists, use it.

Otherwise use controlled polling.

Do not aggressively poll unnecessarily.

---

## 80. AI Data Model

AI output should be structured.

Conceptually:

type MentorAIAnalysis = {
  summary: string;

  skills: {
    name: string;
    confidence?: number;
    evidence?: string[];
  }[];

  recommendedHourlyPrice: number;

  recommendedPriceRange?: {
    min: number;
    max: number;
  };

  reasoning: string;
};

Use the actual backend schema when available.

Do not invent incompatible frontend fields.

---

## 81. Mentor Data Model

Conceptually:

type Mentor = {
  id: string;
  name: string;
  title: string;
  company?: string;
  yearsExperience?: number;
  domain: string;
  skills: string[];
  rating?: number;
  completedSessions?: number;
  avatarUrl?: string;
};

Adapt to the actual backend schema.

---

## 82. Auction Data Model

Conceptually:

type Auction = {
  id: string;
  mentorId: string;
  sessionId: string;
  startingPrice: number;
  currentBid: number;
  bidCount: number;
  endsAt: string;
  status: string;
};

Adapt to actual backend/blockchain schema.

---

## 83. Session Data Model

Conceptually:

type Session = {
  id: string;
  mentorId: string;
  studentId?: string;
  topic: string;
  description?: string;
  startsAt: string;
  durationMinutes: number;
  price: number;
  status: string;
  meetingUrl?: string;
};

The meeting URL must only be returned to authorized participants after scheduling.

---

## 84. Authentication

Use the project's actual authentication solution.

After authentication:

User
→ Determine role
→ Load profile
→ Redirect to correct dashboard/discovery

Do not rely only on frontend role state for authorization.

Backend must enforce authorization.

---

## 85. Student Access

Student pages such as:

/student/*

must require student authorization.

If a mentor tries to access student-only functionality, show a friendly access message.

Example:

"Access restricted."

[ Go to Dashboard ]

Backend authorization remains authoritative.

---

## 86. Mentor Access

Mentor pages such as:

/mentor/*

must require mentor authorization.

Do not rely only on hidden navigation.

---

## 87. Mentor Profile Completion

Mentors should not publish a session until required information is available.

Potential requirements:

✓ Name

✓ Professional title

✓ Domain

✓ Skills

✓ Resource

✓ AI analysis

✓ Starting price

Where useful, show:

Profile completeness

████████░░ 80%

Only enforce fields actually required by the backend.

---

## 88. Mentor Publishing Flow

Recommended flow:

Create Profile
→ Upload Resources
→ AI Analysis
→ AI Price Recommendation
→ Accept/Edit Starting Price
→ Create Session
→ Review Session
→ Publish

The UI should make this progression clear.

---

## 89. Student Discovery Flow

Recommended flow:

Student Profile
→ Professional Domain
→ Learning Goal
→ Natural Language Query
→ AI Matching
→ Recommended Mentors
→ Mentor Profile
→ Available Auctions

---

## 90. Auction Flow

Recommended flow:

Marketplace
→ Auction
→ AI Starting Price
→ Current Market Price
→ Bid Graph
→ Bid History
→ Place Bid
→ Wallet
→ Arc
→ Bid Confirmed

---

## 91. Winner Flow

Auction Ends
→ Winner Determined
→ Winner Screen
→ USDC Escrow
→ Payment Confirmed
→ Session Locked

---

## 92. Session Flow

Session Locked
→ Google Calendar Event
→ Private Google Meet
→ Email Student
→ Email Mentor
→ Scheduled Session
→ Join Meeting
→ Session Completed
→ Payment Released
→ Review

---

## 93. AWS Architecture

Expected backend architecture:

Frontend
→ API Gateway
→ Lambda
→ DynamoDB

Lambda
→ S3

Lambda
→ Amazon Bedrock
→ Strands Agent

Frontend must never expose AWS credentials.

---

## 94. Blockchain Architecture

Expected flow:

Frontend
→ Privy Wallet
→ Arc
→ Smart Contract
→ USDC
→ Escrow
→ Settlement

Frontend triggers wallet interactions but does not control financial truth.

---

## 95. The Graph

The Graph can provide indexed blockchain information such as:

- Auction history
- Bid history
- Transaction history
- Settlement history
- Reputation-related blockchain data

Frontend can consume backend APIs that abstract The Graph.

Do not couple every UI component directly to blockchain indexing.

---

## 96. Main Hackathon Demo

The frontend must support this complete demo:

1. Student registers.
2. Student selects Software Engineering.
3. Student enters System Design Interview Preparation.
4. Mentor registers.
5. Mentor uploads CV.
6. Mentor adds GitHub.
7. Mentor adds LinkedIn.
8. Mentor adds Portfolio.
9. Mentor clicks Analyze My Expertise.
10. AI analyzes mentor.
11. AI recommends $40 USDC/hour.
12. Mentor accepts $40.
13. Mentor creates Friday 7 PM, 60-minute session.
14. Mentor publishes session.
15. Session appears in marketplace.
16. Student receives AI recommendation.
17. Student opens auction.
18. AI starting price is $40 USDC.
19. Bids happen:
   $42
   $47
   $52
   $60
20. Graph rises.
21. Auction closes.
22. Winner is confirmed.
23. $60 USDC enters escrow.
24. Private Google Meet is created.
25. Student receives email.
26. Mentor receives email.
27. Student sees Join Google Meet.
28. Session happens.
29. Student confirms completion.
30. $60 USDC is released.
31. Student submits review.
32. Mentor reputation updates.

This is the primary hackathon demonstration.

---

## 97. Complete Architecture Diagram

MentorX frontend:

Frontend
│
├── Student Experience
│   ├── Registration
│   ├── Discovery
│   ├── Marketplace
│   ├── Auctions
│   ├── Bidding
│   ├── Sessions
│   └── Reviews
│
├── Mentor Experience
│   ├── Registration
│   ├── Profile
│   ├── Resources
│   ├── AI Analysis
│   ├── Price Recommendation
│   ├── Session Creation
│   └── Earnings
│
└── Shared Experience
    ├── Authentication
    ├── Wallet
    ├── Notifications
    ├── Navigation
    └── UI Components

Frontend
↓
API Gateway
↓
AWS Lambda
├── DynamoDB
├── S3
└── Amazon Bedrock
    ↓
Strands Agent

Frontend
↓
Privy
↓
Arc
├── Auctions
├── Bids
├── USDC
├── Escrow
└── Settlement

Blockchain
↓
The Graph
↓
Backend/API
↓
Frontend

Backend
↓
Google Calendar / Google Meet
↓
Email
↓
Student + Mentor

---

## 98. Product Story

The complete MentorX story is:

Professional expertise is scarce.

Mentor time is limited.

AI understands mentor expertise.

AI predicts an initial market value.

Mentors create scarce sessions.

Students compete for limited sessions.

The market discovers the actual price.

Blockchain protects the payment.

The session happens privately.

The mentor gets paid.

The student reviews the mentor.

The mentor reputation grows.

---

## 99. Important Product Distinctions

Always distinguish:

AI Recommended Price
≠
Mentor Starting Price
≠
Current Market Price
≠
Winning Bid
≠
Escrow Amount
≠
Released Amount

Example:

AI Recommended Price:
$40 USDC

Mentor Starting Price:
$40 USDC

Current Market Price:
$57 USDC

Winning Bid:
$60 USDC

Escrow Amount:
$60 USDC

Released Amount:
$60 USDC

---

## 100. Final Implementation Rules

Claude MUST follow all of these rules.

1. Read this entire file before implementing frontend changes.

2. Inspect the existing frontend before creating new files.

3. Reuse existing components whenever possible.

4. Do not create duplicate components.

5. Do not introduce unnecessary libraries.

6. Do not add features outside this specification unless explicitly requested.

7. Prioritize the P0 end-to-end flow.

8. Never fake AI analysis.

9. Never fake AI recommendations.

10. Never fake AI matching.

11. Never fake blockchain transactions.

12. Never fake transaction hashes.

13. Never fake bid confirmations.

14. Never fake escrow.

15. Never fake payment release.

16. Never fake Google Meet creation.

17. Never fake email delivery.

18. Never expose private CVs.

19. Never expose private documents.

20. Never expose private Google Meet links publicly.

21. Never expose private keys.

22. Never expose seed phrases.

23. Never expose AWS credentials.

24. Never expose backend secrets.

25. Never store private CV contents onchain.

26. Never allow frontend code to determine the auction winner.

27. Never allow frontend code to determine final escrow state.

28. Never allow frontend code to determine payment release.

29. Backend/blockchain must remain authoritative for financial state.

30. AI recommendations must be evidence-based.

31. AI must not fabricate professional experience.

32. AI must not fabricate skills.

33. AI must not fabricate certifications.

34. AI must not fabricate projects.

35. AI must not fabricate match scores.

36. Always display financial values with USDC.

37. Do not use unsafe floating-point arithmetic for real financial values.

38. Use strong TypeScript types.

39. Avoid unnecessary `any`.

40. Every asynchronous operation must have loading, success, error, and retry states where appropriate.

41. Never expose raw backend errors.

42. Keep mock services isolated from production services.

43. Do not mix fake financial state with real blockchain state.

44. Do not claim an external action succeeded unless the system confirms it.

45. Keep the UI professional and consistent.

46. Keep navigation simple.

47. Keep the marketplace as the core product experience.

48. Keep AI embedded into the product rather than turning the application into a chatbot.

49. Keep blockchain visible enough to establish trust but simple enough for normal users.

50. Keep the auction experience visually strong because it is a key product differentiator.

51. Make the AI price versus market price comparison obvious.

52. Make the scarcity of mentor time obvious.

53. Make the USDC escrow state obvious.

54. Make the private Google Meet transition obvious.

55. Make the final mentor payment obvious.

56. Make reputation growth obvious.

57. Preserve existing working functionality.

58. Do not rewrite working architecture unnecessarily.

59. Run type checking after implementation.

60. Run linting after implementation.

61. Run relevant tests after implementation.

62. Run the production build after implementation.

63. Check existing functionality after implementation.

64. Fix regressions before finishing.

---

# Final Success Criterion

Do not optimize MentorX for the number of pages.

Do not optimize MentorX for the number of components.

Do not optimize MentorX for the number of features.

Optimize MentorX for one complete, polished, believable end-to-end journey:

Register
→ AI Analysis
→ AI Matching
→ Marketplace
→ Auction
→ Bid
→ Win
→ USDC Escrow
→ Private Google Meet
→ Session
→ Settlement
→ Review
→ Reputation

A new user should understand MentorX within approximately 30 seconds.

The visual story should be:

AI finds the right mentor
↓
AI estimates mentor value
↓
Mentor creates scarce session
↓
Students compete for the session
↓
Market price changes
↓
Winner selected
↓
USDC secured
↓
Private Google Meet scheduled
↓
Session happens
↓
Mentor gets paid
↓
Reputation increases

MentorX should feel like a premium professional marketplace where AI helps discover and price expertise while blockchain makes the transaction trustworthy.

The final frontend must be:

BEAUTIFUL
+
SIMPLE
+
FAST
+
TRUSTWORTHY
+
AI-POWERED
+
MARKETPLACE-DRIVEN
+
BLOCKCHAIN-SECURED

That complete journey is the MentorX frontend.
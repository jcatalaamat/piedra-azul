# Technical Architecture - Piedra Azul Ecosystem

**Complete Technical Stack & Integration Strategy**

---

## Executive Summary

**Philosophy:** Build once, integrate everywhere. Shared infrastructure across all 15 projects.

**Core Technology Decisions:**
- **Frontend:** React (web) + React Native (mobile) for code sharing
- **Backend:** Node.js + Supabase (PostgreSQL, real-time, auth, storage)
- **AI:** GPT-4/Claude for intelligence, vector databases for memory
- **Web3:** Arbitrum (L2 for low fees), Unlock Protocol (NFTs), IPFS (storage)
- **Infrastructure:** AWS + Vercel + Railway for deployment
- **Mobile:** Expo (React Native) for cross-platform iOS/Android

**Integration Layer:** All projects connect through shared APIs, authentication, token economy, and data models.

---

## I. Technology Stack Overview

### Frontend Technologies

**Web Applications:**
- **React 18/19** (latest)
- **TypeScript** (strict mode)
- **Vite** (fast builds vs webpack)
- **Tailwind CSS** (utility-first styling)
- **React Router** (client-side routing)
- **React Query** (server state management)
- **Framer Motion** (animations where needed)

**Mobile Applications:**
- **React Native 0.76+**
- **Expo 52+** (managed workflow for faster development)
- **Tamagui** (cross-platform UI components, web + native)
- **TypeScript**
- **Expo Router** (file-based routing)
- **React Query**
- **Reanimated** (performant animations)

**Why This Stack:**
- Maximum code sharing (web + mobile share 70-80%)
- Proven at scale (used by Facebook, Airbnb, others)
- Large talent pool for hiring
- Fast iteration and deployment
- Modern DX (developer experience)

---

### Backend & Database

**Primary Backend: Supabase**
- **PostgreSQL** (robust, relational database)
- **Real-time subscriptions** (via PostgreSQL LISTEN/NOTIFY)
- **Row-level security** (RLS) for data privacy
- **Built-in authentication** (email, social OAuth, magic links)
- **Storage** for files and media
- **Edge Functions** (Deno) for serverless logic
- **PostgREST** API (auto-generated from schema)

**Why Supabase:**
- Open source (not vendor locked)
- PostgreSQL (battle-tested, not NoSQL limitations)
- Real-time out of the box
- Auth included (saves weeks of development)
- Generous free tier + reasonable pricing
- Self-hostable if needed

**Alternative/Complementary:**
- **Firebase** (used in some existing projects, will migrate)
- **Node.js + Express** (custom APIs where needed)
- **GraphQL** (for complex queries, optional)

---

### AI & Machine Learning

**Large Language Models:**
- **Primary: Claude (Anthropic)** - longer context, better at reasoning
- **Secondary: GPT-4 (OpenAI)** - when Claude unavailable
- **Use Cases:** LIFE OS AI mirror, Community AI Platform insights, content generation

**Vector Databases:**
- **Primary: Supabase pgvector** (PostgreSQL extension)
- **Alternative: Pinecone, Weaviate** (if scale demands)
- **Use Cases:** Semantic search, memory, pattern recognition

**Frameworks:**
- **LangChain** (orchestration, chains, agents)
- **LlamaIndex** (RAG - Retrieval Augmented Generation)
- **Custom fine-tuning** (as budget allows)

**AI Architecture Pattern:**
- User input → Embedding → Vector search (relevant context) → LLM with context → Response
- Continuous learning: Feedback loop improves responses

---

### Blockchain & Web3

**Blockchain: Arbitrum One (Ethereum L2)**
- Low transaction fees ($0.01-0.10 vs $5-50 on mainnet)
- Fast finality (seconds)
- Ethereum security (inherits from mainnet)
- Large ecosystem and tooling

**Smart Contracts:**
- **Solidity** (primary language)
- **Hardhat** (development environment)
- **OpenZeppelin** (secure contract templates)
- **Ethers.js** (JavaScript library for interaction)

**Key Contracts:**
- **ERC-1155 NFTs** (Proyecto Salvaje, Global Ecovillage memberships)
- **Governance** (Colony.io for DAO, Snapshot for voting)
- **TIERRA Token** (ERC-20, utility and governance)

**Why Arbitrum:**
- User experience (fast, cheap)
- Security (Ethereum backing)
- Ecosystem (many projects, good tooling)
- Bridge to mainnet (liquidity when needed)

**Alternatives Considered:**
- **Polygon:** Similar, but less decentralized
- **Solana:** Faster but less proven security
- **Optimism:** Similar to Arbitrum, chose Arbitrum for ecosystem

---

### Infrastructure & DevOps

**Hosting:**
- **Supabase Cloud** (database, auth, storage)
- **Vercel** (web apps, Next.js optimized, edge functions)
- **Railway/Render** (Node.js backends, long-running processes)
- **AWS** (when complex infrastructure needed: S3, Lambda, EC2)
- **Expo Application Services (EAS)** (mobile builds and updates)

**CI/CD:**
- **GitHub Actions** (automated testing, builds, deployments)
- **EAS Build** (mobile app builds for iOS/Android)
- **Vercel** (auto-deploy on git push)

**Monitoring:**
- **Sentry** (error tracking)
- **PostHog** (product analytics, feature flags)
- **Supabase Dashboard** (database performance, logs)
- **Vercel Analytics** (web performance)

**Why This Stack:**
- Mostly serverless (scales automatically, pay-per-use)
- Modern DevOps (fast deployments, no server management)
- Cost-effective (free tiers generous, then pay-as-you-go)
- Developer-friendly (great DX)

---

## II. Core Systems Architecture

### Authentication & Identity

**Single Sign-On (SSO) Across All Projects:**
- Supabase Auth as identity provider
- JWT tokens for session management
- OAuth providers: Google, Apple, GitHub, email/password
- Magic links for passwordless auth

**User Data Model:**
```
users (Supabase auth.users)
├── profiles (public profile data)
│   ├── display_name, avatar_url, bio
│   └── preferences, settings
├── life_os_data (LIFE OS specific)
│   ├── dimensions tracking
│   ├── journal entries
│   └── AI insights
├── ecovillage_memberships (Global Ecovillage)
│   ├── tier, NFT_id, status
│   └── hub bookings
├── community_roles (Community AI)
│   └── community memberships, permissions
└── wallet_address (Web3 integration)
```

**Security:**
- Row-level security (RLS) ensures users only access their data
- End-to-end encryption for sensitive data (journals, health)
- GDPR and CCPA compliant
- Regular security audits

---

### Integration Layer (The Nervous System)

**API Gateway:**
- Central API connecting all projects
- Shared schemas and types (TypeScript)
- Rate limiting and auth middleware
- GraphQL layer for complex queries (optional)

**Event Bus (Real-Time Events):**
- Supabase real-time (WebSockets)
- Event-driven architecture
- Example: User completes Inner Ascend level → LIFE OS updates growth dimension → Community AI tracks progress

**Shared Services:**
1. **Auth Service** (login/logout, token refresh)
2. **Payment Service** (Stripe, crypto wallet)
3. **Notification Service** (email, push, in-app)
4. **Storage Service** (images, videos, files)
5. **AI Service** (LLM calls, embeddings, vector search)

**Data Sync:**
- Offline-first architecture (React Query, Expo SQLite)
- Conflict resolution (last-write-wins with timestamps)
- Background sync when connection restored

---

### LIFE OS Technical Architecture

**Components:**

**1. AI Mirror (Core Intelligence)**
- User journaling → Embeddings → Vector DB
- Retrieval of relevant past entries (RAG)
- LLM generates insights, asks questions, recognizes patterns
- Continuous learning (feedback loop)

**2. 8 Dimensions Tracking**
```
Dimensions:
├── Mind (clarity, focus, learning)
├── Heart (emotional states, regulation)
├── Body (energy, health, movement)
├── Spirit (presence, connection, meaning)
├── Purpose (alignment, contribution)
├── Flow (productivity, ease, creativity)
├── Growth (transformation, challenges, integration)
└── Relations (connections, quality, boundaries)
```

**3. Practice Engine**
- Personalized recommendations based on dimensions
- Library of 1,000+ practices (meditation, somatic, shadow work, journaling prompts)
- Progress tracking and streaks

**4. Integration Hub**
- Connect to wearables (Oura, Whoop, Apple Health) via APIs
- Connect to other projects (Inner Ascend, Global Ecovillage bookings, Community events)
- Unified dashboard showing whole life

**Technical Stack:**
- Mobile: React Native + Expo
- Web: React + Vite
- Backend: Supabase (data) + Edge Functions (AI orchestration)
- AI: Claude API + pgvector (Supabase)
- Real-time: Supabase subscriptions

**Scalability:**
- AI calls: Batch and cache common patterns
- Vector search: Index optimization, sharding if needed
- Database: Read replicas for scaling
- CDN: Cached static assets

---

### Global Ecovillage Network Technical Architecture

**Components:**

**1. Hub Management System**
- Hub profiles (location, amenities, photos, capacity)
- Availability calendar (real-time)
- Pricing by tier (dynamic)
- Rules and governance docs

**2. Booking & Reservation System**
- Member searches hubs by location, dates, amenities
- Real-time availability check
- Booking request → Hub approval (automatic or manual)
- Calendar sync (Google Calendar, iCal export)
- Payments (Stripe for fiat, crypto wallet for tokens)

**3. NFT Membership System**
- Smart contracts (ERC-1155 on Arbitrum)
- Tiers: Explorer, Nomad, Steward, Founding Steward
- Unlock Protocol integration (membership gates)
- Wallet connection (WalletConnect, MetaMask)
- Tiered access rights (booking priority, governance votes)

**4. DAO Governance**
- Snapshot for off-chain voting (proposals)
- Colony.io for on-chain treasury management
- Multi-sig for major decisions
- Transparent financials (all on-chain)

**5. Marketplace (Phase 2)**
- Services listing (transportation, food, experiences)
- Provider profiles and ratings
- Booking and payments
- Commission tracking (5-10%)

**Technical Stack:**
- Mobile: React Native + Expo
- Web: React + Vite + Tailwind
- Backend: Supabase (data) + Node.js (complex logic)
- Blockchain: Arbitrum + Solidity smart contracts + Ethers.js
- Payments: Stripe + crypto wallets
- Maps: Google Maps API, Mapbox

**Scalability:**
- Database: Partition by region (US, EU, LATAM, ASIA)
- Caching: Redis for availability, pricing
- CDN: Images and static assets
- Microservices: Booking engine separate if needed

---

### Community AI Platform Technical Architecture

**Components:**

**1. Community Dashboard**
- Real-time metrics (7 dimensions: personal, community, land, education, governance, economic, formation)
- Visualizations (charts, graphs, heatmaps)
- Alerts and recommendations

**2. AI Intelligence Engine**
- Data ingestion: Manual inputs, sensor data (IoT), calendar events, financial transactions
- Analysis: Pattern recognition, anomaly detection, predictive modeling
- Insights: "Community energy low this week, suggest group gathering" or "Governance participation declining, needs attention"

**3. Formation Support Toolkit**
- Templates: Legal structures, governance frameworks, financial models
- Guides: Land acquisition, crowdfunding, member recruitment
- Mentor matching: Connect forming communities with experienced founders

**4. Integration APIs**
- Connect to existing tools (Slack, Discord, Google Workspace, Notion)
- Bi-directional sync
- Webhook listeners for real-time data

**Technical Stack:**
- Web: React + Vite + Recharts (data viz)
- Backend: Node.js + Supabase
- AI: Claude + custom models + pgvector
- Data Pipeline: ETL processes (Extract, Transform, Load)
- APIs: RESTful + GraphQL

**Scalability:**
- Multi-tenant architecture (each community isolated)
- Background jobs (AI analysis runs async)
- Data warehouse (for historical analysis)
- Caching (computed insights cached, refresh hourly)

---

### Token Economy (TIERRA) Technical Architecture

**Components:**

**1. TIERRA Token (ERC-20)**
- Utility: Payments across ecosystem (LIFE OS, Global Ecovillage, Marketplace, Inner Ascend)
- Governance: Voting rights in Proyecto Salvaje and ecosystem decisions
- Rewards: Incentives for participation (volunteering, contributions)

**2. Wallet Integration**
- Built-in wallets (Expo-compatible, React Native)
- WalletConnect support (external wallets)
- Fiat on-ramps (buy crypto with credit card via Stripe + crypto provider)
- Simple UX (abstract blockchain complexity)

**3. Payment Processing**
- Accept TIERRA in all apps
- Automatic conversion to USD equivalent (oracle price feeds)
- Settlement in TIERRA or fiat (merchant choice)
- Low fees (1-2% vs 20-30% Airbnb/Uber)

**4. Tokenomics Dashboard**
- Token circulation visualizations
- Burn/mint tracking (if deflationary/inflationary)
- Governance proposals and voting
- Rewards distribution

**Technical Stack:**
- Blockchain: Arbitrum (TIERRA deployed as ERC-20)
- Smart Contracts: Solidity
- Wallet: React Native crypto libraries (ethers.js, web3.js)
- Backend: Node.js (track transactions, update balances)
- Price Oracles: Chainlink (if needed for price feeds)

**Scalability:**
- Layer 2 (Arbitrum) handles high throughput
- Batch transactions for gas efficiency
- Off-chain payments with on-chain settlement (if needed)

---

## III. Security & Privacy

### Data Protection

**Encryption:**
- At rest: AES-256 (Supabase default)
- In transit: TLS 1.3 (HTTPS everywhere)
- End-to-end: Sensitive data (journals, health) encrypted client-side

**Privacy Principles:**
1. **Data Minimization:** Collect only what's needed
2. **User Control:** Export, delete, modify data anytime
3. **Transparency:** Clear privacy policy, data usage explained
4. **No Selling:** User data never sold to third parties
5. **Anonymization:** Analytics and AI training use anonymized data

**Compliance:**
- **GDPR** (EU): Right to access, delete, portability
- **CCPA** (California): Similar rights
- **HIPAA** (if health data): Business Associate Agreement if needed

---

### Authentication Security

**Best Practices:**
- Passwords hashed (bcrypt)
- JWT tokens (short-lived, refresh tokens)
- Rate limiting (prevent brute force)
- 2FA option (TOTP via authenticator apps)
- Session management (logout on all devices)
- Suspicious activity alerts

---

### Smart Contract Security

**Process:**
- **OpenZeppelin templates** (battle-tested)
- **Audits:** Third-party audit before mainnet deployment (Quantstamp, Trail of Bits, others)
- **Test Coverage:** 100% on critical paths
- **Multi-sig:** Major actions require multiple approvals
- **Upgradability:** Proxy patterns for bug fixes (carefully managed)

**Common Vulnerabilities Prevented:**
- Reentrancy attacks (checks-effects-interactions pattern)
- Integer overflow (Solidity 0.8+ safe math)
- Access control (OpenZeppelin AccessControl)
- Front-running (commit-reveal where needed)

---

## IV. Performance & Scalability

### Current Scale (Year 1)

- **Users:** 10K-50K across all projects
- **Requests:** 1M-10M API requests/month
- **Database:** <100GB
- **Infrastructure:** Supabase Pro ($25/month) + Vercel Pro ($20/month)

**This Scales Easily to 100K Users**

---

### Future Scale (Year 3+)

- **Users:** 100K-1M
- **Requests:** 100M-1B/month
- **Database:** 1TB+
- **Infrastructure:** Supabase Team ($599/month) + Vercel Enterprise + AWS if needed

**Scaling Strategy:**

**Database:**
- Read replicas (horizontal scaling for reads)
- Connection pooling (PgBouncer)
- Query optimization (indexes, EXPLAIN ANALYZE)
- Sharding (if single database becomes bottleneck, partition by region/user)

**API:**
- CDN for static assets (Cloudflare, Vercel Edge)
- Caching (Redis, in-memory)
- Rate limiting per user/IP
- Microservices (split monolith if needed)

**AI:**
- Batch inference (group similar requests)
- Model caching (common responses)
- GPU instances (if fine-tuning models)
- OpenAI/Anthropic scale automatically

**Mobile:**
- Offline-first (less server dependency)
- Delta sync (only changed data)
- Image optimization and lazy loading

---

## V. Development Workflow

### Code Organization

**Monorepo Structure (Inspired by Mazunte Connect):**
```
piedra-azul/
├── apps/
│   ├── life-os-mobile/      (React Native)
│   ├── life-os-web/          (React + Vite)
│   ├── ecovillage-mobile/    (React Native)
│   ├── ecovillage-web/       (React + Vite)
│   ├── community-ai-web/     (React + Vite)
│   ├── inner-ascend-mobile/  (React Native)
│   └── [other apps]
├── packages/
│   ├── ui/                   (Shared Tamagui components)
│   ├── api/                  (Shared API client)
│   ├── auth/                 (Authentication logic)
│   ├── database/             (Supabase schemas, migrations)
│   ├── blockchain/           (Smart contracts, Web3 utils)
│   └── ai/                   (AI service wrappers)
├── services/
│   ├── api-gateway/          (Node.js central API)
│   ├── ai-service/           (LLM orchestration)
│   └── [other services]
└── docs/                     (Technical documentation)
```

**Benefits:**
- Code sharing (60-80% reuse across apps)
- Single source of truth (packages/)
- Consistent versioning
- Easier refactoring

---

### Development Process

**1. Local Development:**
- Docker Compose (Supabase local, Redis, etc.)
- Hot reload (Vite, Expo)
- Mock data and APIs
- Fast iteration

**2. Testing:**
- **Unit Tests:** Jest, React Testing Library
- **Integration Tests:** Playwright (web), Detox (mobile)
- **E2E Tests:** Critical user flows automated
- **CI:** Run on every PR (GitHub Actions)

**3. Staging:**
- Staging environment (mirrors production)
- QA testing before production
- Beta testers (early access)

**4. Production:**
- Blue-green deployments (zero downtime)
- Feature flags (gradual rollouts)
- Rollback ready (if issues)
- Monitoring alerts (Sentry)

---

### Release Cycle

**Web Apps:**
- Deploy on merge to main (continuous deployment)
- Instant updates (no user action needed)
- A/B testing with feature flags

**Mobile Apps:**
- EAS Update (OTA updates, instant for JS changes)
- App Store releases (monthly, for native changes)
- Beta testing (TestFlight, Google Play Beta)

---

## VI. AI Architecture Deep Dive

### LIFE OS AI Mirror

**Architecture:**
```
User Input (journal entry)
    ↓
Frontend (React Native)
    ↓
API Gateway (authentication)
    ↓
AI Service (Node.js)
    ↓
1. Embed entry (OpenAI embedding API)
2. Store in vector DB (Supabase pgvector)
3. Retrieve similar past entries (semantic search)
4. Construct prompt with context
5. Call LLM (Claude API)
6. Stream response to user
    ↓
Frontend (display response, store in DB)
```

**Prompt Engineering:**
- System prompt defines AI personality (compassionate, insightful, non-judgmental)
- Context includes: User profile, recent entries, patterns, 8 dimensions state
- Examples (few-shot learning) guide response style
- Constraints: Stay in scope, avoid medical advice, encourage professional help if needed

**Personalization:**
- AI learns user's language, communication style
- Adapts to user's journey stage (beginner vs advanced)
- Remembers key insights and references them

**Privacy:**
- User data never used to train general models (opt-out guaranteed)
- Option for local-only AI (future: on-device models for paranoid users)

---

### Community AI Platform Intelligence

**Architecture:**
```
Community Data Sources:
├── Manual Inputs (surveys, check-ins)
├── Calendar Events (gatherings, meetings)
├── Financial Transactions (costs, income)
├── Governance (proposals, votes)
├── Member Activity (engagement, participation)
└── External (weather, local events)
    ↓
Data Pipeline (ETL)
    ↓
Analysis Engine:
├── Pattern Recognition (trends, anomalies)
├── Predictive Modeling (forecasts)
└── Recommendation Engine (suggestions)
    ↓
Dashboard (visualizations, insights)
```

**Insights Examples:**
- "Community energy has dropped 20% in past month. Correlated with rainy season and fewer gatherings. Suggest: Indoor social event this weekend."
- "Governance participation is 30% this quarter, down from 60% last quarter. Identified: Meeting times conflict with member availability. Suggest: Poll for better times."
- "New members retention is 80% when paired with buddy, 40% without. Suggest: Assign buddy to all new members."

**Privacy:**
- Individual data anonymized (only aggregate patterns)
- Communities own their data (can export, delete)
- Insights never shared between communities without permission

---

## VII. Web3 Integration Details

### Smart Contract Architecture

**Proyecto Salvaje & Global Ecovillage NFTs:**
```solidity
contract PiedraAzulMembership {
    // ERC-1155 (multiple token types)
    uint256 constant EXPLORER = 1;
    uint256 constant NOMAD = 2;
    uint256 constant STEWARD = 3;
    uint256 constant FOUNDING_STEWARD = 4;

    // Membership data
    mapping(uint256 => Membership) public memberships;

    struct Membership {
        uint256 tier;
        uint256 nightsPerYear;
        uint256 governanceVotes;
        bool active;
    }

    // Booking logic
    mapping(address => Booking[]) public bookings;

    // Governance integration
    // Colony.io for treasury
    // Snapshot for voting
}
```

**TIERRA Token:**
```solidity
contract TIERRAToken is ERC20, Ownable {
    // Standard ERC-20

    // Utility functions
    function payForService(address provider, uint256 amount) external;

    // Governance functions
    function voteOnProposal(uint256 proposalId, bool support) external;

    // Rewards
    function rewardContribution(address contributor, uint256 amount) external onlyOwner;
}
```

**Security:**
- Audited before mainnet (Quantstamp, Trail of Bits, or similar)
- Multi-sig for admin functions
- Pausable in emergency
- Upgradeable (carefully, via proxy pattern)

---

### Wallet Integration

**User Experience:**
- **Option 1:** Built-in wallet (easiest, we custody)
- **Option 2:** WalletConnect (MetaMask, Rainbow, etc.)
- **Fiat On-Ramp:** Buy crypto with credit card (Stripe + MoonPay/Wyre)

**For Non-Crypto Users:**
- Abstract blockchain complexity
- "Pay with TIERRA" button (feels like PayPal)
- Auto-convert USD to TIERRA under the hood
- Gas fees abstracted (account abstraction or relay)

---

## VIII. Future Technical Roadmap

### Phase 1: Foundation (Months 0-6)
- LIFE OS MVP (web + mobile)
- Global Ecovillage booking system
- Proyecto Salvaje NFT contracts deployed
- Supabase schema and APIs
- Authentication SSO

### Phase 2: Integration (Months 6-12)
- Integration layer connecting all projects
- Community AI Platform pilot
- Token economy (TIERRA) launch
- AI Mirror v1 in LIFE OS
- Multi-hub booking operational

### Phase 3: Intelligence (Months 12-18)
- Advanced AI features (predictive insights)
- Community AI Dashboard v1
- Wearable integrations (Oura, Whoop)
- Marketplace v1
- DAO governance live

### Phase 4: Scale (Months 18-36)
- Performance optimizations
- International expansion (multi-region)
- Advanced AI (fine-tuned models)
- Marketplace v2 (full featured)
- Mobile app feature parity with web

---

## IX. Team & Hiring for Technical Roles

### Immediate Needs (Post-Seed)

**Senior Full-Stack Engineer:**
- React + React Native expertise
- TypeScript proficiency
- Supabase or Firebase experience
- Ship fast, iterate quickly

**Product Designer:**
- Mobile-first design
- User research and testing
- Design systems (Figma)
- Tamagui or similar experience

**Fractional Roles:**
- AI/ML Consultant (10 hrs/week) for LIFE OS and Community AI
- Smart Contract Auditor (project-based) for NFT contracts
- DevOps/Infrastructure (5 hrs/week) for scaling

### Year 2 Needs

- Backend Engineer (Node.js, Supabase, scaling)
- Mobile Engineer (React Native specialist)
- AI Engineer (full-time for advanced features)
- QA Engineer (testing, automation)

---

## X. Technical Risks & Mitigation

### Risk 1: AI Costs Spiral

**Mitigation:**
- Cache common responses
- Batch similar requests
- Fine-tune smaller models (cheaper)
- Hybrid: LLM for complex, rules for simple

### Risk 2: Smart Contract Bugs

**Mitigation:**
- Audits by reputable firms
- Test coverage 100% on critical paths
- Multi-sig for major actions
- Bug bounty program

### Risk 3: Scaling Bottlenecks

**Mitigation:**
- Architect for scale from day 1
- Monitor performance closely
- Horizontal scaling (add resources easily)
- Modular architecture (replace bottlenecks)

### Risk 4: Technology Changes

**Mitigation:**
- Don't over-invest in any single technology
- Abstract dependencies (can swap if needed)
- Stay current, continuous learning
- Pragmatic, not dogmatic

---

## Conclusion

**Technical Philosophy:**
- **Pragmatic over perfect:** Ship fast, iterate based on user feedback
- **Boring technology:** Proven stacks over bleeding edge (unless clear value)
- **Developer experience:** Happy developers = faster shipping
- **User experience:** Abstracts complexity, feels magical
- **Integration first:** Build once, use everywhere

**This architecture supports:**
- Rapid development (MVP in 6 months)
- Easy scaling (1K → 100K → 1M users)
- Low operational overhead (serverless, managed services)
- Team efficiency (monorepo, code sharing)
- Future flexibility (can swap technologies if needed)

**We're ready to build the operating system for regenerative living.**

---

*Last Updated: January 2026*

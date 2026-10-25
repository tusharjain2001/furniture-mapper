# Project Context: Home Renovation Course Website (MAPPER Method)

Handoff notes from the first planning chat (Oct 2026). Read this first before continuing.

## The client
- A home renovation education brand. They teach people to plan their own renovation (common in Western countries) instead of relying on a contractor. They also connect people with contractors when needed.
- The brand name may be "Map My Renovation" (heard in the Zoom call, not confirmed).
- The branding project is done and the style guide is approved. The website is the new project.
- Last year's website UI exploration and old sitemap are NOT approved, so ignore them or cross-check them against the brief. **The brief is the most up-to-date source.**
- Founder's ChatGPT screenshots and the masterclass pages (e.g. the bedroom revamp flow) are reference only, not approved designs.

## What they sell, in simple terms
Video courses on planning a renovation, sold as 3 **Planning Kits**:
- **Kitchen Kit**
- **Bathroom Kit**
- **Whole Home Kit**
(Bedroom was mentioned in the call but may come later.)

Every kit follows the same **6-step MAPPER method**:
1. **M**ap out your vision
2. **A**ssess your options
3. **P**rioritize based on budget and needs
4. **P**lan it out step by step
5. **E**xecute
6. **R**eassess

Each step contains:
- **One core video, the same across all kits**
- **Room-specific extra videos and links** (e.g. kitchen materials, bathroom materials)
- **A summary / checklist PDF** and their planner

## Required features
1. Buy a kit, then log in to a member area
2. Steps shown **in order**: finishing a video ticks it ✓ and unlocks the next one
3. **Locked vs. unlocked state**: non-buyers see an intro video, and the rest shows locked
4. **Upsell inside the member area**: show locked content with an "Upgrade" button (e.g. a Kitchen buyer sees an upgrade to Whole Home)
5. **Upgrades stay in the same workspace**: Kitchen → Whole Home unlocks the extra lessons in the same account and keeps progress (no separate product, no re-watching)
6. A progress tracker or dashboard per kit
7. One unified resource center
8. **Mobile first** (no app; users browse on phones)
9. **Scalable**: may add a dashboard or AI tools later

## Pages in scope
1. Homepage
2. Know Before Your Reno (a separate guide/checklist page in a set order)
3. MAPPER Method page
4. Planning Kit sales page (kit comparison + upgrade path)
5. Contact

After those: the member dashboard, step page template (reusable: video + summary + checklist + resources), and progress tracker.

## Client's own words (latest message)
- Interested in **MemberPress** because the core video per letter is shared across kits, with room-specific extras per kit.
- An upgrade from Kitchen → Whole Home should stay in the same workspace with more unlocked.
- If that's over budget, they're open to **Teachable** or another LMS (videos + PDF summaries + planner). They value simplicity and want it "clear and manageable".
- Wants it scalable for a future dashboard or AI tools.

## Voice note from the project lead (transcribed)
They need a platform for member login, step-by-step courses, completion ticks, sequential unlock, and upsells in the member area. **UI flexibility is a must** because their team designs the UI. They asked which platform to use and are open to other suggestions.

## Decision so far (recommended, not yet confirmed by the client)
**Custom build, NOT MemberPress / Teachable.**

| Part | Tool | Why |
|---|---|---|
| Website + member area | **Next.js (React)** | The developer knows React; full custom UI |
| Backend, DB, auth, **admin panel** | **Payload CMS** (open source, runs inside Next.js) | Auto-generated admin panel, so no custom admin portal to build (the client won't pay for one) |
| Payments, upgrades, refunds | **Stripe Checkout + webhook** | The Stripe dashboard handles orders and refunds |
| Video hosting | **Bunny Stream** (alternatives: Mux, Vimeo) | Cheap, mobile-friendly, domain lock + signed URLs, built-in player |

Why not MemberPress: it's WordPress/PHP, its fixed course templates make the custom UI hard, sequential unlock may need LearnDash or custom code, there's plugin licence and update upkeep, and future dashboard or AI tools are awkward. (MemberPress itself is fine and fits the content model; it just doesn't suit a React dev or the custom design.)
Why not Teachable: each kit would be a separate course, so core videos get duplicated and an upgrade means a new course with progress reset. Design control is limited.

### Proposed Payload collections
- **Steps**: the 6 MAPPER steps (title, letter, order, summary)
- **Lessons**: Bunny video ID, title, description, PDF, step, `kits` access (Kitchen / Bathroom / Whole Home / All)
- **Users**: buyers, owned kits, completed lessons (progress)
- **Media**: PDFs, planners, images

### Access rule
- Core lessons → any kit owner
- Kitchen extras → Kitchen + Whole Home
- Bathroom extras → Bathroom + Whole Home
- Whole Home extras → Whole Home only
(Assumes Whole Home includes everything; this still needs confirming.)

### Client admin workflow
Upload the video in Bunny → copy its ID → in Payload, create a Lesson (pick the step, tick the kits, paste the video ID, attach the PDF) → it goes live for the right members.

## Message already sent / drafted to the client
> Hi [Name], MemberPress would work, but we'd recommend a custom build instead. MemberPress's fixed templates would limit the MAPPER journey design, especially on mobile. A custom build gives you exactly the design you want, upgrades that unlock more content in the same account, a simple admin panel to manage videos yourself, and an easy way to add a dashboard or AI tools later. Quick question: should Whole Home include all the Kitchen and Bathroom videos?

## Open questions for the client
1. Does Whole Home include all the Kitchen + Bathroom extras?
2. Can a user own two room kits (Kitchen + Bathroom) without Whole Home?
3. Is the step unlock strictly sequential, or just progress ticks?
4. Pricing per kit and the upgrade pricing (charge only the price difference?)
5. Final confirmation on the custom build vs. MemberPress

## Still needed from the client / team
- The brief (latest), style guide (colors, fonts, logo), MAPPER deck, and the founder's reference screenshots
- Figma links if designs exist

## Next steps
1. Get the client's answer on the platform + open questions
2. Scaffold Next.js + Payload with the collections above
3. Stripe checkout + webhook → set owned kits on the user
4. Build public pages (mobile first) using the style guide
5. Build the member area: step list, video player, progress ticks, locked/upgrade cards

## Notes on the developer (user)
- React developer; wants **no complications** and no unpaid extra work (e.g. a custom admin portal)
- Prefers **short, simple** client messages

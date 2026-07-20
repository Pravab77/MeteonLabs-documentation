```
---
aliases:
  - TerraCast Delivery Phases
  - TerraCast Roadmap
tags:
  - terracast
  - meteon-labs
  - roadmap
  - v1
---

# TerraCast v1 Delivery Phases

> [!info] TerraCast documentation
> [[TerraCast Project Overview|Project Overview]] · [[PRD|PRD]] · [[TerraCast|Docs Hub]] · [[Architecture|Architecture]] · [[Rules|Engineering Rules]] · [[Design|Design System]]

**Company:** Meteon Labs  
**Product:** TerraCast  
**Delivery model:** Sequential, demoable phases  
**Last updated:** 20 July 2026

Each phase must produce a working increment. Work may be planned for a later phase, but implementation begins only after the current phase exit criteria pass and are confirmed.

## Phase 1 — Product Foundation and Authentication

**Goal:** Deliver a polished SaaS foundation with real authentication, onboarding and an empty personalized app shell.

### Build

- Provision a Supabase development project for Auth and PostgreSQL.
- Scaffold the Next.js and Django projects using the structure in `Architecture.md`.
- Establish the design tokens and shared UI primitives from `Design.md`.
- Build the public SaaS shell:
  - Responsive navigation
  - Landing-page hero with a real product preview, not invented metrics
  - Product value sections
  - “How it works” section
  - Clear sign-up and sign-in actions
  - Professional footer
- Set up `next-intl` from the first page so no UI string is hardcoded. English may be the only complete locale in this phase, but an Odia layout sample must be used for rendering checks.
- Implement Supabase sign-up, sign-in, sign-out and session restoration.
- Verify Supabase access tokens on Django before serving protected APIs.
- Create the Django user profile domain for mode, language and location preferences.
- Build onboarding for:
  - Mode: farmer, outdoor worker, elderly or general
  - Language
  - Location preference, without requiring browser location permission yet
- Build the authenticated app shell with responsive navigation, profile controls and a meaningful empty dashboard state.
- Add styled loading, empty, error and not-found states.

### Quality gate

- No credentials are committed.
- All protected Django routes reject missing or invalid identity.
- All visible copy uses translation keys.
- Sign-in, onboarding and app-shell layouts work on mobile and desktop.
- Keyboard focus, field labels, validation messages and touch targets pass the accessibility rules.
- No fake live weather, testimonials, customer logos or trust claims appear.

### Exit criteria

A new user can discover the product on a professional landing page, sign up, choose mode and language, and reach an empty authenticated dashboard that visibly reflects those choices. A returning user can sign in and restore the same profile.

## Phase 2 — Core Weather Data Pipeline

**Goal:** Deliver accurate live weather end to end without AI-generated copy.

### Decision gate

Select the weather and geocoding approach before integration. Record India coverage, observation freshness, forecast range, alert support, rate limits, terms and cost. Do not wire an invented or temporary fake endpoint.

### Build

- Implement the weather-provider adapter inside the Django `weather` app.
- Normalize provider responses into TerraCast condition and forecast schemas.
- Validate coordinates, manual location input and provider responses.
- Add GPS capture with clear consent and a manual-location fallback.
- Build current-conditions and forecast API resources.
- Display:
  - Location and observation time
  - Current temperature and condition
  - Feels-like temperature where available
  - Short hourly outlook
  - Multi-day forecast
  - Source freshness or stale status
- Use a v1-appropriate built-in or platform cache only if measurement shows it is needed. Redis remains out of scope.
- Add friendly provider-unavailable, invalid-location, permission-denied and stale-data states.

### Quality gate

- Provider payloads never leak directly to the frontend.
- Displayed units, timestamps and location names are correct.
- Mock fixtures are clearly limited to automated tests and marked development demos.
- One widget failure does not blank the whole weather page.

### Exit criteria

The dashboard shows accurate, timestamped current conditions and forecast data for GPS and manually selected locations, with no AI-generated language and no fake live data.

## Phase 3 — AI Weather Brief

**Goal:** Convert validated weather data into a short, useful plain-language brief while preserving a complete non-AI fallback.

### Decision gate

Select the LLM provider based on structured-output support, regional-language potential, latency, privacy, reliability and expected cost. Flag any required dependency before adding it.

### Build

- Create the top-level `agents/` package and CrewAI integration.
- Implement the Weather Brief Crew using only normalized weather fields.
- Define and validate a structured output containing:
  - Brief headline
  - Short explanation
  - Practical action cues
  - Condition key for presentation
- Add a Django service and `/api/v1/weather/brief/` flow.
- Render raw weather first where practical; add the brief without blocking the entire page.
- Provide a deterministic brief-unavailable fallback when agent output times out or fails validation.
- Log generation failures without logging secrets or unnecessary exact location data.

### Quality gate

- Generated output cannot alter raw measurements or provider alerts.
- Prompt inputs contain only required validated context.
- Timeouts and malformed responses are handled.
- The page remains useful when CrewAI or the LLM provider is unavailable.

### Exit criteria

The dashboard shows a concise, human-readable English weather brief beside verified raw data, and the raw-weather experience continues to work during an intentional AI failure test.

## Phase 4 — Regional Language Experience

**Goal:** Deliver natural regional-language UI and weather briefs rather than literal word replacement.

### Build

- Complete locale message files for the approved first release languages: Hindi, Bangla, Telugu, Odia and English.
- Treat Rajasthani and additional languages as separately scoped additions because script, dialect and translation review must be confirmed.
- Load the correct Noto family variants for each supported script.
- Implement language switching that preserves the current product context.
- Build the Regional Language Crew to adapt an approved brief for language, tone and literacy context.
- Add explicit fallback behavior when a generated translation is unavailable.
- Review key terminology with a fluent reviewer before claiming production readiness.

### Quality gate

- Navigation, forms, validation, errors, empty states and accessibility labels are localized.
- Long translations do not clip, overlap or break navigation.
- The application never assembles translated sentences from fragments.
- At minimum, Odia is tested end to end as the non-Latin verification locale.

### Exit criteria

A user can change language and receive a coherent localized interface and natural weather brief while remaining on the same location and screen. English fallback is visible and safe if generation fails.

## Phase 5 — Mode Personalization and Literacy-Independent Presentation

**Goal:** Make each user mode meaningfully different and make core weather meaning understandable without relying on reading.

### Build

- Implement mode-specific compositions in `components/modes/`:
  - **Farmer:** crop and field-work relevance, rain timing and action-first cues
  - **Outdoor worker:** exposure windows, rain, wind and heat cues
  - **Elderly:** calm pacing, larger controls, simplified hierarchy and strong readability
  - **General:** balanced summary and forecast detail
- Build a local condition-to-icon/animation mapping using stable backend condition keys.
- Add lightweight weather motion for clear, cloudy, rain, storm, wind and heat states.
- Add a static equivalent for reduced motion and low-performance situations.
- Pair text, icon, color, motion and accessible labels without making any one channel mandatory.
- Preserve the professional SaaS visual system across every mode.

### Quality gate

- Switching mode changes hierarchy, visuals and action emphasis—not only font size or wording.
- Icons remain understandable at small sizes and in bright-light conditions.
- Animation does not delay the weather brief or cause layout shift.
- Reduced-motion mode communicates the same meaning.
- A non-reading usability review confirms that core condition and concern level can be understood from the visual presentation.

### Exit criteria

Each mode is visibly and functionally distinct, and a user can identify the current condition and whether attention is required through icons or animation without depending on the brief text.

## Phase 6 — Verified Alerts

**Goal:** Present timely, trustworthy and mode-appropriate severe-weather alerts without relying on AI as the safety authority.

### Build

- Implement the Django `alerts` app.
- Ingest official provider alerts where available.
- Add reviewed deterministic rules only where the provider data supports them.
- Store alert source, affected location, severity, issued time, expiry and verification state.
- Implement the Alert Explanation Crew to simplify and localize an already verified alert.
- Build accessible alert UI with distinct watch and warning treatments.
- Adapt alert presentation for all modes while keeping the authoritative facts unchanged.
- Add clear unavailable, stale and no-active-alert states.
- Use clearly marked simulated conditions for tests and demonstrations.

### Quality gate

- AI cannot create, suppress, change severity or change expiry for an alert.
- Alert state remains available if generated explanation fails.
- Alert color is not the only severity signal.
- Stale or unverified conditions cannot appear as current verified warnings.
- Copy avoids absolute safety promises.

### Exit criteria

A marked test scenario produces the correct verified watch or warning, with source and time visible, and every user mode communicates it clearly. An AI failure test leaves the authoritative alert intact.

## Phase 7 — Quality, Deployment and v1 Release

**Goal:** Turn the complete product into a stable, observable and deployable v1.

### Build

- Run cross-browser, cross-device, cross-language and cross-mode QA.
- Complete accessibility review for keyboard, screen-reader labels, contrast and reduced motion.
- Verify slow-network, offline, stale-data and provider-failure behavior.
- Measure frontend performance, weather latency and brief-generation latency.
- Optimize images, fonts, animations and loading boundaries where measurements justify it.
- Select deployment targets for Next.js and Django.
- Configure production Supabase, environment variables, allowed origins, redirects and secure headers.
- Add deployment-safe logging, health checks and error monitoring using approved platform capabilities or separately approved dependencies.
- Write operator instructions and a release checklist.
- Complete a security and privacy review focused on identity and location handling.

### Release gate

- All earlier phase exit criteria still pass in the production-like environment.
- No secrets or development-only endpoints are present in the build.
- No fake data is presented as live.
- All supported locales and modes pass their primary journey.
- Alert and fallback behavior passes intentional failure tests.
- Public marketing pages accurately describe only features present in v1.

### Exit criteria

TerraCast is deployed, stable and usable across the supported devices, modes and languages. A user can move from the public site through authentication to a live, personalized weather experience with trustworthy fallback and alert behavior.

## v1 Definition of Done

v1 is complete only when the public SaaS website and authenticated weather product form one coherent experience. Visual polish alone is insufficient, and feature completeness without reliable failures, localization, accessibility and alert integrity is also insufficient.

## Related Notes

- [[TerraCast Project Overview]] provides the original product vision behind the roadmap.
- [[PRD]] defines the product requirements delivered by these phases.
- [[Architecture]] defines the system implemented across these phases.
- [[Rules]] defines the boundaries and completion requirements for each phase.
- [[Design]] provides the UI and UX acceptance criteria used at every quality gate.
```
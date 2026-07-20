```
---
aliases:
  - TerraCast Engineering Rules
tags:
  - terracast
  - meteon-labs
  - engineering-rules
---

# TerraCast Engineering Rules

> [!info] TerraCast documentation
> [[TerraCast Project Overview|Project Overview]] · [[PRD|PRD]] · [[TerraCast|Docs Hub]] · [[Architecture|Architecture]] · [[Phases|Delivery Phases]] · [[Design|Design System]]

**Company:** Meteon Labs  
**Product:** TerraCast  
**Applies to:** Every human or AI contributor  
**Last updated:** 20 July 2026

When a requirement is unclear or two documents conflict, stop and ask. Do not hide an assumption in implementation.

## 1. Source of Truth

- `Phases.md` defines what may be built now.
- `Architecture.md` defines system boundaries and folder ownership.
- `Design.md` defines product presentation and UX acceptance criteria.
- This file defines non-negotiable engineering behavior.
- A current-phase requirement wins over an attractive future idea.

## 2. Stack Boundaries

- Frontend: **Next.js App Router + TypeScript + Tailwind CSS** only.
- Do not add Create React App, plain JavaScript source files or CSS-in-JS.
- Backend: **Django + Django REST Framework** only.
- Do not mix in Flask or FastAPI.
- Access application data through Django models and services. The frontend must not use raw SQL or introduce another ORM.
- Authentication: **Supabase Auth** only. Do not add a second auth provider or hand-roll token issuance.
- Localization: **next-intl** only. Every user-facing string must use message keys from its first implementation.
- Agent orchestration: **CrewAI** only, and only inside the top-level `agents/` package.
- Django is the only caller of weather and LLM providers.
- **Redis and Docker are explicitly deferred for v1.** Do not add them as general best practice.

## 3. Phase and Scope Discipline

- Build only the active phase.
- Do not begin later-phase functionality before the current phase exit criteria pass.
- Do not add billing, TerraCast Pro, reports, developer APIs, MCP or research features to v1.
- Clean extension points are allowed; placeholder services, fake endpoints and half-built screens are not.
- Do not refactor unrelated files during a scoped task. Record the opportunity separately.
- Do not silently change the folder structure in `Architecture.md`.
- Any intentional architecture change must update the relevant document in the same reviewed change.

## 4. Dependency Policy

Before adding any frontend or backend package, state:

1. What requirement it solves.
2. Why the approved stack or standard library cannot solve it adequately.
3. Its runtime, bundle-size, security and maintenance impact.
4. Whether it is needed in production or only development.

Do not add a package merely to solve a one-off mock or visual effect. Prefer browser, platform, Python standard-library, Django or Tailwind capabilities where they are sufficient.

## 5. SaaS UI Quality Rules

- Build a customer-facing product, not an admin template or analytics dashboard.
- Follow the tokens and layouts in `Design.md`; do not invent new colors, radii or shadow styles inside individual components.
- Use restrained surfaces, clear hierarchy and generous whitespace. Avoid decorative glassmorphism, excessive gradients, neon effects and noisy chart grids.
- The public site and authenticated app must share one brand system.
- Never use invented testimonials, customer logos, usage numbers, forecast accuracy claims or safety claims.
- A page must have one obvious primary action.
- Every interactive element requires default, hover, focus, active, disabled and loading behavior where applicable.
- Mobile layout is the baseline. Desktop expansion must not make the app dense or dashboard-like.
- Use progressive disclosure for secondary weather metrics rather than placing every value above the fold.

## 6. Accessibility and Literacy Independence

- Meet WCAG AA contrast at minimum.
- All functionality must be keyboard-accessible.
- Visible focus states are required and may not be removed for aesthetics.
- Touch targets must be at least 44 by 44 CSS pixels.
- Information may never depend on color alone.
- Icons that carry meaning require an accessible label; decorative icons are hidden from assistive technology.
- Support reduced motion. A static icon must preserve the same meaning when animation is disabled.
- Low-literacy modes must use meaningful icons or animations, layout emphasis and simple actions. A larger font alone does not count as a mode variant.
- Test every significant UI change at a narrow mobile viewport and with at least one non-Latin script, using Odia as the first verification locale.

## 7. Localization Rules

- No user-facing string is hardcoded in a component, including placeholders, validation, empty states, alt text and toast messages.
- Store stable API keys and codes in the backend; translate display text in the presentation layer unless generated-language requirements explicitly apply.
- Do not build sentences by concatenating translated fragments.
- Layouts must tolerate longer translated text without clipping or overlap.
- Use language names in their own script where practical.
- Locale fallback must be explicit and must never produce a blank interface.

## 8. Weather and Alert Integrity

- Never invent a weather-data provider, endpoint or response shape.
- Never label mock data as live data.
- Display source update time for live conditions and alerts.
- Do not silently present stale weather as current.
- Official provider alerts and approved deterministic backend rules are authoritative.
- An LLM or CrewAI agent must not be the sole source of alert existence, severity or expiry.
- AI may explain, simplify or localize a verified alert but may not suppress or contradict it.
- When there is no verified alert, use neutral language such as “No active alerts found.” Do not make an absolute safety guarantee.
- Simulated severe-weather data is allowed only in clearly marked tests and demos.

## 9. Error Handling and Resilience

- Users must never see raw stack traces, unstyled framework errors, blank screens or provider error payloads.
- Weather and alert failures require direct, friendly language and a useful next action.
- An AI brief failure must fall back to verified raw weather instead of failing the whole page.
- Isolate non-critical widget failures whenever practical.
- All backend external calls require bounded timeouts and handled exceptions.
- Log technical detail server-side and return a stable sanitized error code and message.
- Do not swallow exceptions or replace a real failure with fake success data.
- Preserve the last known data only when it is labelled with its timestamp and stale status.

## 10. Security and Privacy

- Never place secrets, private keys, provider tokens or Supabase service-role keys in frontend code or committed files.
- Use environment variables and provide only a credential-free `.env.example` when needed.
- Verify Supabase JWT signature, issuer, audience, expiry and required claims on Django before trusting identity.
- Validate and sanitize all request data on Django even when the frontend validates it.
- Use Django/DRF authorization for every user-owned resource.
- Enable Supabase Row Level Security for tables holding user data.
- Request location permission only in response to a clear user action, explain why it is needed and always provide manual entry.
- Collect the minimum location precision required for the feature.
- Never log access tokens, authorization headers, exact user locations or unnecessary personal data.
- Do not commit `.env` files, credentials, local databases or generated secret files.

## 11. API and Data Contract Rules

- Version public backend routes under `/api/v1/`.
- Validate requests and responses with DRF serializers or explicit validated domain schemas.
- Do not pass provider-specific payloads directly to the frontend.
- Use stable machine-readable codes for conditions, alert severity and errors.
- API changes must update the frontend types and relevant documentation together.
- Preserve backward-compatible fields within v1 where practical; flag breaking changes before implementation.
- Date and time values cross the API in timezone-aware ISO 8601 format.

## 12. Agent and Generated-Content Rules

- Agent code belongs only in `agents/`; orchestration entry points are called by Django services.
- Agents receive validated, minimal context rather than raw requests or secrets.
- Require structured output and validate it before returning it to users.
- Never scatter direct LLM calls across views, components or utilities.
- Prompts may not claim access to data that was not supplied.
- Generated text must not change normalized measurements or verified alert fields.
- Provide a deterministic fallback for timeout, malformed output or provider failure.

## 13. Performance Rules

- Optimize for mid-range mobile devices and unstable networks.
- Avoid blocking the entire page on AI generation when raw weather can render first.
- Keep animations lightweight and lazy-load non-critical media.
- Reserve layout space for async content to avoid disruptive shifts.
- Do not add heavy charts where a compact forecast row or trend cue communicates the same information.
- Measure before introducing caching infrastructure. Any v1 cache must use an approved built-in or platform capability; Redis remains deferred.

## 14. Testing and Completion

A task is complete only when the relevant checks pass:

- Type checking and linting for the touched frontend scope.
- Backend tests for changed business logic and API behavior.
- Authentication and authorization checks for protected routes.
- Loading, empty, error and stale-data states.
- Mobile and desktop responsive behavior.
- Keyboard navigation and visible focus states.
- Reduced-motion behavior where animation is involved.
- At least one Odia rendering check for user-facing copy or layout changes.
- Current phase exit criteria in `Phases.md`.

Do not declare completion based only on the happy path or a visually correct screenshot.

## 15. Forbidden Actions

- Do not invent providers, production data, endorsements or performance claims.
- Do not weaken animation or icon requirements to simplify a low-literacy mode.
- Do not expose credentials or sensitive internal errors.
- Do not bypass Django to access the database, weather provider or LLM provider from the frontend.
- Do not make AI the sole authority for severe-weather alerts.
- Do not add deferred v2/v3 infrastructure or features to v1.
- Do not skip a phase gate because a later feature appears related.

## 16. Related Notes

- [[TerraCast Project Overview]] provides the original project context.
- [[PRD]] defines the product requirements these rules protect.
- [[Architecture]] defines service ownership, data flow and folder structure.
- [[Phases]] defines when each capability may be implemented.
- [[Design]] defines the required SaaS experience, accessibility and visual quality.
- [[TerraCast]] is the central project index for the Obsidian vault.
```
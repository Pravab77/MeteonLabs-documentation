```
---
aliases:
  - TerraCast Design System
  - TerraCast UI UX
tags:
  - terracast
  - meteon-labs
  - design-system
  - saas
---

# TerraCast Product Design System

> [!info] TerraCast documentation
> [[TerraCast Project Overview|Project Overview]] · [[PRD|PRD]] · [[TerraCast|Docs Hub]] · [[Architecture|Architecture]] · [[Rules|Engineering Rules]] · [[Phases|Delivery Phases]]

**Company:** Meteon Labs  
**Product:** TerraCast  
**Design direction:** Calm, premium, trustworthy SaaS  
**Last updated:** 20 July 2026

## 1. Design North Star

TerraCast should feel like a modern big-technology product built around weather, not a generic admin dashboard and not a cartoon weather app. The experience combines the clarity of a professional SaaS platform with the warmth and directness required for broad use across India.

Three principles control every visual decision:

1. **Meaning before decoration.** Weather condition, concern level and recommended action must be clear before visual flourish is added.
2. **Calm by default, unmistakable when urgent.** Everyday screens stay restrained so that verified alerts have real visual authority.
3. **Simple without looking basic.** Use disciplined spacing, typography, interaction and content—not extra effects—to make the product feel premium.

## 2. Brand Personality

TerraCast is:

- Trustworthy, not dramatic
- Precise, not technical for its own sake
- Local and human, not childish
- Premium, not luxurious
- Helpful, not overconfident
- AI-assisted, not AI-themed

Avoid generic “AI” gradients, glowing orbs, excessive glass effects, cartoon farms, crowded analytics tiles and fake real-time activity.

## 3. Color System

### Core palette

| Token | Color | Hex | Primary use |
|---|---|---:|---|
| `brand-900` | Deep Atmosphere | `#0B2239` | Hero text, dark navigation, premium brand moments |
| `brand-700` | Monsoon Blue | `#145B7D` | Strong brand surfaces and active navigation |
| `brand-600` | Terra Blue | `#1677A6` | Primary buttons, links and selected controls |
| `brand-100` | Pale Sky | `#DCEFF7` | Selected cards, soft weather context |
| `brand-50` | Air Blue | `#F0F8FC` | Subtle section backgrounds |
| `sun-400` | Sunlight | `#F4C95D` | Clear-weather illustration only |
| `field-600` | Field Green | `#2F7D4A` | Favorable-condition cues and success |
| `surface` | White | `#FFFFFF` | Cards, navigation and controls |
| `canvas` | Cloud White | `#F6F8FA` | App and page background |
| `ink` | Midnight Slate | `#0F172A` | Primary text |
| `muted` | Weathered Slate | `#64748B` | Secondary text and metadata |
| `border` | Mist Line | `#DDE5EC` | Borders and dividers |

### Alert palette

| Token | Color | Hex | Use |
|---|---|---:|---|
| `watch` | Deep Amber | `#B45309` | Verified moderate concern/watch |
| `watch-bg` | Amber Mist | `#FFF7E6` | Watch background |
| `warning` | Signal Red | `#B42318` | Verified high-severity warning |
| `warning-bg` | Red Mist | `#FFF1F0` | Warning background |

Dark amber and red are reserved for verified alert states, destructive account actions and their accessibility-required focus or text treatments. They must not be used for decoration, branding or ordinary sunny weather.

### Usage rules

- Primary pages use `canvas`; important content sits on `surface`.
- Borders do more work than shadows.
- Use brand blue for actions and selected states, never to imply an alert.
- Never communicate severity through color alone; pair it with an icon, label and clear heading.
- Dark mode is optional in v1. If implemented, invert neutral surfaces while preserving the semantic meaning of brand and alert colors.

## 4. Typography

- **Primary family:** Noto Sans.
- **Script coverage:** Noto Sans Devanagari, Bengali, Telugu and Oriya where required.
- **Weights:** Prefer 400, 500, 600 and 700. Avoid ultra-light weights outdoors.
- **Numerals:** Use tabular numerals for temperatures, times and forecast values where available.

| Style | Mobile | Desktop | Weight | Use |
|---|---:|---:|---:|---|
| Display | 36px / 1.1 | 56px / 1.05 | 700 | Marketing hero only |
| App headline | 28px / 1.2 | 36px / 1.15 | 700 | Today's brief |
| H1 | 24px / 1.25 | 30px / 1.2 | 650–700 | Page heading |
| H2 | 20px / 1.3 | 24px / 1.25 | 600 | Section heading |
| Body large | 18px / 1.55 | 18px / 1.55 | 400 | Brief and accessible mode copy |
| Body | 16px / 1.5 | 16px / 1.5 | 400 | Standard UI copy |
| Label | 14px / 1.4 | 14px / 1.4 | 600 | Controls and compact metadata |
| Caption | 13px / 1.4 | 13px / 1.4 | 500 | Updated time and source |

Do not reduce non-Latin script sizes simply to make them fit. Let text wrap and design for expansion.

## 5. Spacing, Shape and Elevation

- Base spacing unit: 4px with primary rhythm at 8px increments.
- Page gutters: 20px mobile, 32px tablet and 48px desktop.
- Marketing content width: maximum 1200px.
- Reading-content width: 680–760px.
- App content width: maximum 1280px.
- Minimum touch target: 44 by 44px.
- Control radius: 10–12px.
- Card radius: 16px.
- Large feature surface radius: 20–24px.
- Default card border: 1px solid `border`.
- Default shadow: subtle and low-spread; reserve stronger elevation for menus and dialogs.

Avoid putting every element inside a card. Use whitespace and section hierarchy to prevent “dashboard tile” overload.

## 6. Public SaaS Website

The public website should explain TerraCast quickly, show the product honestly and guide the visitor toward one primary action.

### Global navigation

- Meteon Labs/TerraCast brand at the left.
- Product and How it works links in the center or primary navigation region.
- Language selector, Sign in and a single primary “Get started” action at the right.
- Mobile navigation opens in an accessible sheet or menu with the same hierarchy.
- Navigation may become lightly elevated on scroll but should not use heavy blur.

### Landing-page sequence

1. **Hero:** A direct outcome-led headline, a short supporting statement, primary CTA and secondary product link.
2. **Product preview:** A realistic responsive dashboard composition using clearly labelled sample content until live data exists.
3. **Core value:** Three focused benefits—local relevance, understandable guidance and inclusive delivery.
4. **How it works:** Location → verified weather data → personalized brief.
5. **User modes:** A concise comparison of farmer, outdoor worker, elderly and general modes.
6. **Language and accessibility:** Show real supported scripts without claiming languages not yet production-ready.
7. **Trust section:** Explain source timestamps, transparent failures and verified-alert behavior without making accuracy guarantees.
8. **Final CTA and footer:** One clear account action, product links and essential company/legal placeholders only when real pages exist.

Do not add pricing, enterprise lead forms, customer logos, testimonials or usage counters until those business facts are real.

## 7. Authenticated App Shell

### Desktop

- Use a compact left navigation or top navigation, not both at full density.
- Keep Dashboard and Forecast primary; Settings and account actions remain secondary.
- Place location, language and profile controls in a predictable header area.
- The main content starts with the weather brief, not a grid of metrics.

### Mobile

- Use a compact top bar and either a small bottom navigation or menu.
- Keep the first screen focused on location, current condition, brief and alert state.
- Forecast details follow in a single column.
- Avoid horizontal scrolling except for an intentionally scrollable hourly forecast row with clear affordance.

## 8. Dashboard Information Hierarchy

The authenticated dashboard follows this order:

1. Location and last-updated time
2. Verified active alert, if one exists
3. Current-condition visual and plain-language brief
4. Primary action cue for the selected mode
5. Hourly outlook
6. Multi-day forecast
7. Secondary values such as humidity, wind and feels-like temperature
8. Data source and freshness information

The absence of an alert should not consume the same visual weight as an active warning.

### Weather hero

- Pair one strong condition visual with the brief headline.
- Use large temperature only when it improves understanding; it must not overpower the action-oriented brief.
- Keep raw measurements secondary but discoverable.
- Reserve enough height before asynchronous data loads to prevent layout shift.

### Forecast presentation

- Use compact rows or cards with day/time, condition icon, high/low or temperature and precipitation cue.
- Prefer readable patterns over dense charts.
- If a chart is later justified, it must have text alternatives and cannot be the only source of meaning.

## 9. Mode-Specific Presentation

Modes share the design system but change information priority and visual communication.

| Mode | Primary emphasis | Interface behavior |
|---|---|---|
| Farmer | Rain timing, wind and field-work suitability | Action cue and weather visual lead; supporting values remain expandable |
| Outdoor worker | Heat, rain, wind and safe work windows | Strong time-based cues, direct actions and glove-friendly controls |
| Elderly | Immediate condition and simple precautions | Larger controls, fewer simultaneous choices, calm wording and strong contrast |
| General | Balanced brief and forecast exploration | Moderate density with access to supporting measurements |

Mode differences must not stereotype users or alter verified facts.

## 10. Iconography and Motion

- Use rounded, clear weather shapes with consistent stroke or fill rules.
- Icons must remain legible at 20–24px and strong at hero scale.
- Store approved local animation assets in `frontend/public/animations/`.
- Map animations from stable condition keys; generated text never selects arbitrary files.
- Everyday motion should be slow and ambient: drifting cloud, gentle rain or subtle sun rays.
- Verified alerts use a distinct but restrained motion pattern.
- Avoid flashing, continuous aggressive pulsing or motion that competes with reading.
- Respect `prefers-reduced-motion` and provide a meaningful static state.
- Lazy-load non-critical motion and never block the brief on animation download.

## 11. Components and States

Core primitives should cover:

- Button
- Text field and location search field
- Select or accessible language picker
- Mode selection card
- Navigation item
- Weather condition visual
- Brief panel
- Hourly forecast item
- Daily forecast row
- Alert banner and alert panel
- Empty state
- Inline error state
- Skeleton/loading state
- Dialog or confirmation surface only when necessary

Every data-driven component specifies:

- Loading
- Success
- Empty or not-applicable
- Stale
- Partial failure
- Full failure

Skeletons should approximate the final layout. Do not use indefinite spinners for the entire page.

## 12. Content Design

TerraCast copy should be direct, calm and useful.

- Start with the situation: “Rain is likely after 4 PM.”
- Follow with a practical cue: “Finish outdoor work earlier if possible.”
- Keep one idea per sentence.
- Explain measurements only when they help a decision.
- Use familiar words and local phrasing after language review.
- Avoid alarmist terms when no verified warning exists.
- Avoid certainty when forecast data is probabilistic.
- Never describe AI output as guaranteed, official or perfectly accurate.

Buttons use actions such as “Use my location,” “Choose manually,” “View forecast” and “Try again.” Avoid vague labels such as “Continue” when a more specific action fits.

## 13. Accessibility Requirements

- WCAG AA contrast minimum throughout.
- Complete keyboard navigation and visible focus rings.
- Semantic heading order and landmarks.
- Form labels remain available to assistive technology even when visual design uses compact fields.
- Error text describes what happened and what the user can do.
- Icons with meaning include a visible text partner or accessible label.
- Color, animation or sound is never the only communication channel.
- Zoom to 200% must not hide essential actions.
- Non-Latin text must wrap without clipping.
- Reduced-motion mode receives static equivalents.

## 14. Responsive Acceptance Criteria

Every key screen must be reviewed at minimum at:

- 360px mobile width
- 768px tablet width
- 1280px desktop width

At all three sizes:

- No accidental horizontal page scroll occurs.
- Primary action and current weather meaning remain above the fold where practical.
- Navigation remains usable.
- Long Odia copy does not overlap controls.
- Alerts remain visible without hiding the underlying weather context.

## 15. Visual Definition of Done

A screen is visually complete only when:

- It follows defined tokens rather than one-off styles.
- It looks consistent with the public website and app shell.
- Loading, error, empty, stale and long-copy states are designed.
- It works on mobile before desktop polish is accepted.
- It passes keyboard, contrast, reduced-motion and Odia layout checks.
- It contains no fake claims, decorative alert colors or unlabelled sample data.
- The primary user decision is clear within a few seconds.

## 16. Related Notes

- [[TerraCast Project Overview]] provides the original project identity and product context.
- [[PRD]] defines the user needs and product requirements this system visualizes.
- [[Architecture]] defines where marketing, app-shell, weather and mode components live.
- [[Rules]] makes this document's accessibility and SaaS quality requirements mandatory.
- [[Phases]] identifies when each design capability is implemented and reviewed.
- [[TerraCast]] is the central project index for the Obsidian vault.
```
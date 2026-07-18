---
title: TerraCast — Product Requirements and Backend Development Specification
aliases:
  - TerraCast PRD
  - TerraCast Backend Specification
company: Meteon Labs
product: TerraCast
document_type: PRD and Engineering Specification
version: 1.1
status: Draft
market: India
created: 2026-07-19
updated: 2026-07-19
tags:
  - meteon-labs
  - terracast
  - prd
  - backend
  - django
  - weather
  - ai
---

# TerraCast — Product Requirements and Backend Development Specification

> [!info] Document purpose
> This is the primary product and backend development reference for TerraCast v1. It explains what the product must do, what the backend owns, how data moves through the system, how the APIs and database should be designed, and how the MVP should be implemented and verified.

| Field | Value |
|---|---|
| Company | Meteon Labs |
| Product | TerraCast |
| Product type | AI-powered hyperlocal weather intelligence platform |
| Target market | India |
| Current phase | Pre-development and architecture planning |
| Primary stack | Next.js, Django, PostgreSQL through Supabase |
| Document version | 1.1 Engineering Draft |

---

## 1. Executive Summary

TerraCast is a hyperlocal weather intelligence platform designed for Indian users who need more than raw temperature, rainfall, and wind values. It converts weather data into understandable, action-oriented guidance based on the user's location, language, accessibility needs, and selected user mode.

The first release supports four user modes:

- Farmer
- Outdoor worker
- Elderly user
- General user

The backend retrieves weather data, converts provider-specific responses into a stable internal format, evaluates weather risks, generates personalized guidance, manages profiles and locations, and exposes everything through a versioned REST API.

> [!important] Core engineering principle
> AI explains the weather. Deterministic backend logic decides the facts and critical risk levels.

An LLM can generate natural-language summaries, but it must not invent weather values or independently decide that a dangerous weather condition exists. Critical alerts are produced from validated provider data and versioned backend rules.

---

## 2. Product Problem

Most weather applications available to Indian users have four major limitations:

1. They display raw meteorological values without explaining what action the user should take.
2. Their localization is limited or based on literal translation.
3. Their content assumes that every user can comfortably read text and understand weather terminology.
4. They show nearly identical information to users with very different risks and decisions.

For example, heavy rainfall may mean:

- Delay irrigation for a farmer.
- Avoid or reschedule a delivery shift for an outdoor worker.
- Stay indoors and avoid slippery roads for an elderly user.
- Carry an umbrella for a general user.

TerraCast closes the gap between weather data and practical decision-making.

---

## 3. Product Objectives

### 3.1 Primary Objectives

| Objective | Expected outcome |
|---|---|
| Hyperlocal forecasts | Forecasts are generated for precise coordinates instead of only a city name |
| Action-oriented guidance | Raw data is converted into clear recommendations |
| User-mode personalization | Guidance and surfaced metrics change according to the selected mode |
| Regional accessibility | Interface content and generated guidance are available in supported Indian languages |
| Literacy-independent alerts | Critical conditions are also represented through icons, colors, motion, and severity states |
| Reliable backend contracts | The frontend receives predictable, versioned, validated API responses |

### 3.2 Draft Engineering Success Targets

These are initial service-level targets for the MVP. They should be revised after load testing and real usage.

| Metric | MVP target |
|---|---|
| Cached forecast API latency | p95 below 500 ms |
| Fresh provider-backed forecast latency | p95 below 3 seconds |
| AI guidance generation latency | p95 below 8 seconds |
| API availability during beta | 99.5% |
| Successful structured AI responses | At least 98% after retry and fallback handling |
| Critical alert rule evaluation | 100% of valid forecast responses |
| Core backend test coverage | At least 80% for service and rule-engine modules |

### 3.3 Non-Goals for v1

The following are intentionally outside the first release:

- Training a custom weather forecasting model
- Replacing official meteorological warnings
- Building a public developer API
- Building the Meteon MCP server
- Building a complex multi-agent production system
- Supporting business billing or subscriptions
- Providing historical climate research tools
- Running a multi-provider automatic failover system
- Using Redis, Kafka, or a distributed task queue in the initial implementation

---

## 4. Users and Backend Implications

| User mode | User need | Backend implication |
|---|---|---|
| Farmer | Irrigation, sowing, harvesting, rain, frost, and hail guidance | Store agricultural preferences and prioritize rain, soil, humidity, and wind signals |
| Outdoor worker | Shift planning and heat, rain, UV, and lightning safety | Prioritize hourly conditions and exposure-related risks |
| Elderly user | Simple, high-contrast, low-density safety guidance | Return short action lists and accessibility metadata |
| General user | Daily planning and common forecast information | Return balanced current, hourly, and daily information |

The selected mode is stored in the backend profile. It must not exist only as frontend state because the same account may be used from multiple devices.

---

## 5. Product Scope

### 5.1 v1 — TerraCast Core

The v1 backend must support:

- Authentication verification through Supabase Auth
- User profile and preference management
- Saved and primary locations
- Coordinate-based weather retrieval
- Provider response normalization
- Current, hourly, and daily forecast responses
- Deterministic weather-risk evaluation
- Personalized AI guidance
- Regional-language generation
- Critical alert metadata for the animation system
- Feedback collection
- Health checks, logs, metrics, and error tracking

### 5.2 v2 — Product Expansion

- Meteon Reports newsletter
- TerraCast Pro
- Scheduled reports and alerts
- Deeper forecast history
- More user-mode controls
- Background job processing
- Redis-backed caching and queues if scale requires them

### 5.3 v3 — Meteon Platform

- Meteon API
- Meteon MCP server
- B2B accounts and API keys
- Usage metering and billing
- Meteon Research datasets and analytical outputs

---

## 6. Functional Requirements

Priority definitions:

- **P0:** Required for the MVP to function
- **P1:** Important for a useful public beta
- **P2:** Valuable but can be added after the first beta

| ID | Priority | Requirement | Acceptance criteria |
|---|---|---|---|
| AUTH-001 | P0 | Verify authenticated Supabase users | A valid access token maps to one backend profile; invalid or expired tokens return 401 |
| USER-001 | P0 | Create and update a user profile | Mode, language, units, and accessibility settings can be retrieved and updated |
| LOC-001 | P0 | Save user locations | Users can create, update, list, delete, and select one primary location |
| LOC-002 | P0 | Validate coordinates | Latitude and longitude outside valid ranges are rejected with a structured 400 response |
| WTH-001 | P0 | Retrieve current weather | The API returns normalized current conditions for a valid location |
| WTH-002 | P0 | Retrieve hourly and daily forecasts | The response includes provider timestamps, timezone, units, and freshness metadata |
| WTH-003 | P0 | Cache provider responses | Equivalent requests reuse a non-expired snapshot instead of repeatedly calling the provider |
| RISK-001 | P0 | Evaluate weather risks | Every valid forecast is checked by deterministic heat, rain, wind, storm, and cold rules |
| GUIDE-001 | P0 | Generate personalized guidance | Guidance uses the selected user mode and only facts present in normalized weather data |
| GUIDE-002 | P0 | Validate AI output | Invalid model output is retried once and then replaced by a deterministic template |
| LANG-001 | P0 | Return guidance in a selected language | The requested supported language is applied without changing numeric weather facts |
| ALERT-001 | P0 | Return critical alert metadata | Alerts include type, severity, time window, action list, origin, and a stable visual code |
| FEED-001 | P1 | Collect user feedback | A user can mark guidance helpful or unhelpful and optionally add a short comment |
| ADMIN-001 | P1 | Manage system configuration | Authorized administrators can update supported languages, provider status, and risk thresholds |
| AUDIT-001 | P1 | Record important system events | Auth failures, provider failures, alert creation, and configuration changes are traceable |
| NOTIFY-001 | P2 | Deliver push or email notifications | Users can opt in to scheduled or urgent notifications |

---

## 7. High-Level System Architecture

~~~mermaid
flowchart TD
    Client["Next.js Web App"] --> Auth["Supabase Auth"]
    Client --> API["Django REST API"]
    API --> Services["TerraCast Application Services"]
    Services --> DB["Supabase PostgreSQL"]
    Services --> External["Weather Provider and LLM"]
~~~

### 7.1 Component Responsibilities

| Component | Responsibility |
|---|---|
| Next.js frontend | Presentation, interaction, route handling, local UI state, next-intl, and animations |
| Supabase Auth | Sign-up, sign-in, password recovery, sessions, and access-token issuance |
| Django REST API | Authorization, validation, orchestration, business rules, persistence, and API contracts |
| PostgreSQL | Profiles, locations, weather snapshots, guidance, alerts, configuration, and audit records |
| Weather provider | Raw current, hourly, daily, and alert data |
| LLM provider | Natural-language guidance generated from validated structured facts |

### 7.2 Backend Ownership Boundary

The Django backend owns:

- Authorization decisions
- Request validation
- Weather-provider integration
- Unit and field normalization
- Risk evaluation
- AI context construction
- AI output validation
- Persistence and cache decisions
- API error formatting
- Audit and operational events

The frontend must never directly call the weather provider or LLM provider. This prevents secret exposure, inconsistent rules, and uncontrolled provider usage.

---

## 8. Backend Design Principles

### 8.1 Thin Views and Explicit Services

API views should parse a request, call a service, and serialize the result. Business logic belongs in service modules, not in views or Django models.

~~~text
API View
   -> Input Serializer
   -> Application Service
   -> Domain Rules, Provider Adapter, or Repository
   -> Output Serializer
~~~

### 8.2 Provider Independence

TerraCast depends on its own normalized weather schema, not on a provider-specific response. A provider adapter converts external data into the TerraCast schema.

Changing the weather provider should require a new adapter, not changes throughout the application.

### 8.3 Deterministic Risk Evaluation

Risk levels are generated by versioned backend rules. The LLM receives already-computed risks and explains them in natural language.

### 8.4 Structured AI Output

The AI provider must return JSON matching an explicit schema. Free-form model output must not be stored or sent to the frontend without validation.

### 8.5 Versioned Interfaces

All public endpoints begin with **/api/v1/**. Breaking API changes require a new version or a backward-compatible migration period.

### 8.6 Graceful Degradation

If AI generation fails, the user must still receive:

- The normalized forecast
- Deterministic risk levels
- Template-based safety guidance
- A response flag showing that fallback guidance was used

---

## 9. Proposed Backend Project Structure

~~~text
backend/
├── manage.py
├── pyproject.toml
├── .env.example
├── config/
│   ├── settings/
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── apps/
│   ├── accounts/
│   │   ├── models.py
│   │   ├── serializers.py
│   │   ├── services.py
│   │   ├── permissions.py
│   │   ├── urls.py
│   │   └── views.py
│   ├── locations/
│   ├── weather/
│   │   ├── providers/
│   │   ├── normalizers/
│   │   ├── selectors.py
│   │   └── services.py
│   ├── risks/
│   │   ├── rules/
│   │   └── engine.py
│   ├── guidance/
│   │   ├── prompts/
│   │   ├── schemas.py
│   │   ├── services.py
│   │   └── fallbacks.py
│   ├── alerts/
│   ├── feedback/
│   └── audit/
├── common/
│   ├── exceptions.py
│   ├── middleware.py
│   ├── pagination.py
│   ├── responses.py
│   └── validators.py
└── tests/
    ├── unit/
    ├── integration/
    ├── contract/
    └── e2e/
~~~

### 9.1 Django App Responsibilities

| Django app | Responsibility |
|---|---|
| **accounts** | Backend profile, preferences, user mode, and authorization helpers |
| **locations** | Saved locations, coordinate validation, primary-location rules, and timezone metadata |
| **weather** | Provider adapters, normalization, snapshots, forecast retrieval, and freshness rules |
| **risks** | Deterministic thresholds and risk-evaluation engine |
| **guidance** | Prompt construction, structured generation, validation, caching, and fallbacks |
| **alerts** | Alert lifecycle, severity, visual codes, and user-specific presentation |
| **feedback** | Guidance and alert feedback |
| **audit** | Security and important system event records |

---

## 10. Data Model

### 10.1 Entity Relationship Overview

~~~mermaid
erDiagram
    USER_PROFILE ||--o{ SAVED_LOCATION : owns
    USER_PROFILE ||--o{ GUIDANCE : receives
    SAVED_LOCATION ||--o{ WEATHER_SNAPSHOT : has
    WEATHER_SNAPSHOT ||--o{ GUIDANCE : supports
    WEATHER_SNAPSHOT ||--o{ ALERT : produces
~~~

### 10.2 User Profile

Stores application-specific information for an authenticated Supabase user.

| Field | Type | Notes |
|---|---|---|
| id | UUID | Primary key |
| supabase_user_id | UUID | Unique external identity reference |
| mode | Enum | farmer, outdoor_worker, elderly, or general |
| language_code | String | Code such as en, hi, or, bn, or te |
| unit_system | Enum | Metric initially |
| accessibility_preferences | JSONB | High contrast, reduced motion, text size, and simplified layout |
| created_at | Timestamp | UTC |
| updated_at | Timestamp | UTC |

Important constraints:

- **supabase_user_id** is unique.
- Mode and language use only supported values.
- Passwords and authentication credentials are never stored in this table.

### 10.3 Saved Location

| Field | Type | Notes |
|---|---|---|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to the user profile |
| label | String | User-facing name such as Home or Farm |
| latitude | Decimal | Range -90 to 90 |
| longitude | Decimal | Range -180 to 180 |
| normalized_name | String | Resolved locality name |
| timezone | String | IANA timezone identifier |
| is_primary | Boolean | Only one primary location per user |
| created_at | Timestamp | UTC |
| updated_at | Timestamp | UTC |

Recommended database rules:

- Composite index on user ID and primary status
- Database constraint or transaction logic that permits only one primary location per user
- Spatial indexing can be added later if radius searches become necessary

### 10.4 Weather Snapshot

Stores normalized provider data and acts as the initial forecast cache.

| Field | Type | Notes |
|---|---|---|
| id | UUID | Primary key |
| location_id | UUID | Nullable when serving an unsaved coordinate |
| coordinate_key | String | Rounded coordinate pair used for cache lookup |
| provider | String | Provider identifier |
| provider_request_id | String | Optional trace reference |
| current_data | JSONB | Normalized current conditions |
| hourly_data | JSONB | Normalized hourly periods |
| daily_data | JSONB | Normalized daily periods |
| provider_alerts | JSONB | Validated upstream alerts |
| observed_at | Timestamp | Time represented by the data |
| fetched_at | Timestamp | Provider retrieval time |
| expires_at | Timestamp | Cache expiry |
| schema_version | Integer | Normalized schema version |
| source_hash | String | Detects equivalent source data |

> [!note] Initial cache decision
> Redis is intentionally deferred. During the single-instance MVP, normalized forecast snapshots are cached in PostgreSQL using **coordinate_key** and **expires_at**. The code should still use a cache abstraction so Redis can replace or supplement this implementation later.

### 10.5 Guidance

| Field | Type | Notes |
|---|---|---|
| id | UUID | Primary key |
| user_id | UUID | User receiving the guidance |
| location_id | UUID | Related location |
| weather_snapshot_id | UUID | Exact facts used for generation |
| mode | String | User mode at generation time |
| language_code | String | Generated language |
| headline | String | Short summary |
| summary | Text | Main explanation |
| actions | JSONB | Ordered action list |
| risk_summary | JSONB | Deterministic risks included in the response |
| visual_state | JSONB | Animation and icon identifiers |
| generation_source | Enum | LLM or fallback_template |
| model_name | String | AI model identifier when applicable |
| prompt_version | String | Versioned prompt reference |
| input_hash | String | Used to reuse equivalent guidance |
| generated_at | Timestamp | UTC |
| expires_at | Timestamp | Re-generation threshold |

### 10.6 Alert

| Field | Type | Notes |
|---|---|---|
| id | UUID | Primary key |
| weather_snapshot_id | UUID | Snapshot that triggered the alert |
| alert_type | Enum | Heat, heavy rain, flood, wind, storm, lightning, cold, or hail |
| severity | Enum | info, watch, warning, or critical |
| origin | Enum | official, weather_provider, or terracast_rule |
| rule_version | String | Rule set used to calculate severity |
| starts_at | Timestamp | Expected start |
| ends_at | Timestamp | Expected end |
| visual_code | String | Stable frontend animation or icon identifier |
| facts | JSONB | Values that triggered the rule |
| status | Enum | active, expired, or cancelled |
| created_at | Timestamp | UTC |

### 10.7 Supporting Data

The system will also require:

- Feedback
- Supported languages
- Risk-rule versions
- Prompt versions
- Audit events

These may begin as version-controlled configuration files and move to database tables when administrators need runtime control.

---

## 11. Weather Provider Integration

### 11.1 Provider Interface

Every weather provider adapter implements the same internal contract:

~~~python
class WeatherProvider:
    def get_forecast(self, latitude, longitude, timezone=None):
        """Return provider data for normalization."""

    def get_alerts(self, latitude, longitude):
        """Return official or provider-issued alerts when available."""
~~~

The rest of the system must not import a provider SDK directly.

### 11.2 Normalized Weather Schema

At minimum, the internal schema supports:

- Temperature and apparent temperature
- Humidity
- Precipitation probability and amount
- Wind speed, direction, and gust
- Atmospheric pressure
- Visibility
- UV index
- Weather condition code
- Sunrise and sunset
- Hourly periods
- Daily periods
- Provider alerts
- Source, timestamps, timezone, units, and data freshness

Missing fields are represented as null. They must never be invented or silently replaced with zero.

### 11.3 Cache Strategy

1. Round coordinates to a documented precision to create a coordinate key.
2. Search for a non-expired normalized snapshot.
3. Return it immediately when available.
4. Otherwise call the provider with strict timeouts.
5. Validate and normalize the response.
6. Save the new snapshot.
7. Evaluate risks before returning the result.

Suggested starting cache lifetime:

| Data type | Initial lifetime |
|---|---|
| Current conditions | 10 minutes |
| Hourly forecast | 30 minutes |
| Daily forecast | 2 hours |
| Critical provider alert | 5 minutes |

These values belong in configuration and must not be scattered as hard-coded values.

### 11.4 Provider Failure Behaviour

If the provider fails:

- Use a recently expired snapshot only when it is inside the configured stale window.
- Mark the response as stale.
- Include the original fetch timestamp.
- Do not generate a new critical alert from data beyond the allowed stale window.
- Return **503 WEATHER_PROVIDER_UNAVAILABLE** when no safe fallback exists.

---

## 12. Risk and Alert Engine

### 12.1 Responsibility

The risk engine converts normalized weather facts into typed and versioned risk results. It is independent of the LLM.

Example internal result:

~~~json
{
  "type": "extreme_heat",
  "severity": "warning",
  "score": 0.82,
  "facts": {
    "temperature_c": 42,
    "apparent_temperature_c": 46,
    "humidity_percent": 58
  },
  "rule_version": "heat-v1"
}
~~~

### 12.2 Rule Design

Each rule defines:

- Required weather fields
- Thresholds
- Severity mapping
- Applicable forecast window
- User-mode adjustments to guidance
- Stable visual code
- Rule version

User mode may change the recommended action, but it does not change the underlying weather fact or severity calculation.

### 12.3 Official Alerts

Provider or government-issued alerts must be clearly distinguished from TerraCast-generated risk assessments. Every alert response includes an origin:

- **official**
- **weather_provider**
- **terracast_rule**

---

## 13. AI Guidance Pipeline

### 13.1 AI Responsibility

The AI layer may:

- Summarize validated weather facts
- Rephrase deterministic risks
- Adapt tone and detail to the user mode
- Generate natural regional-language guidance
- Produce short action lists

The AI layer must not:

- Change temperatures, rainfall, wind, timestamps, or units
- Create an alert that the risk engine did not produce
- Claim certainty beyond the provider data
- Provide unsupported agricultural, medical, or emergency claims
- Hide the age or freshness of weather data

### 13.2 Generation Flow

~~~mermaid
flowchart TD
    Data["Normalized Weather"] --> Rules["Risk Engine"]
    Rules --> Context["Structured Context"]
    Context --> LLM["LLM Generation"]
    LLM --> Validate["Schema and Fact Validation"]
    Validate --> Result["Guidance or Template Fallback"]
~~~

### 13.3 Structured Output Contract

~~~json
{
  "headline": "Heavy rain is likely this evening",
  "summary": "Rain may become intense between 5 PM and 8 PM.",
  "actions": [
    {
      "priority": 1,
      "text": "Finish outdoor work before 5 PM",
      "icon_code": "work_indoors"
    }
  ],
  "risk_references": ["heavy_rain"],
  "language_code": "en",
  "confidence_note": "Based on the latest forecast available at 14:30 IST"
}
~~~

### 13.4 Validation and Fallback

The backend validates:

- Required fields and maximum lengths
- Supported language code
- Allowed risk references
- Allowed icon codes
- Numeric facts against the source snapshot
- Absence of unexpected HTML or executable content

If validation fails:

1. Retry once with the validation errors added to the model context.
2. If the retry fails, use a deterministic language template.
3. Record the failure reason without logging private data or provider secrets.

### 13.5 Multi-Agent Decision

CrewAI can be explored during prototyping, but v1 should not require multiple production agents. One well-defined guidance service with separate pipeline stages is easier to test, observe, and control.

The conceptual crews remain useful as future domain boundaries:

- Weather Brief
- Regional Language
- Alert Reasoning
- Meteon Reports

They should become separate agents only if evaluation proves that this improves quality enough to justify the additional latency, cost, and failure modes.

---

## 14. API Design

### 14.1 General Conventions

| Convention | Decision |
|---|---|
| Base path | /api/v1/ |
| Protocol | HTTPS only outside local development |
| Format | JSON |
| Authentication | Supabase access token using the Bearer authorization scheme |
| Time format | ISO 8601 in UTC, with location timezone returned separately |
| IDs | UUID |
| Field naming | snake_case |
| Units | Explicit in field names or response metadata |
| Request tracing | X-Request-ID returned on every response |

### 14.2 System Endpoints

| Method | Endpoint | Purpose | Auth |
|---|---|---|---|
| GET | /api/v1/health/live | Process liveness | No |
| GET | /api/v1/health/ready | Database and required dependency readiness | No |
| GET | /api/v1/meta/languages | Supported languages | No |

### 14.3 Profile Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /api/v1/profile | Get the current user profile |
| PATCH | /api/v1/profile | Update mode, language, units, or accessibility settings |

### 14.4 Location Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /api/v1/locations | List saved locations |
| POST | /api/v1/locations | Create a location |
| GET | /api/v1/locations/{location_id} | Retrieve one location |
| PATCH | /api/v1/locations/{location_id} | Update a location |
| DELETE | /api/v1/locations/{location_id} | Delete a location |
| POST | /api/v1/locations/{location_id}/make-primary | Set the primary location |

### 14.5 Forecast and Guidance Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /api/v1/locations/{location_id}/forecast | Current, hourly, and daily normalized forecast |
| GET | /api/v1/locations/{location_id}/alerts | Active alerts and risk states |
| POST | /api/v1/locations/{location_id}/guidance | Generate or reuse personalized guidance |
| GET | /api/v1/guidance/{guidance_id} | Retrieve previously generated guidance |
| POST | /api/v1/guidance/{guidance_id}/feedback | Submit feedback |

### 14.6 Forecast Response Example

~~~json
{
  "data": {
    "location": {
      "id": "0d8b8e2c-90f4-4f17-86bb-6831eef8ad67",
      "name": "Bhubaneswar",
      "latitude": 20.2961,
      "longitude": 85.8245,
      "timezone": "Asia/Kolkata"
    },
    "current": {
      "temperature_c": 32.4,
      "apparent_temperature_c": 38.1,
      "humidity_percent": 74,
      "condition_code": "partly_cloudy"
    },
    "hourly": [],
    "daily": [],
    "risks": [],
    "freshness": {
      "status": "fresh",
      "fetched_at": "2026-07-19T10:30:00Z",
      "expires_at": "2026-07-19T10:40:00Z"
    },
    "source": {
      "provider": "configured_provider",
      "schema_version": 1
    }
  },
  "meta": {
    "request_id": "req_01J..."
  }
}
~~~

### 14.7 Standard Error Response

~~~json
{
  "error": {
    "code": "WEATHER_PROVIDER_UNAVAILABLE",
    "message": "Weather data is temporarily unavailable.",
    "details": {},
    "request_id": "req_01J..."
  }
}
~~~

Internal stack traces, SQL details, secret values, and raw provider errors must never be returned to clients.

### 14.8 Important HTTP Status Codes

| Status | Use |
|---|---|
| 200 | Successful read or update |
| 201 | Resource created |
| 204 | Successful deletion with no body |
| 400 | Invalid request data |
| 401 | Missing, invalid, or expired authentication |
| 403 | Authenticated but not authorized |
| 404 | Resource does not exist or does not belong to the user |
| 409 | State conflict |
| 422 | Valid JSON but semantically invalid input |
| 429 | Rate limit exceeded |
| 502 | Invalid upstream response |
| 503 | Required dependency unavailable |

---

## 15. Core Backend Workflows

### 15.1 Authentication

1. The user signs in through Supabase Auth.
2. Supabase returns an access token to the frontend.
3. The frontend sends the token to Django.
4. Django verifies the token signature, issuer, audience, and expiry.
5. Django loads or creates the matching user profile.
6. The request continues with the authenticated user context.

### 15.2 Forecast Request

1. Validate that the requested location belongs to the user.
2. Build a normalized coordinate cache key.
3. Look for a fresh weather snapshot.
4. Fetch from the configured provider when the snapshot is absent or expired.
5. Validate and normalize the provider response.
6. Store the snapshot with freshness metadata.
7. Run deterministic risk rules.
8. Serialize and return the forecast.

### 15.3 Guidance Request

1. Load user mode, language, and accessibility settings.
2. Obtain a fresh or safely stale weather snapshot.
3. Run the risk engine.
4. Build a strictly structured generation context.
5. Reuse existing guidance when the input hash is unchanged and unexpired.
6. Otherwise call the LLM.
7. Validate its schema and facts.
8. Use a template fallback if generation fails.
9. Store the result and return it to the frontend.

### 15.4 Alert Presentation

1. Risk rules or upstream alerts produce a typed alert.
2. The backend assigns severity and visual code.
3. User mode determines the action wording.
4. Language generation or templates localize the text.
5. The frontend maps the visual code and severity to the correct icon, color, and animation.

---

## 16. Regional Language and Accessibility Design

### 16.1 Responsibility Split

| Content | Owner |
|---|---|
| Navigation, buttons, labels, and validation messages | Next.js using next-intl |
| Personalized weather guidance | Django guidance service |
| Critical fallback guidance | Versioned backend translation templates |
| Font rendering | Noto font family in the frontend |
| Animation and visual alerts | Frontend using the backend visual code |

### 16.2 Language Rules

- Never translate numeric values by asking the model to regenerate them.
- Units remain consistent with response metadata.
- Use native script where supported.
- Keep weather terms understandable and avoid unnecessary technical vocabulary.
- Maintain deterministic templates for every P0 language before enabling it.
- Language rollout requires human review of critical alert templates.

### 16.3 Accessibility Metadata

~~~json
{
  "high_contrast": true,
  "reduced_motion": false,
  "large_text": true,
  "simplified_guidance": true,
  "audio_guidance": false
}
~~~

The backend returns semantic states such as severity and visual code. The frontend decides how those states are visually rendered.

---

## 17. Authentication, Authorization, and Security

### 17.1 Authentication Rules

- Supabase owns credentials and sessions.
- Django stores only the external user ID and product profile.
- Every protected request verifies the access token.
- Tokens are never written to application logs.
- Service-role credentials are backend-only.

### 17.2 Authorization Rules

- Users access only their own profile, locations, guidance, alerts, and feedback.
- Object ownership is checked in database queries, not only after retrieval.
- Administrative endpoints require a separate explicit role.
- A missing resource and a resource belonging to another user should normally produce the same 404 response to avoid exposing identifiers.

### 17.3 Input and API Security

- Validate coordinates, enums, text lengths, and JSON shapes.
- Use ORM parameterization instead of raw SQL wherever possible.
- Configure strict CORS origins.
- Enforce HTTPS in production.
- Store secrets only in environment variables or a managed secret store.
- Add request-size limits.
- Escape or reject unexpected markup in AI output.
- Apply rate limits to provider-backed and AI-backed endpoints.

### 17.4 AI-Specific Security

- Treat user-provided labels and feedback as untrusted input.
- Do not place unescaped user text inside system-level prompt instructions.
- Allow only known fields in structured output.
- Keep provider keys, system prompts, and internal rules out of client responses.
- Record prompt and rule versions for debugging and evaluation.

### 17.5 Privacy

TerraCast collects only data required for the product:

- External authentication identifier
- Saved location coordinates and labels
- Product preferences
- Generated guidance and feedback
- Operational logs with redaction

The product must provide a future path for account-data export and deletion. Exact retention policies must be approved before public launch.

---

## 18. Reliability and Error Handling

### 18.1 Dependency Timeouts

Initial limits:

| Dependency | Timeout |
|---|---|
| Weather provider | 5 seconds |
| LLM provider | 15 seconds |
| Database query | Monitored with a slow-query threshold at 500 ms |

### 18.2 Retry Rules

- Retry only operations that are safe to repeat.
- Use exponential backoff with jitter.
- Do not retry invalid 4xx provider responses.
- Limit weather-provider retries to two attempts.
- Limit the LLM schema-correction retry to one attempt.

### 18.3 Stale Data States

Every weather response has one of these freshness states:

- **fresh**
- **stale**
- **unavailable**

The UI shows when the data was last updated. Stale data must never appear current without a visible freshness indicator.

### 18.4 Duplicate Guidance Prevention

Guidance generation uses an input hash based on:

- Weather snapshot
- User mode
- Language
- Prompt version
- Risk-rule version

Equivalent requests reuse the same unexpired output, reducing latency and model cost.

---

## 19. Observability

### 19.1 Structured Logging

Each application log should include:

- Timestamp
- Severity
- Service and environment
- Request ID
- Safe user identifier
- Endpoint
- Duration
- Status code
- Provider name
- Cache state
- Error code

Never log:

- Access tokens
- API keys
- Passwords
- Complete prompts containing user data
- Raw exception responses that may contain secrets

### 19.2 Metrics

Track:

- Request count, latency, and error rate by endpoint
- Weather-provider latency and failure rate
- LLM latency, failure rate, and estimated usage
- Forecast cache hit rate
- Stale-response count
- Guidance fallback rate
- Alert count by type and severity
- Database connection and slow-query metrics

### 19.3 Health Checks

- Liveness confirms that the Django process is running.
- Readiness confirms that the database and required configuration are available.
- Optional upstream status is reported separately so a temporary LLM failure does not incorrectly mark the complete API as unavailable.

---

## 20. Configuration

| Environment variable | Purpose |
|---|---|
| DJANGO_SECRET_KEY | Django cryptographic secret |
| DATABASE_URL | PostgreSQL connection |
| SUPABASE_URL | Supabase project URL |
| SUPABASE_JWKS_URL | Token verification keys |
| SUPABASE_JWT_AUDIENCE | Expected token audience |
| WEATHER_PROVIDER_NAME | Selected provider adapter |
| WEATHER_PROVIDER_API_KEY | Weather-provider secret |
| LLM_PROVIDER_NAME | Selected AI provider |
| LLM_API_KEY | AI-provider secret |
| ALLOWED_HOSTS | Django host validation |
| CORS_ALLOWED_ORIGINS | Approved frontend origins |
| ENVIRONMENT | development, staging, or production |
| LOG_LEVEL | Logging verbosity |

The repository contains an **.env.example** file with placeholder names only. Real environment files must never be committed.

---

## 21. Testing Strategy

### 21.1 Unit Tests

Test isolated logic:

- Coordinate validation
- Cache-key creation
- Provider normalization
- Risk rules and severity boundaries
- Input hashing
- Prompt-context construction
- AI schema validation
- Fallback templates
- Authorization helpers

### 21.2 Integration Tests

Test:

- Django with PostgreSQL
- Supabase token verification using test keys or mocked verification
- Provider adapters against recorded fixtures
- LLM client against mocked structured responses
- Transactions for primary-location updates

### 21.3 Contract Tests

Recorded provider payloads verify that adapter changes do not break the normalized TerraCast schema.

### 21.4 API Tests

Cover:

- Success responses
- Invalid input
- Unauthenticated access
- Cross-user resource access
- Provider timeout
- Stale snapshot fallback
- LLM validation failure
- Rate limiting

### 21.5 AI Evaluation

Maintain a fixed evaluation dataset containing:

- Different weather conditions
- All user modes
- All supported languages
- Critical and non-critical risks
- Missing provider fields
- Contradictory or malformed model output

Evaluate:

- Factual consistency
- Action relevance
- Language quality
- Unsupported claims
- Schema validity
- Severity consistency

### 21.6 Load and Performance Tests

Before beta:

- Test repeated forecast reads with a warm cache.
- Test provider-backed cold requests.
- Test concurrent guidance generation.
- Measure database connection usage.
- Confirm that rate limits and provider quotas behave safely.

---

## 22. Development and Deployment Environments

### 22.1 Environments

| Environment | Purpose |
|---|---|
| Local | Individual development with test credentials and mocked providers |
| Staging | Production-like integration, migration testing, and frontend QA |
| Production | Real users and production provider credentials |

Each environment uses separate databases, secrets, and external-provider credentials.

### 22.2 CI Pipeline

Every pull request runs:

1. Code formatting and linting
2. Static checks
3. Unit tests
4. Integration tests
5. Migration consistency checks
6. Secret scanning
7. Dependency vulnerability checks

### 22.3 Deployment Rules

- Apply database migrations as a controlled release step.
- Run health checks after deployment.
- Keep the previous release available for rollback.
- Never run destructive migrations without a reviewed migration plan.
- Seed only non-sensitive configuration data.
- Docker is optional for the first build and can be introduced when environment consistency or deployment infrastructure requires it.

---

## 23. Suggested Repository Layout

~~~text
terracast/
├── frontend/                 # Next.js application
├── backend/                  # Django REST API
├── docs/
│   ├── PRD.md
│   ├── API.md
│   ├── DATA_MODEL.md
│   └── ADR/
├── scripts/
├── .github/
│   └── workflows/
├── .gitignore
└── README.md
~~~

Architecture Decision Records should be added for important decisions:

- Weather provider selection
- Supabase token-verification approach
- Forecast-cache implementation
- Risk-threshold source
- LLM provider and structured-output method
- Production deployment platform

---

## 24. Implementation Plan

### Phase 0 — Engineering Foundation

Deliverables:

- Repository structure
- Django project and settings split
- Django REST Framework setup
- PostgreSQL connection
- Environment configuration
- Standard response and exception handling
- Request-ID middleware
- Linting, formatting, tests, and CI

Exit criteria:

- The API starts locally.
- Health endpoints work.
- CI passes on an empty feature branch.
- Secrets are not stored in Git.

### Phase 1 — Authentication, Profiles, and Locations

Deliverables:

- Supabase access-token verification
- Automatic backend profile creation
- Profile endpoints
- Saved-location CRUD
- Primary-location transaction rules
- Authorization tests

Exit criteria:

- A signed-in user can manage only their own profile and locations.
- Cross-user access tests pass.

### Phase 2 — Weather Data Pipeline

Deliverables:

- Provider interface and first adapter
- Normalized schema
- Snapshot persistence
- Forecast cache and freshness states
- Forecast endpoint
- Provider fixtures and contract tests

Exit criteria:

- The same frontend contract works independently of the raw provider response.
- Fresh, cached, stale, and unavailable paths are tested.

### Phase 3 — Risk and Alert Engine

Deliverables:

- Versioned rule structure
- Initial heat, rain, wind, storm, and cold rules
- Alert entity
- Severity and visual-code mapping
- Rule boundary tests

Exit criteria:

- Every forecast is evaluated deterministically.
- Critical alert responses do not depend on an LLM.

### Phase 4 — AI Guidance and Languages

Deliverables:

- Structured generation schema
- Prompt-context builder
- LLM adapter
- Fact and schema validation
- Input-hash caching
- Deterministic fallbacks
- Initial language templates
- Guidance and feedback endpoints

Exit criteria:

- All user modes produce valid structured guidance.
- Invalid AI responses safely fall back.
- P0 languages pass human review for critical templates.

### Phase 5 — Hardening and Beta

Deliverables:

- Structured logging and metrics
- Rate limits
- Error tracking
- Load testing
- Security review
- Data-retention decision
- Staging deployment
- Runbook and rollback procedure

Exit criteria:

- Draft service-level targets are met or documented exceptions are approved.
- Known critical security issues are resolved.
- Beta monitoring and rollback paths are working.

---

## 25. Definition of Done

A backend feature is complete when:

- Requirements and acceptance criteria are linked to the implementation.
- Input and output serializers are defined.
- Authorization is enforced.
- Business logic is implemented in services or domain modules.
- Success and failure paths have tests.
- Logs and metrics are added where operationally useful.
- Errors use standard response codes.
- API documentation is updated.
- Database migrations are reviewed.
- No secrets or personal data appear in logs or source control.
- The frontend can integrate without depending on undocumented behaviour.

---

## 26. Draft Data Retention

These values are starting proposals and require product and privacy review.

| Data | Proposed retention |
|---|---|
| Active user profile and locations | Until account deletion |
| Weather snapshots | 48 hours, with aggregation later if needed |
| Generated guidance | 30 days |
| Feedback | Until account deletion or anonymization |
| Operational logs | 30 days |
| Security audit events | 90 days |

---

## 27. Open Decisions

| Decision | Why it matters | Status |
|---|---|---|
| Launch languages | Determines templates, review effort, and QA scope | Open |
| Hyperlocal precision | Affects cache keys, provider cost, and user expectations | Open |
| Weather provider | Determines coverage, fields, quotas, reliability, and licensing | Open |
| Official alert source | Distinguishes official warnings from TerraCast risks | Open |
| Risk thresholds | Must be scientifically reviewed and region-aware | Open |
| LLM provider and model | Affects cost, latency, language quality, and structured-output reliability | Open |
| Free versus Pro split | Affects API limits and future entitlement design | Deferred to v2 |
| Hosting platform | Affects deployment, networking, observability, and cost | Open |
| Account deletion policy | Required before public launch | Open |

---

## 28. Future Extensibility

The v1 design allows future expansion without implementing it prematurely.

### TerraCast Pro

Add an entitlement layer around advanced endpoints and forecast limits. Paid logic must not be hard-coded inside weather services.

### Meteon Reports

Reuse normalized snapshots, risk results, and guidance services. Scheduled report generation can later use a background task queue.

### Meteon API

Introduce organizations, API keys, scopes, quotas, usage records, and billing. Public API contracts should remain separate from internal frontend APIs.

### Meteon MCP

Expose carefully scoped tools built on the same application services. MCP tools must not bypass authorization, rate limits, or domain rules.

### Meteon Research

Build a separate analytical data pipeline instead of turning the transactional application database into a research warehouse.

---

## 29. Glossary

| Term | Meaning |
|---|---|
| Hyperlocal | Weather information tied to precise coordinates or a small locality |
| Normalization | Converting provider-specific data into TerraCast's stable internal schema |
| Snapshot | A stored weather response with source and freshness metadata |
| Risk engine | Deterministic code that evaluates weather facts against versioned rules |
| Guidance | User-mode and language-specific interpretation of weather facts |
| Visual code | Stable identifier used by the frontend to select an icon or animation |
| Freshness | Whether forecast data is fresh, safely stale, or unavailable |
| Provider adapter | Code that isolates an external provider from the rest of the application |
| Fallback template | Deterministic guidance used when AI generation is unavailable or invalid |
| ADR | Architecture Decision Record explaining an important technical decision |

---

## 30. Final v1 Engineering Position

TerraCast v1 should be built as a modular Django REST API backed by PostgreSQL and authenticated through Supabase. Weather data is normalized behind a provider interface, cached as versioned snapshots, and evaluated by deterministic risk rules. AI generates personalized and regional-language explanations from these verified facts, with strict schema validation and template fallbacks.

This approach keeps the first build understandable and testable while preserving clean extension points for Redis, background workers, subscriptions, public APIs, MCP tools, and research systems in later versions.

> [!warning] Product safety statement
> TerraCast provides decision-support information and does not replace official meteorological or emergency alerts. Official sources and emergency guidance must take priority during severe weather.

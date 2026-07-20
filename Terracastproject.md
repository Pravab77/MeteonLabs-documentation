## Project Documentation

- [[PRD|TerraCast PRD]]
- [[Architecture]]
- [[Rules]]
- [[Phases]]
- [[Design]]
- [[TerraCast|Documentation Hub]]```
## Related Documents

- [[TerraCast Backend Architecture]]
- [[API Documentation]]
- [[Database Design]]
- [[Development Roadmap]]
```

Obsidian may show these links in a lighter color because those notes do not exist yet. Click each link to create the note. Once created, the Graph view will show lines connecting them to `TerraCast PRD.md`.

A good MeteonLabs structure could be:

```
[[Meteon Labs]]
[[TerraCast Backend Architecture]]
[[TerraCast API Documentation]]
[[TerraCast Database Schema]]
[[TerraCast Development Roadmap]]
[[TerraCast AI System]]
```

To force the view to refresh, close and reopen the **Graph view** tab, or press:

```
Ctrl + P
```

Then search:

```
Open graph view
```

The gear icon on the right side of the graph lets you control filters, node sizes, link visibility, and display settings. The graph itself is generated from internal `[[note links]]`, tags, and attachments in your vault.

create content telling about the project i have created another file and we'll link it

Create a new note named:

```
TerraCast Project Overview
```

Paste this into it:

````
---
title: TerraCast Project Overview
project: TerraCast
company: Meteon Labs
status: Planning
market: India
type: AI Weather Intelligence Platform
---

# TerraCast Project Overview

TerraCast is an AI-powered hyperlocal weather intelligence platform being developed by [[Meteon Labs]] for the Indian market.

The project aims to make weather information more understandable, useful, and actionable. Instead of only showing raw values such as temperature, rainfall percentage, wind speed, or humidity, TerraCast explains what the weather means for the user and what actions they may need to take.

The platform is designed for users who may not benefit fully from traditional weather applications, including:

- Farmers
- Outdoor workers
- Elderly users
- Students and daily commuters
- Coastal communities
- Users who prefer regional Indian languages

## Project Vision

The vision of TerraCast is to build an intelligent weather platform that combines:

- Real-time weather data
- AI-generated weather explanations
- Hyperlocal forecasts
- Regional language support
- Severe weather risk analysis
- Cyclone intelligence
- Historical climate insights
- Personalized recommendations

TerraCast is intended to act as a weather decision-support system rather than a basic forecast application.

## Problem Being Solved

Most weather applications provide numerical information without explaining its practical meaning.

For example, a user may see:

```text
Rainfall probability: 80%
Humidity: 92%
Wind speed: 35 km/h
````

However, the user may still not know:

- Whether it is safe to travel
- Whether outdoor work should be postponed
- Whether crops require protection
- Whether heavy rainfall is expected
- Whether coastal conditions are dangerous
- Whether elderly people should avoid going outside

TerraCast converts this raw weather data into clear, user-focused information.

## Core Product Experience

A user selects or searches for a location and receives:

1. Current weather conditions
2. Hourly forecast
3. Seven-day forecast
4. Rain, heat, wind, and storm risk indicators
5. AI-generated weather summary
6. Personalized safety and activity recommendations
7. Regional language explanations
8. Relevant official warnings and alerts

Example AI summary:

> Heavy rainfall is likely during the evening. Avoid unnecessary travel after 5 PM and carry rain protection. Strong winds may affect outdoor activities and temporary structures.

## Primary Features

### Hyperlocal Weather Dashboard

The dashboard provides location-specific information such as:

- Temperature
- Feels-like temperature
- Rainfall probability
- Humidity
- Wind speed and direction
- Air pressure
- Visibility
- Sunrise and sunset
- Hourly forecast
- Seven-day forecast

Related note:

- [[TerraCast Weather Dashboard]]

### AI Weather Summary

TerraCast uses AI to transform forecast data into understandable natural-language summaries.

The AI layer may explain:

- Expected weather conditions
- Travel risks
- Outdoor activity suitability
- Heat-related risks
- Rainfall intensity
- Agricultural impact
- Recommended precautions

Related note:

- [[TerraCast AI System]]

### Cyclone Intelligence

The cyclone module focuses primarily on the Bay of Bengal and Indian coastal regions.

It may provide:

- Cyclone location
- Movement direction
- Estimated wind speed
- Coastal risk level
- Rainfall impact
- District-level warnings
- Cyclone tracking map
- AI-generated impact explanation

Related note:

- [[TerraCast Cyclone Intelligence]]

### Climate Trends

The climate section helps users understand long-term changes in weather patterns.

It may include:

- Historical temperature trends
- Rainfall trends
- Heatwave frequency
- Seasonal comparisons
- Climate anomaly detection
- Location-based climate insights

Related note:

- [[TerraCast Climate Analytics]]

### AI Weather Assistant

Users can ask natural-language questions such as:

- Will it rain this evening?
- Is it safe to travel tomorrow?
- Should I plan outdoor work today?
- Is there a cyclone risk near Odisha?
- Why is the humidity so high?
- What does a heatwave warning mean?

The assistant will answer using current forecast data, historical weather information, official alerts, and TerraCast's internal knowledge base.

Related note:

- [[TerraCast AI Assistant]]

## Target Users

### Farmers

Farmers can receive rainfall, temperature, wind, and crop-related weather guidance.

### Outdoor Workers

Construction workers, delivery workers, street vendors, and field workers can receive practical heat, rain, and storm warnings.

### Coastal Communities

Coastal users can access cyclone, wind, storm surge, and heavy rainfall intelligence.

### Elderly Users

The platform can provide simplified alerts for extreme heat, cold, humidity, and severe weather.

### General Users

Students, commuters, travellers, and families can use TerraCast for daily planning.

## Personalization

TerraCast may support different user modes:

```
General Mode
Farmer Mode
Worker Mode
Elderly Mode
Traveller Mode
Coastal Mode
```

Each mode changes how weather information and recommendations are presented.

For example, the same rainfall forecast may be interpreted differently for a commuter, farmer, or coastal resident.

## Regional Language Support

TerraCast aims to support major Indian languages so that weather information is accessible to users beyond English-speaking audiences.

Initial language support may include:

- English
- Hindi
- Odia

Additional regional languages can be added in later versions.

## Proposed Technology Direction

The first version of TerraCast may use:

### Frontend

- HTML
- CSS
- JavaScript
- Django templates or a modern frontend framework

### Backend

- Python
- Django
- Django REST Framework
- PostgreSQL

### AI Layer

- Large Language Model integration
- Retrieval-Augmented Generation
- Structured weather-data interpretation
- Prompt templates
- Guardrails and response validation

### Infrastructure

- Docker
- Redis
- Background workers
- REST APIs
- Logging and monitoring

The detailed development architecture is documented in:

- [[TerraCast Backend Architecture]]
- [[TerraCast API Documentation]]
- [[TerraCast Database Design]]
- [[TerraCast Development]]

## Product Development Phases

### Phase 1 — MVP

The MVP will focus on:

- Location search
- Current weather
- Hourly forecast
- Seven-day forecast
- AI-generated summary
- Basic rain, heat, and wind risk indicators

### Phase 2 — Personalization

The second phase may introduce:

- User accounts
- Saved locations
- User modes
- Notification preferences
- Regional language support
- Personalized recommendations

### Phase 3 — Advanced Intelligence

The third phase may include:

- Cyclone tracking
- Climate trend analysis
- Historical weather comparisons
- RAG-based weather assistant
- Severe weather notifications
- Agricultural weather intelligence

### Phase 4 — Platform Expansion

Later versions may include:

- TerraCast Pro
- Public developer API
- Research datasets
- Newsletter
- Mobile application
- MCP tools
- Weather intelligence services for businesses

## Project Goals

The main goals of TerraCast are to:

1. Make weather information easier to understand
2. Provide actionable weather guidance
3. Support regional Indian languages
4. Improve access to severe-weather information
5. Build a reliable AI-assisted weather platform
6. Create a scalable product under the Meteon Labs ecosystem

## Success Criteria

TerraCast will be considered successful when users can:

- Search for any supported location
- Understand upcoming weather conditions quickly
- Receive practical recommendations
- Identify important weather risks
- Access information in their preferred language
- Trust the platform's data and explanations
- Use the platform comfortably on mobile and desktop devices

## Project Relationships

- Parent company: [[Meteon Labs]]
- Main requirements: [[TerraCast PRD]]
- Backend design: [[TerraCast Backend Architecture]]
- API design: [[TerraCast API Documentation]]
- Database design: [[TerraCast Database Design]]
- AI architecture: [[TerraCast AI System]]
- Development plan: [[TerraCast Development]]

> [!warning] Product safety  
> TerraCast provides weather decision-support information. It does not replace official meteorological warnings, disaster-management instructions, or emergency guidance
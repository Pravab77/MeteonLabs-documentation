\# Meteon Labs Frontend Architecture



The Meteon Labs frontend is built as a separate Next.js application that communicates with the Django REST Framework backend through HTTP APIs.



\## Core Architecture



```mermaid

graph TD

&#x20;   User\[User Browser] --> Frontend\[Next.js Frontend]

&#x20;   Frontend --> API\[Django REST API]

&#x20;   API --> Database\[(PostgreSQL)]

&#x20;   API --> Redis\[(Redis Cache)]

&#x20;   API --> Weather\[Weather APIs]

&#x20;   API --> AI\[AI and RAG Services]

```



\## Project Structure



\* \[\[Public Assets]]

\* \[\[Application Routes]]

\* \[\[Frontend Components]]

\* \[\[Frontend Services]]

\* \[\[Frontend Types]]

\* \[\[Custom Hooks]]

\* \[\[Frontend Utilities]]

\* \[\[React Context]]

\* \[\[Frontend Configuration]]



\## Pages



\* \[\[Home Page]]

\* \[\[Forecast Page]]

\* \[\[Cyclone Page]]

\* \[\[Climate Page]]

\* \[\[AI Assistant Page]]

\* \[\[Dashboard Page]]



\## Layout Components



\* \[\[Navbar]]

\* \[\[Footer]]

\* \[\[Sidebar]]



\## Weather Components



\* \[\[Current Weather Card]]

\* \[\[Hourly Forecast]]

\* \[\[Seven Day Forecast]]

\* \[\[Weather Map]]

\* \[\[Weather Risk Card]]



\## Cyclone Components



\* \[\[Cyclone Map]]

\* \[\[Cyclone Card]]

\* \[\[Cyclone Timeline]]



\## Climate Components



\* \[\[Rainfall Chart]]

\* \[\[Temperature Chart]]

\* \[\[Climate Summary]]



\## AI Assistant Components



\* \[\[Chat Window]]

\* \[\[Chat Message]]

\* \[\[Prompt Input]]



\## Shared UI Components



\* \[\[Button Component]]

\* \[\[Card Component]]

\* \[\[Input Component]]

\* \[\[Modal Component]]

\* \[\[Skeleton Component]]



\## API Services



\* \[\[API Client]]

\* \[\[Weather Service]]

\* \[\[Cyclone Service]]

\* \[\[Climate Service]]

\* \[\[Assistant Service]]

\* \[\[Authentication Service]]



\## Type Definitions



\* \[\[Weather Types]]

\* \[\[Cyclone Types]]

\* \[\[Climate Types]]

\* \[\[Assistant Types]]

\* \[\[User Types]]



\## Hooks



\* \[\[Use Weather Hook]]

\* \[\[Use Location Hook]]

\* \[\[Use Authentication Hook]]



\## Utilities



\* \[\[Constants]]

\* \[\[Date Formatting]]

\* \[\[Weather Utilities]]

\* \[\[Validation Utilities]]



\## Context Providers



\* \[\[Authentication Context]]

\* \[\[Location Context]]



\## Configuration



\* \[\[Site Configuration]]

\* \[\[Navigation Configuration]]



\## Environment



\* \[\[Frontend Environment Variables]]

\* \[\[Next.js Configuration]]

\* \[\[TypeScript Configuration]]

\* \[\[Frontend Package Configuration]]




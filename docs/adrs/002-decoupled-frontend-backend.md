# ADR-002: Separation av frontend och backend (Headless/Decoupled)

## Status
Godkänd

## Kontext
Webbplatsen är Freaky Fashions huvudsakliga försäljningskanal. Trafiken förväntas öka kraftigt under kampanjer (t.ex. Black Friday)[cite: 26]. Kundupplevelsen kräver snabba laddtider och ett responsivt gränssnitt[cite: 20, 23].

## Alternativ
* **Monolitisk webbapplikation (Server-Side Rendering):** Backend genererar HTML-sidor. Enkelt, men belastar servern vid varje sidvisning.
* **Frikopplad frontend (SPA/Headless) via API:** Frontend (Single Page Application) kommunicerar med backend via ett REST API[cite: 27, 29].

## Beslut
Vi väljer att **separera frontend och backend (Headless/Decoupled)**[cite: 27, 29].

## Motivering
Genom att frikoppla gränssnittet kan statiska filer för frontend levereras snabbt via ett Content Delivery Network (CDN)[cite: 26, 29]. Detta avlastar backend-servern avsevärt vid höga trafiktoppar[cite: 26, 27]. Det möjliggör också att marknadsteamet eller frontend-utvecklarna kan uppdatera gränssnittet utan att påverka affärslogiken i backend[cite: 27].

## Konsekvenser / trade-offs
* **Fördelar:** Bättre prestanda, snabbare laddtider för kunden och oberoende skalbarhet för frontend och backend[cite: 23, 27, 29].
* **Nackdelar / Kostnad:** Kräver hantering av två separata kodbaser och mer initial planering av API-gränssnitten[cite: 29].
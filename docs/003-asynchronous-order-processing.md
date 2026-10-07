# ADR-003: Asynkron orderhantering med meddelandekö

## Status
Godkänd

## Kontext
När en kund slutför ett köp i kassan måste ordern registreras, lagersaldot uppdateras i logistiksystemet och en orderbekräftelse skickas via e-post[cite: 22]. Om det externa lagersystemet eller e-posttjänsten drabbas av fördröjningar eller avbrott riskerar kassaflödet att låsa sig[cite: 26].

## Alternativ
* **Synkron integration:** Kassan väntar på svar från alla bakomliggande system innan köpet bekräftas till kunden.
* **Asynkron integration via meddelandekö:** Kassan sparar ordern och lägger ett meddelande i en kö. Bakomliggande system behandlar ordern i sin egen takt[cite: 26, 27, 28].

## Beslut
Vi väljer att använda **asynkron meddelandehantering (Azure Service Bus / Message Queue)** för orderbehandling[cite: 26, 27, 28].

## Motivering
Lösningen säkerställer hög tillgänglighet och prestanda i kassan[cite: 23, 26, 27]. Kunden får omedelbart en köpbekräftelse utan att behöva vänta på tröga externa integrationer[cite: 27, 28]. Om lagersystemet ligger nere sparas ordern säkert i kön och behandlas automatiskt när systemet är uppe igen[cite: 26].

## Konsekvenser / trade-offs
* **Fördelar:** Extremt tålig kassa och skydd mot avbrott i externa system[cite: 26, 28].
* **Nackdelar / Kostnad:** Något mer komplex arkitektur, svårare felsökning samt risk för tillfällig inkonsekvens i data (eventual consistency) innan alla system synkroniserats[cite: 28].
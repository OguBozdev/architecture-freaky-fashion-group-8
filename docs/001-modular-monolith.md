# ADR-001: Val av systemarkitektur – Modulär monolit

## Status
Godkänd

## Kontext
Freaky Fashion AB ska expandera sin e-handel i Microsoft Azure[cite: 23, 25]. Systemet förvaltas och vidareutvecklas av ett mindre utvecklingsteam[cite: 24, 25]. Det finns behov av god struktur och möjlighet att växa, men teamet vill undvika den komplexitet i drift, nätverk och övervakning som en fullskalig mikrotjänstarkitektur medför[cite: 24, 26].

## Alternativ
* **Traditionell monolit:** Enkel att bygga initialt, men riskerar att bli svår att underhålla och skala när kodbasen växer.
* **Modulär monolit:** Tydligt separerade moduler inom samma kodbas. Ger bra struktur och enkelhet vid utveckling och driftsättning.
* **Mikrotjänster:** Hög skalbarhet och oberoende driftsättningar, men kräver betydande resurser för infrastruktur, övervakning och distribuerad felhantering[cite: 26].

## Beslut
Vi väljer att bygga systemet som en **modulär monolit**[cite: 26].

## Motivering
En modulär monolit ger tydliga gränser mellan systemets domäner (t.ex. katalog, kassa, order) utan den höga operativa komplexitet som mikrotjänster innebär[cite: 15, 26]. Det passar perfekt för teamets storlek och gör att vi kan fokusera på affärslogiken[cite: 24, 26]. Om en viss del av systemet i framtiden kräver fristående skalning kan den modulen brytas ut till en egen mikrotjänst.

## Konsekvenser / trade-offs
* **Fördelar:** Enklare utveckling, testning, felsökning och CI/CD-flöden[cite: 15, 23]. Lägre drift- och infrastrukturkostnader[cite: 24, 25].
* **Nackdelar / Kostnad:** Hela applikationen måste driftsättas tillsammans. Det går inte att skala en enskild modul oberoende utan att skala hela instansen[cite: 15, 28].
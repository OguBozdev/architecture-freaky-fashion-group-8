# ADR-004: Strategi för datalagring och databaser i Azure

## Status
Godkänd

## Kontext
Freaky Fashion AB behöver lagra strukturerad order- och kunddata, varierad produktinformation, högupplösta produktbilder samt hantera höga trafiktoppar under reor (t.ex. Black Friday)[cite: 23, 26, 37]. Systemet förvaltas av ett litet utvecklingsteam och ska driftsättas i Microsoft Azure[cite: 24, 25, 41].

## Alternativ
* **Enskild relationsdatabas för allt (även bilder):** Enkelt, men extremt dyrt och trögt vid hantering av bildfiler och hög trafik[cite: 43, 47].
* **Ren NoSQL-lösning (t.ex. Azure Cosmos DB för allt):** Bra skalbarhet, men saknar inbyggt stöd för ACID-transaktioner för order- och betalningsflöden[cite: 41, 42].
* **Polyglot Persistence (Azure SQL + Blob Storage + Redis Cache):** Kombinerar olika lagringstekniker anpassade efter varje datatyps specifika behov[cite: 41, 43, 44, 45].

## Beslut
Vi väljer **Polyglot Persistence** med följande uppdelning i Azure[cite: 41, 43, 44, 45]:
1. **Azure SQL Database** för ordrar, kunder och produktkatalog[cite: 41, 45].
2. **Azure Blob Storage** för produktbilder och mediefiler[cite: 43, 45].
3. **Azure Managed Redis** som cachelager framför produktkatalogen[cite: 44, 45].

## Motivering
Relationaldata hör hemma i Azure SQL där transaktionssäkerhet garanteras[cite: 41]. Genom att flytta bilder till Azure Blob Storage hålls databasen lätt och billig[cite: 43]. Azure Managed Redis säkerställer att vi klarar 10× trafikökningar under kampanjer utan att belasta SQL-databasen onödigt mycket[cite: 26, 44].

## Konsekvenser / trade-offs
* **Fördelar:** Optimal prestanda, hög skalbarhet, låga lagringskostnader för bilder och hög datakonsistens för ordrar[cite: 41, 43, 44].
* **Nackdelar / Kostnad:** Kräver hantering av fler än en Azure-tjänst samt en strategi för att rensa/uppdatera cachen i Redis när produkter ändras[cite: 44, 45].
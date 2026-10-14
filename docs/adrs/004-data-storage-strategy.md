# ADR-004: Strategi för datalagring och databaser i Azure

## Status
Godkänd

## Kontext
Freaky Fashion AB behöver lagra strukturerad order- och kunddata, varierad produktinformation, högupplösta produktbilder samt hantera höga trafiktoppar under reor (t.ex. Black Friday). Systemet förvaltas av ett litet utvecklingsteam och ska driftsättas i Microsoft Azure.

## Översikt över valda lagringslösningar

| Typ av data | Vald databas eller lagringslösning | Motivering |
| :--- | :--- | :--- |
| **Kunder, Ordrar & Betalningar** | Azure SQL Database | Strukturerad data med strikta relationer, ACID-transaktioner och krav på 100% datakonsistens. |
| **Produktkatalog & Attribut** | Azure SQL Database (med JSON-stöd) | Håller nere komplexiteten för det lilla teamet och samlar affärsdata i en hanterad tjänst. |
| **Produktbilder & Media** | Azure Blob Storage | Skalbar och billig objektlagring för ostrukturerade filer som avlastar databasen. |
| **Cachade produkter & Sessioner** | Azure Managed Redis | Blixtsnabb in-memory caching för att klara höga trafiktoppar (10× under reor). |

## Alternativ
* **Enskild relationsdatabas för allt (även bilder):** Enkelt, men extremt dyrt och trögt vid hantering av bildfiler och hög trafik.
* **Ren NoSQL-lösning (t.ex. Azure Cosmos DB för allt):** Bra skalbarhet, men saknar inbyggt stöd för ACID-transaktioner för order- och betalningsflöden.
* **Polyglot Persistence (Azure SQL + Blob Storage + Redis Cache):** Kombinerar olika lagringstekniker anpassade efter varje datatyps specifika behov.

## Beslut
Vi väljer **Polyglot Persistence** med följande uppdelning i Azure:
1. **Azure SQL Database** för ordrar, kunder och produktkatalog.
2. **Azure Blob Storage** för produktbilder och mediefiler.
3. **Azure Managed Redis** som cachelager framför produktkatalogen.

## Motivering
Relationaldata hör hemma i Azure SQL där transaktionssäkerhet garanteras. Genom att flytta bilder till Azure Blob Storage hålls databasen lätt och billig. Azure Managed Redis säkerställer att vi klarar 10× trafikökningar under kampanjer utan att belasta SQL-databasen onödigt mycket.

## Konsekvenser / trade-offs
* **Fördelar:** Optimal prestanda, hög skalbarhet, låga lagringskostnader för bilder och hög datakonsistens för ordrar.
* **Nackdelar / Kostnad:** Kräver hantering av fler än en Azure-tjänst samt en strategi för att rensa/uppdatera cachen i Redis när produkter ändras.
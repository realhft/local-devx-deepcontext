# Orbit Local Test Repo

A tiny repository used to test the Orbit Context Engine locally. It contains fictional facts about the **Greenhouse Platform** team and its garden-tool catalogue service. Nothing here is real.

## 1. Overview

The Greenhouse Platform team was founded on 3 March 2021 by **Marta Okonkwo**, who is still its engineering lead. The team has 7 engineers and one product manager, **Dev Ramanathan**. The service they own is called **Sprout**, a catalogue of garden tools written in Java 17 with Spring Boot.

## 2. Tool catalogue

| Tool | Weight (g) | Shelf | Material |
|---|---|---|---|
| iron pickaxe | 2400 | B2 | iron |
| copper watering can | 950 | C3 | copper |
| bamboo plant label | 12 | A1 | bamboo |
| stainless trowel | 310 | A4 | stainless steel |
| oak-handled rake | 780 | D1 | oak and steel |
| ceramic seed bowl | 540 | C1 | ceramic |

Shelves are coded `A1` to `D4`. Row A holds small hand tools, row B heavy tools, row C containers and row D long-handled tools.

## 3. Opening hours and contacts

- The warehouse is open 08:00 to 18:30 on weekdays and 09:00 to 13:00 on Saturdays. It is closed on Sundays and on **Midsummer Day (24 June)**.
- General questions go to `greenhouse@example.com`.
- Urgent incidents go to the on-call phone `+1-555-0142`. The on-call rotation changes every Monday at 10:00.

## 4. API

| Endpoint | Method | Purpose |
|---|---|---|
| `/tools` | GET | List all tools as JSON |
| `/tools/{name}` | GET | Return one tool, or 404 if unknown |
| `/tools/{name}/reserve` | POST | Reserve a tool for up to 72 hours |
| `/health` | GET | Liveness check, returns `{"status":"ok"}` |

The service listens on port **8080** by default. Set `SPROUT_PORT` to change it. A reservation is limited to **3 tools per customer** at the same time.

## 5. Configuration

| Variable | Default | Meaning |
|---|---|---|
| `SPROUT_PORT` | 8080 | HTTP port |
| `SPROUT_DB_URL` | `jdbc:h2:mem:sprout` | Database connection string |
| `SPROUT_RESERVE_HOURS` | 72 | Maximum reservation length |
| `SPROUT_LOG_LEVEL` | INFO | Log verbosity |

## 6. How to run

```bash
./start.sh --port 8080
```

To run the tests: `./gradlew test`. The project needs Java 17 and Gradle 8.5.

## 7. Release history

- **v1.0.0** (12 May 2021): first release, with `/tools` and `/tools/{name}`.
- **v1.1.0** (9 September 2021): added reservations. The limit was 24 hours.
- **v1.2.0** (2 February 2022): raised the reservation limit from 24 to 72 hours and added `/health`.
- **v2.0.0** (30 November 2022): moved from Java 11 to Java 17 and renamed the old `/items` endpoint to `/tools`.

## 8. Known issues

- Reservations made at exactly midnight are sometimes counted against the previous day (bug **SPR-214**, open since March 2022).
- The `bamboo plant label` weight is rounded to the nearest gram, so it can show as 12 or 13 depending on the scale (bug **SPR-301**, won't fix).

## 9. Frequently asked questions

**Who approves a reservation longer than 72 hours?** Only Marta Okonkwo can approve it, and only for school visits.

**Why was `/items` renamed?** The team decided in v2.0.0 that "tools" described the catalogue better, so `/items` now returns 410 Gone.

**What happens if a tool is not returned in time?** The customer is charged a late fee of 5 coins per day, up to a maximum of 40 coins.

## 10. Team and ownership

| Person | Role | Owns |
|---|---|---|
| Marta Okonkwo | Engineering lead | Reservation rules, release approvals |
| Dev Ramanathan | Product manager | Roadmap, opening hours |
| Ines Falk | Backend engineer | `/tools` API, bug SPR-214 |
| Tomas Reyes | Backend engineer | Database, configuration |

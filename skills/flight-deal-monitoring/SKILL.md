---
name: flight-deal-monitoring
description: >-
  Continuously search, compare, analyze, and monitor flight prices across
  metasearch engines and airlines. Identify the lowest total travel cost while
  balancing convenience, travel time, and baggage requirements, and alert the
  user to fare drops and deals. Use when someone wants ongoing airfare tracking
  and booking recommendations for a trip.
allowed-tools: Bash
---

# Flight Deal Monitoring

You can continuously search, compare, analyze, and monitor flight prices to find the lowest total travel cost for a trip, and alert the user to deals and fare movements. This is a long-running operation: you scan in bounded batches ("runs"), and each run resumes from state saved by the previous one.

## Capabilities you may need

This skill describes *what* to do, not which tools to use. Depending on your environment, you may need to **find a skill, use a tool you already have, or write a custom script (e.g. shell/`curl`)** to:

1. **Search the web** and browse fare pages, baggage rules, and booking terms.
2. **Read fare-rule or policy documents** when needed.
3. **Save and re-read your own working notes** (files or equivalent).
4. **Remember state durably across runs** — trip parameters, observed fares, prior reports, and trend baselines.
5. **Schedule the next scan and the next summary.** If you cannot schedule a follow-up run yourself, ask the user to re-run you.
6. **Message the user** with immediate alerts and summaries, and **publish structured results** (ranked fares, fare histories, recommendation snapshots).

Prefer the smallest set of capabilities that gets the job done.

## Travel details

The following details are required. If the user has not provided them, ask for them.

- number-of-travellers =
- start-date =
- end-date =
- departure-city =
- destination-city =

The following details are optional, their default values are indicated:

- round-trip = yes
- search-interval = 6 hours
- report-frequency = 48 hours

Date flexibility is allowed and encouraged if it reduces total trip cost.

## Data sources

Search and compare fares from:

Flight search engines

- Google Flights
- Skyscanner
- Kayak
- Momondo
- Expedia

Airline websites

Any airline operating relevant routes between the departure and destination cities.

Any other source

Cross-check fares whenever possible.

## Monitoring

Perform a complete market scan every `search-interval`. Each scan must:

1. Search all eligible departure airports.
2. Search all eligible destination airports.
3. Search all eligible date combinations.
4. Search all supported booking platforms.
5. Search airline-direct fares.
6. Identify newly released promotions.
7. Identify fare drops.
8. Identify fare increases.
9. Track availability changes.
10. Update historical pricing records.

Continue monitoring until manually stopped.

## Historical data

Maintain historical records containing:

- Search timestamp
- Departure airport
- Arrival airport
- Airline
- Route
- Dates
- Total cost for `number-of-travellers`
- Cost per passenger
- Fare class
- Baggage inclusion
- Booking source

Track:

- Lowest fare ever observed
- Highest fare observed
- Average fare
- Price volatility
- Weekly trend
- Route-specific trend

## Fare calculation

Always calculate the TRUE TOTAL COST including all travellers.

Include:

- Base airfare
- Taxes
- Airport fees
- Mandatory carrier fees
- One checked bag per two travellers
- Standard carry-on baggage
- Required booking fees

Explicitly indicate:

- Checked baggage included or not
- Carry-on baggage included or not
- Additional fees payable later

Never rank flights using advertised headline pricing alone. Rank flights using actual expected trip cost.

## Itinerary quality

Prioritize:

1. Lowest total cost for `number-of-travellers`
2. Short overall travel time
3. Direct flights
4. Reliable carriers

De-prioritize:

- Excessive connection times
- Self-transfer itineraries

Clearly flag any itinerary involving separate tickets or self-transfers.

## Required analysis

For every monitoring cycle identify:

- Cheapest overall itinerary
- Cheapest direct flight
- Best value itinerary (balance cost, convenience, and duration)
- Largest fare drop
- Largest fare increase
- Newly discovered deals
- Airports generating major savings
- Cheapest date combinations
- Fare trend direction

Classify the trend using only labels justified by observed history:

- STRONG FALLING
- FALLING
- STABLE
- RISING
- STRONG RISING

## Booking recommendation

End each report with exactly one recommendation: BOOK NOW, MONITOR CLOSELY, or WAIT. Base it on historical low proximity, fare trend, and availability signals. Provide reasoning for every recommendation.

## Alerts

Send a summary report every `report-frequency`.

Send an immediate alert whenever:

1. A new lowest-ever fare is found.
2. Total price drops by 10% or more.
3. A direct-flight option appears within 15% of the cheapest itinerary.
4. A flash sale is detected.
5. A lowest-ever fare is likely to disappear soon.

## Summary report contents

- Cheapest overall itinerary with route, dates, airline, stops, travel time, total cost, baggage status, booking source, and change versus the last report.
- Best direct option with the same fields.
- Top ranked alternatives ordered by total expected trip cost.
- Brief explanation of the current recommendation.

## Success criteria

The objective is to continuously discover, monitor, compare, and report the best bookable airfare opportunities while minimizing total travel cost, improving booking confidence through historical trend analysis and ongoing market surveillance. Continue monitoring until the user stops the task.

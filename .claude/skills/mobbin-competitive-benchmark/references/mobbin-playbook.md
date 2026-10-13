# Mobbin playbook for benchmarks

## Tools

- `search_flows`: best for benchmarks. One call returns multi-screen journeys with previews. Use `limit` 2–3 per call.
- `search_screens`: for a single missing moment (a payment sheet, a filter panel) or to check whether an app exists on Mobbin. `mode: "standard"` is faster for existence checks.
- `search_sections`: website sections only (pricing, hero, footer). Rarely needed for app flows.
- Always pass `output_destination: "doc"` and the same `task_intent` across calls.

## Query patterns

Describe one journey with the app name, from where it starts to where it ends:
- "Agoda booking a hotel room from search results to checkout and payment"
- "Trip.com searching hotels by city and dates and browsing hotel results list with filters"
- "Marriott Bonvoy searching for a hotel, selecting a room rate and reserving"

Avoid combining two journeys in one query, negations, or style words.

## Coverage check

Results for a different app mean the requested app isn't indexed. Seen in practice: Traveloka and Tiket.com were not on Mobbin (checked 2026). Booking.com, Agoda, Trip.com, Airbnb, Marriott Bonvoy, World of Hyatt and GoPay were. Re-check rather than relying on this list, because Mobbin's coverage grows.

Good stand-ins by role:
- Regional Asian OTA missing → Trip.com, Agoda
- Chain direct → Marriott Bonvoy, World of Hyatt
- Alternative lodging → Airbnb

## Filling gaps

After the per-app pass, list which stages each app lacks. Run a stage-specific query only for important gaps, e.g. "Agoda searching hotels by destination and dates, browsing results list with filters and sort".

## Local-market patterns

Mobbin captures are often from the US or Singapore, so local payment methods (e-wallets, virtual accounts, minimarket payment, PayLater) may not appear in competitor flows. Search for the pattern itself, e.g. "payment method selection with e-wallets like GoPay or OVO, bank transfer virtual account". Use the results as `Reference` patterns, and keep the competitor cells as "Mobbin + Self-test".

## Reading results

- Look at the images, not the flow names.
- Note the capture context from each screen: currency, city, dates, logged-in state, member status.
- Flow URLs (`https://mobbin.com/flows/<id>`) go in the Evidence Index and pattern links; screen URLs (`https://mobbin.com/screens/<id>`) are fine for single-screen references.

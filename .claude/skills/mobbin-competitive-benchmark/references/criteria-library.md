# Criteria library

## Abandonment drivers (Baymard Institute, used as the "Driver" column for booking and checkout flows)

- Unexpected extra costs (largest cause)
- Forced account creation
- Checkout too long or complicated
- Payment trust
- Total cost not shown upfront
- Plus, for travel: search is the front door (search quality gates everything after it)

Travel abandons harder than general e-commerce, and fees surfacing late is the main reason. Mobile carries most of the recoverable loss.

## Hotel / travel booking (B2C), 6 stages

The full set with observations is in `assets/example-hotel-b2c-spec.json`.

1. **Entry & Search**: search as primary content · date picker shows nights · guest/room picker · pre-search refinement · promo noise around search
2. **Results & Filtering**: all-in price on cards · filter & sort usability · map access · card scannability · urgency & persuasion · sponsored labelling
3. **Property Detail**: photo gallery · review depth · key reasons to book · sticky CTA with price + reassurance · location clarity
4. **Room & Rate Selection**: rate options clear (meal, cancellation, payment timing, guests) · per-night vs total consistency · flexibility tools (price alerts, calendar) · rate-level urgency
5. **Guest Details & Checkout**: steps and progress · forced account / ID steps (Mobbin + Self-test) · booking conditions before payment · price breakdown and late fees · special requests · distractions in checkout
6. **Payment, Trust & Confirmation**: pay now vs later · local payment methods (Mobbin + Self-test) · security reassurance · confirmation clarity · post-booking self-service

## Adapting to other journeys

Keep the same shape: 5–7 stages following the user's journey, 4–6 criteria each, each tied to a driver.

- **E-commerce checkout**: Cart · Delivery · Payment · Review · Confirmation · Post-purchase. Drivers: same Baymard list.
- **Onboarding / sign-up**: Entry · Account creation · Verification/KYC · Personalisation · First value · Notifications/permissions. Drivers: time to first value, drop-off at verification, permission denial.
- **Event ticketing**: Discovery · Event detail · Ticket/seat selection · Checkout · Payment · E-ticket & entry. Drivers: fees surfacing late, seat-map clarity, queue/hold timers, ticket delivery trust.
- **B2B dashboards**: swap "abandonment" for task success: findability, data density, bulk actions, status clarity, error recovery.

Mark criteria that depend on behaviour Mobbin can't capture (speed, errors, logged-out paths, other markets) as "Mobbin + Self-test".

# Spec format for build_benchmark.py

One JSON object. Full worked example: `assets/example-hotel-b2c-spec.json`.

```json
{
  "title": "Hotel Booking (B2C) — Competitive Benchmark built from Mobbin",
  "subtitle": "One line on scope and method.",
  "platforms": [
    {"name": "Booking.com", "on_mobbin": true},
    {"name": "Traveloka", "on_mobbin": false}
  ],
  "readme": [
    {"heading": "HOW THIS DIFFERS FROM A MANUAL BENCHMARK",
     "rows": [["Evidence source", "..."], ["What Mobbin cannot show", "..."]]}
  ],
  "stages": [
    {
      "name": "1. ENTRY & SEARCH",
      "criteria": [
        {
          "criterion": "Search is the primary home content",
          "look_for": "Is the search form above any promo?",
          "driver": "Search is the front door",
          "evidence": "Mobbin",
          "observations": {"Booking.com": "Search card is the first block on the Stays tab."},
          "friction": [],
          "best": "Booking.com",
          "implication": "Put search above any promo."
        }
      ],
      "scores": {"Booking.com": 4}
    }
  ],
  "patterns": [
    {"type": "Copy", "stage": "2. Results", "pattern": "...", "seen_on": "Agoda", "why": "...", "url": "https://mobbin.com/flows/..."}
  ],
  "friction": [
    {"platform": "Airbnb", "device": "iOS (Mobbin)", "step": "Results", "what": "...", "severity": 3, "url": "https://mobbin.com/flows/...", "implication": "..."}
  ],
  "evidence": [
    {"platform": "Booking.com", "flow": "Booking a property", "stages": "3 Detail, 5 Checkout", "screens": 21, "context": "Tokyo listing, USD, 2026 dates", "url": "https://mobbin.com/flows/..."}
  ]
}
```

## Behaviour notes

- **Platforms** become columns in order. `on_mobbin: false` makes the whole column yellow with "Not on Mobbin — self-test", and adds a row to the Evidence Index automatically.
- **observations**: omit a platform to render "Not captured" (grey). Don't write "Not captured" yourself unless you want that exact text.
- **friction**: platform names whose cell should be red for that criterion.
- **evidence**: any value containing "Self-test" is highlighted yellow.
- **scores**: omit a platform to leave the score blank (yellow) and excluded from averages. Scores must be whole numbers 1–5.
- **severity** in friction: 3 likely abandonment, 2 hesitation, 1 minor.
- Stage names should start with "N. " so the short stage label is derived correctly.
- Any number of stages and platforms works; the Scoring Summary, ranks and chart resize to fit.

## Tabs produced

Read Me · Benchmark Matrix · Scoring Summary (formulas + bar chart) · Patterns · Friction Log (with 10 blank yellow rows for self-testing) · Evidence Index.

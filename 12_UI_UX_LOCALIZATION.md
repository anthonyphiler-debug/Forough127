# 12 — UI/UX and Localization

## 1. Product visual direction

The interface must look premium, modern and professionally finished. Do not reduce visual quality to generic admin templates.

Customer panel:
- violet/blue palette;
- restrained glassmorphism;
- clear cards, charts, status indicators and navigation;
- high readability and low visual noise.

Developer/platform panel:
- green/turquoise palette;
- distinct visual identity while sharing design tokens and components.

## 2. Localization

- Persian-first content and navigation.
- RTL layout as first-class behavior.
- Jalali calendar display where users interact with dates.
- Store and exchange timestamps in UTC.
- Default display timezone Asia/Tehran; tenant-level timezone may be configurable.
- Correct Persian digits and locale-aware number formatting where appropriate.
- Avoid mixing RTL and LTR identifiers without directional isolation.

## 3. UX requirements

- Responsive desktop, tablet and mobile layouts.
- Accessible labels, focus states, keyboard navigation and contrast.
- Empty, loading, error, success and permission-denied states for every major screen.
- Confirm destructive actions.
- Explain actionable errors in Persian.
- Do not use color as the only status signal.
- Tables support filtering, sorting, pagination and useful empty states.
- Operational screens expose retry/review actions only to authorized roles.

## 4. Frontend technology

HTML/CSS/JavaScript only; TypeScript is not allowed. Keep components modular. Avoid embedding secrets or authorization logic in the browser. Android delivery later uses Capacitor and should reuse responsive web UI where feasible.

---
name: Znayu
description: A calm, exact operational register for trustworthy self-hosted status communication.
colors:
  page-blue-black: "#0f121a"
  service-blue-black: "#191c24"
  interactive-blue-black: "#21242d"
  structure-muted: "#21242d"
  structure-clear: "#2d313c"
  ink-white: "#ffffff"
  text-secondary: "#a7acbb"
  text-muted: "#8a91a5"
  health-emerald: "#10b981"
  incident-red: "#f87171"
  maintenance-blue: "#60a5fa"
  degraded-amber: "#f59e0b"
typography:
  display:
    fontFamily: "ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
    fontSize: "1.65rem"
    fontWeight: 680
    lineHeight: 1.25
    letterSpacing: "-0.03em"
  headline:
    fontFamily: "ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
    fontSize: "1rem"
    fontWeight: 700
    lineHeight: 1.35
    letterSpacing: "-0.018em"
  title:
    fontFamily: "ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
    fontSize: "0.9375rem"
    fontWeight: 530
    lineHeight: 1.4
    letterSpacing: "-0.012em"
  body:
    fontFamily: "ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
    fontSize: "0.8125rem"
    fontWeight: 400
    lineHeight: 1.55
    letterSpacing: "normal"
  label:
    fontFamily: "ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
    fontSize: "0.6875rem"
    fontWeight: 600
    lineHeight: 1
    letterSpacing: "normal"
rounded:
  segment: "1px"
  control: "5px"
  surface: "12px"
  pill: "999px"
  circle: "50%"
spacing:
  tight: "4px"
  xs: "8px"
  sm: "12px"
  md: "16px"
  lg: "20px"
  xl: "24px"
  xxl: "28px"
  section: "72px"
  section-large: "88px"
components:
  navigation-link:
    textColor: "{colors.text-secondary}"
    typography: "{typography.body}"
    rounded: "{rounded.control}"
    padding: "9px 12px"
  navigation-link-active:
    backgroundColor: "{colors.interactive-blue-black}"
    textColor: "{colors.ink-white}"
    typography: "{typography.body}"
    rounded: "{rounded.control}"
    padding: "9px 12px"
  subscribe-action:
    backgroundColor: "transparent"
    textColor: "{colors.text-secondary}"
    rounded: "{rounded.control}"
    padding: "0 13px"
    height: "36px"
  service-register:
    backgroundColor: "{colors.service-blue-black}"
    textColor: "{colors.ink-white}"
    rounded: "{rounded.surface}"
    width: "100%"
  service-row:
    backgroundColor: "{colors.service-blue-black}"
    textColor: "{colors.ink-white}"
    padding: "24px 22px 19px"
  uptime-segment:
    backgroundColor: "{colors.health-emerald}"
    rounded: "{rounded.segment}"
    height: "25px"
  maintenance-status:
    backgroundColor: "rgb(96 165 250 / 13%)"
    textColor: "{colors.maintenance-blue}"
    typography: "{typography.label}"
    rounded: "{rounded.pill}"
    padding: "5px 8px"
  incident-status:
    backgroundColor: "rgb(16 185 129 / 14%)"
    textColor: "{colors.health-emerald}"
    typography: "{typography.label}"
    rounded: "{rounded.pill}"
    padding: "5px 8px"
---

# Design System: Znayu

## Overview

**Creative North Star: "The Operational Register"**

Znayu presents status information like a precise public record: calm enough to scan under pressure, exact enough to earn trust, and polished enough to feel commercially finished without disguising its self-hosted character. The visual system uses familiar status-page grammar, but its centered navigation, horizontal health masthead, unlabelled service register, and quiet editorial history create an independent composition.

The system is restrained and legible. Deep blue-black layers hold compact white type, operational color appears only where it communicates state, and the primary service surface carries almost all of the first-view information without becoming a dashboard collage. The interface rejects gradients, glass, neon, ornamental imagery, and exposed monitor-configuration metadata.

**Key Characteristics:**

- Calm, exact, and modern operational presentation.
- Compact hierarchy within an 800px centered reading column.
- One continuous service register instead of a collection of cards.
- Sparse semantic color and flat structural depth.
- Native system typography with tabular operational numerals.

## Colors

The palette is a deep blue-black neutral field with white information hierarchy and narrowly assigned operational accents.

### Primary

- **Health Emerald:** The sole positive-state accent for overall health, service status, operational uptime segments, focus outlines, and resolved-state markers.

### Secondary

- **Maintenance Blue:** Reserved for scheduled-maintenance status; its translucent field distinguishes planned work from health and incident states.

### Tertiary

- **Incident Red:** Marks unavailable uptime segments and active incident severity without bleeding into healthy or resolved communication.
- **Degraded Amber:** Marks degraded uptime segments; it remains distinct from the blue used for scheduled work.

### Neutral

- **Page Blue-Black:** The full-page foundation and browser scrollbar track.
- **Service Blue-Black:** The continuous service register and the cutout around incident timeline markers.
- **Interactive Blue-Black:** Hover and active navigation fill.
- **Muted Structure:** Quiet surface borders and editorial separators.
- **Clear Structure:** Higher-contrast dividers, control outlines, and the masthead rule.
- **Ink White:** Primary headings, service names, and uptime values.
- **Secondary Text:** Navigation, descriptions, operational summaries, and range labels.
- **Muted Text:** Timestamps, dates, and tertiary footer information.

### Named Rules

**The Operational Color Rule.** Emerald means healthy or resolved, red means unavailable, blue means scheduled maintenance, and amber means degraded; never use these accents as decoration.

**The Blue-Black Field Rule.** Preserve the close tonal relationship between page, primary surface, hover fill, and dividers so structure reads through restraint rather than contrast-heavy panels.

## Typography

**Display Font:** Native system sans (with ui-sans-serif and platform fallbacks)
**Body Font:** Native system sans (with ui-sans-serif and platform fallbacks)

**Character:** A compact, contemporary sans hierarchy keeps the page familiar and highly legible. Tight negative tracking is reserved for the wordmark and primary headings; tabular numerals stabilize uptime percentages and times.

### Hierarchy

- **Display** (680, 1.65rem, 1.25): The horizontal overall-health statement; it reduces to 1.3rem on narrow mobile screens.
- **Headline** (700, 1rem, 1.35): Editorial section headings for maintenance and incident history.
- **Title** (530, 0.9375rem, 1.4): Service names and incident titles, with modest negative tracking on the service register.
- **Body** (400, 0.8125rem, 1.55): Operational summaries, descriptions, navigation, and update copy; event text stays within 48–62 characters per line where the layout permits.
- **Label** (600, 0.6875rem, 1): Compact status pills and the quietest range or footer annotations.

### Named Rules

**The Native Precision Rule.** Use the system sans stack throughout; establish hierarchy with measured size, weight, spacing, and tabular numerals rather than a display typeface.

## Layout

The public page uses one centered reading column capped at 800px, with 20px desktop gutters, 16px compact-tablet gutters, and 12px narrow-mobile gutters. Its header is a three-column grid: wordmark left, exactly centered three-item navigation, and Subscribe action right. Below it, a horizontal masthead pairs the overall state with a right-aligned service count and timestamp.

The service register is self-explanatory and therefore has no redundant section title. Four service rows share one continuous surface, separated by hairlines. Each row places identity and state on one line, followed by a 25px uptime rail: 90 one-day segments on desktop and the latest 30 on screens up to 520px. Maintenance and incident history sit below the fold with editorial spacing rather than competing in the first viewport.

At 760px, gutters and masthead spacing tighten while event layouts simplify. At 520px, the header becomes two rows with centered navigation, the masthead stacks, service results become vertical, and maintenance collapses to a single column.

### Named Rules

**The One Register Rule.** Services belong in one continuous reading surface; do not split them into independent dashboard cards.

**The Recent Thirty Rule.** Preserve all 90 daily segments on desktop and show only the latest 30 on narrow mobile screens.

## Elevation & Depth

The system is flat by default. Page, service surface, hover fill, borders, and hairlines create depth through neighboring blue-black tones. The primary service register alone receives an almost-imperceptible structural shadow; navigation, maintenance, incidents, and status pills remain unraised.

### Shadow Vocabulary

- **Register Structural Shadow** (`0 4px 7px rgb(0 0 0 / 3%)`): Applied only to the primary service register to separate the continuous operational surface from the page.
- **Health Halo** (`0 0 0 6px rgb(16 185 129 / 12%)`): A state signal around the overall-health dot, not a general elevation effect.

### Named Rules

**The One Shadow Rule.** Only the primary service surface may cast structural elevation, and that shadow must remain barely perceptible.

## Shapes

The form language is gently restrained: the primary register uses a 12px radius, navigation and compact controls use 5px corners, and status labels become full pills only when their compact semantic role benefits from enclosure. Uptime segments are nearly square with 1px rounding, while their first and last segments receive 4px outer corners to make the rail read as one bounded sequence. Circular dots communicate health and timeline state.

Borders are 1px and use the two structural neutrals. Avoid stacked outlines, decorative containers, and exaggerated rounding.

### Named Rules

**The Role-Shaped Corner Rule.** Use broad rounding for the primary register, modest corners for controls, pills only for compact statuses, and near-square geometry for data.

## Components

### Navigation

The header feels balanced rather than symmetrical by width: the navigation remains exactly centered while the wordmark and action align independently.

- **Wordmark:** Lowercase text, white, compact and tightly tracked; no invented logo mark.
- **Links:** Secondary text on transparent backgrounds with 5px corners and 9px by 12px padding.
- **Hover / Active:** Shift to white text on the interactive blue-black fill over 140ms.
- **Mobile:** Keep all three labels on one horizontally scrollable centered row beneath the wordmark and action.

### Buttons

The Subscribe action is quiet and clearly secondary to health information.

- **Shape:** Modestly rounded control (5px) with a 1px clear-structure border.
- **Primary:** No primary call-to-action exists on this read-only surface.
- **Secondary:** Transparent fill, secondary text, 36px desktop height, and compact horizontal padding.
- **State:** The prototype action is disabled without dimming; keyboard focus still uses the global emerald outline when an action is enabled.

### Cards / Containers

The service register is the only card-like container and behaves as a continuous ledger.

- **Corner Style:** Gently rounded primary surface (12px).
- **Background:** Service blue-black against the darker page field.
- **Shadow Strategy:** Register Structural Shadow only.
- **Border:** One quiet 1px perimeter with clearer 1px row separators.
- **Internal Padding:** 24px 22px 19px per desktop row; 20px 14px 17px on narrow mobile.

### Uptime Rails

The rail is the signature operational component: compact, exact, and readable as a time sequence without a chart frame.

- **Geometry:** A 25px-tall grid of 90 one-day segments with 1px gaps and near-square corners.
- **State:** Health Emerald is the default, Degraded Amber marks partial impairment, and Incident Red marks unavailability.
- **Range:** Label the beginning and Today below the rail; switch from 90 days ago to 30 days ago when older segments are hidden on mobile.

### Status Pills

Pills are reserved for scheduled and resolved event status, not ordinary service health.

- **Scheduled:** Maintenance Blue text on a subtle translucent blue field.
- **Resolved:** Health Emerald text on a subtle translucent emerald field.
- **Shape:** Fully rounded with 5px by 8px padding and compact label typography.

### Event History

Maintenance and incident information reads like a quiet operational log below the service register.

- **Maintenance:** Date, title and description, then a Scheduled pill; collapse to one column on narrow mobile.
- **Incident:** Date divider, incident title with Resolved pill, and one timeline update connected by a subtle vertical rule.
- **Density:** Use hairlines and whitespace rather than enclosing each event in a card.

## Do's and Don'ts

### Do:

- **Do** keep the primary public reading column centered and capped at 800px.
- **Do** make service health understandable without a heading or explanatory dashboard chrome.
- **Do** preserve 25px daily rails with 90 segments on desktop and the latest 30 on mobile.
- **Do** use operational colors only for the states they name.
- **Do** keep maintenance and incident history quiet and below the primary service surface.

### Don't:

- **Don't** expose monitor configuration such as protocols, methods, endpoints, or ports on the public page.
- **Don't** turn the service register into separate cards or a dashboard collage.
- **Don't** use gradients, glass effects, neon accents, ornamental imagery, or decorative data visualization.
- **Don't** copy Better Stack or ReelFlix composition; use them only as a quality and density reference.
- **Don't** add shadows beyond the primary register's subtle structural shadow.

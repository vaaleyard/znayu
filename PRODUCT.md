# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

Delegated: SvelteKit with TypeScript. It avoids React while providing a component model suitable for a responsive public status page now and a more interactive administration interface later. The application will eventually be distributed with Docker; the deployment architecture remains open.

## Users

- Operators who understand the monitored systems and configure monitors, publish maintenance windows, and manage status information.
- Visitors who open the public status page to verify whether services are operating normally and to understand incidents or maintenance.

## Product Purpose

Znayu is a self-hosted status-page product. It lets operators publish monitor updates and maintenance information, and gives visitors a clear, trustworthy view of current service health and recent uptime.

The initial phase is interface-only: build realistic screens with sample data, without monitoring, persistence, authentication, or other application logic. The first surface is the public status page.

## Positioning

Znayu combines self-hosted ownership with the presentation quality and professional credibility commonly associated with polished commercial status-page products. It is intended as a more considered alternative to self-hosted tools whose public interfaces feel utilitarian or visually unfinished.

## Operating Context

Operators configure monitors and maintenance information. Visitors consult a responsive public web page on desktop or mobile when checking whether services are healthy, degraded, under maintenance, or unavailable.

The first public status page will show sample monitored services and uptime history, with each segment in an uptime bar representing one day.

## Capabilities and Constraints

- Public presentation of monitored services and their current health.
- Uptime history for each monitored service.
- Publication and viewing of monitor updates.
- Publication and viewing of maintenance information.
- Responsive behavior across desktop and mobile web.
- Self-hosted distribution, with Docker planned for a later phase.
- No product logic is part of the initial interface phase; screens use realistic sample data.
- Monitoring protocols, persistence, authentication, notification channels, and deployment architecture are deliberately undecided.

## Brand Commitments

- Product name: Znayu.
- The interface must feel attractive, professional, intentional, and credible rather than visually resembling a generic AI-generated template or an unfinished open-source utility.
- Better Stack is a user-provided quality reference, not a design to copy.
- Public status pages should remain visually simple and direct: concise navigation, immediate overall health, one unified service group, uptime history, and quiet event history without a dominant visual metaphor. Better Stack's ReelFlix status page is a user-provided reference for hierarchy and density, not a brand or layout to copy literally.
- The public reading column should feel compact rather than spanning most of a desktop viewport (800 px for the current dark composition), with a contemporary commercial-product finish rather than a legacy status-page or administrative-template appearance.
- The public page must present service names, health, uptime, incidents, and maintenance without exposing monitor-configuration details such as HTTP/HTTPS methods, TCP protocols, or port numbers.

## Evidence on Hand

There are currently no product assets, production data, customer claims, testimonials, benchmarks, or existing interface components. Future work must not fabricate proof or operational claims.

## Product Principles

1. Make system health understandable at a glance without hiding meaningful detail.
2. Treat the public status page as a trust surface, not merely an operational dashboard.
3. Pair self-hosted control with the finish and coherence expected from a commercial product.
4. Keep operator workflows and visitor-facing communication distinct while maintaining one product language.
5. Introduce application logic only after the interface direction is established and approved.

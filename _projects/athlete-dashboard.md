---
title: "Nomad Baseball Athlete Dashboard"
excerpt: "A comprehensive R Shiny dashboard designed to function as a living, coach-facing scouting report — consolidating eight distinct data domains per athlete into a single interactive interface."
tools: "R · R Shiny · Plotly · dplyr"
header:
  teaser: /assets/images/dashboard-teaser.png
---

## Summary

Coaches at a high-volume training facility like Nomad Baseball work with a large and constantly rotating roster of athletes across different backgrounds, competition levels, and physical profiles. Before each session, a coach needs to know who they're working with — not just their name, but their movement tendencies, where they've been, what they've tested, and what the plan is.

That information existed, but it was scattered. I built the Nomad Athlete Dashboard to consolidate everything into a single coach-facing interface that requires no technical knowledge to use. The goal was to reduce session prep time and give every coach instant context on every athlete.

---

## Data Domains

The dashboard organizes each athlete's profile across eight tabs:

| Tab | Contents |
|---|---|
| **Athlete Overview** | Name, position, level, organization, and key background info |
| **Motor Preferences** | Movement tendencies and response patterns used to guide coaching cues |
| **Power Testing** | Raw output numbers and percentile scores from Nomad's testing battery |
| **Pitching Data** | Trackman metrics including velocity, spin rate, movement profiles, and pitch mix |
| **Athlete Context** | Background notes, training history, and relevant personal context |
| **Injury History** | Previous injuries, current limitations, and return-to-performance notes |
| **Posture Notes** | Structural and postural observations that inform movement coaching |
| **Athlete Plan** | Individualized development plan with prioritized focus areas |

---

## Design Philosophy

The dashboard was built specifically for non-technical end users. Coaches navigate the full athlete profile through a dropdown selector and tabbed layout — no spreadsheets, no digging through files. Every piece of information is where they expect it to be.

The motor preferences module was designed with future expansion in mind. The movement tendency and response pattern data it captures is intended to feed a planned drill recommendation engine that will algorithmically suggest targeted drill work based on an athlete's motor profile and performance data.

Similarly, a planned future feature will embed quick-access throwing video directly within each athlete's profile, pairing visual movement context with the quantitative data already present.

---

## Links

[Live App](https://YOUR-SHINYAPPS-LINK-DASHBOARD){: .btn .btn--primary}
[GitHub Repository](https://github.com/YOUR-GITHUB-USERNAME/nomad-athlete-dashboard){: .btn .btn--inverse}

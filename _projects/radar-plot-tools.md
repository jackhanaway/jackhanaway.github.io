---
title: "Nomad Baseball Athlete Radar Plot Tools"
excerpt: "Interactive R Shiny applications that profile high school and college athletes across five power development metrics, normalized to population percentiles."
tools: "R · R Shiny · Plotly · dplyr"
header:
  teaser: /assets/images/radar-plot-teaser.png
---

## Summary

During my time as a Performance & Data Analytics Intern at Nomad Baseball, one of the first needs I identified was a faster, more visual way for coaches to understand where each athlete stood physically relative to their peers. Nomad tests all incoming athletes across a standardized battery of power metrics, but that data was living in spreadsheets — not in a format a coach could pull up in seconds before a session.

I built two R Shiny applications to solve this: one for the high school population and one for the college population. Each app generates an interactive radar plot that instantly profiles an athlete across five metrics and places them in context against their peer group.

---

## Metrics

Each athlete is profiled across five power development metrics that are standard in Nomad's testing protocol:

- **Body Weight** — baseline physical size context
- **Trap Bar Deadlift Speed (1.25% BW)** — lower body power and bar speed
- **Bench Press Speed (0.75% BW)** — upper body power and pressing speed
- **Countermovement Jump (CMJ)** — reactive lower body power and athleticism
- **Med Ball Shot Put — Left & Right (4lb)** — rotational power output, bilateral

These metrics were chosen because they are measurable, reproducible, and directly relevant to baseball performance demands.

---

## Methodology

Raw testing values are converted to percentile scores using empirical cumulative distribution functions (ECDF) calculated within each population (high school athletes and college athletes separately). This means a 70th percentile score on CMJ tells a coach that the athlete jumps higher than 70% of the athletes Nomad has tested at that level.

The radar plot visualizes all five percentile scores simultaneously, making it immediately clear where an athlete's physical strengths and development opportunities lie. An "Average Player" benchmark is included in every chart to provide a stable visual reference point.

---

## App Features

- Athlete selector dropdown — coaches can pull up any athlete by name in seconds
- Dual display — radar plot shows both percentile scores and raw output values
- Separate HS and college population apps — ensures comparisons are within the correct competitive tier
- Built for non-technical end users — no code interaction required

---

## Links

[High School Athlete App](https://nomadbaseball.shinyapps.io/NomadHighSchoolPowerApp/){: .btn .btn--primary}
[College Athlete App](https://nomadbaseball.shinyapps.io/CollegePowerApp/){: .btn .btn--primary}
[GitHub Repository](https://github.com/jackhanaway/nomad-radar-plots){: .btn .btn--inverse}

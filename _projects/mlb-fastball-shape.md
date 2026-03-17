---
title: "MLB Fastball Shape and Hitter Performance"
excerpt: "A Python research project analyzing how elite fastball movement shapes, quantified through composite metrics built from Statcast data, create measurable, statistically significant differences in hitter performance and swing mechanics."
tools: "Python · pandas · pybaseball · Statcast · scipy"
header:
  teaser: /assets/images/fastball-teaser.png
---

## Summary

Not all fastballs are created equally, and not all hitters handle these different fastball shapes equally well. This project was built to move beyond traditional velocity-and-spin summaries and quantify fastball movement in a way that examines the relationship between fastball shape, measured by “ride” and “run”, and hitter performance, identifying outlier pitch shapes, notable hitter tendencies, and statistically significant patterns to inform coaching and scouting

Using Statcast pitch-tracking data pulled via the `pybaseball` library in Python, I engineered two composite movement metrics — **True Ride** and **True Run** — that better capture the "active" movement component of a fastball than raw break measurements (induced vertical break and horizontal break) alone. I then linked elite pitch-shape percentiles to hitter performance outcomes and swing mechanics, using t-tests and distributional analysis to identify statistically significant differences.

This project was completed as part of RSM 360 — Analytical Methods in Sports at the University of Tennessee.

## True Ride and True Run Formulas

True Ride is a weighted composite of five z-scored components pulled from Statcast, scaled to a league-average index of 100 with a standard deviation of 15. While the metric seeks to quantify a fastball's ride outside of standard ball flight metrics, Induced Vertical Break has the largest weight in the formula, representing the theoretical movement of a pitch if thrown from a flat surface in a vacuum. To account for release mechanics and pitch trajectory, Vertical Assault Angle (VAA) was included as the second largest weight in the True Ride formula,  describe the vertical angle at which the ball crosses the strike zone, incorporating release height, arm angle, spin rate, location, and movement. A flatter (lesser) VAA was rewarded in the formula, as the flatter approach angle enhances perceived ride from the hitter's perspective. Smaller weights incorprated include rewarding of a greater release extension, a lower release height, and higher velocity. The values of the five weighted components are:

| Component | Weight | Direction |
|---|---|---|
| Induced Vertical Break (IVB) | 0.40 | Higher = better |
| Vertical Approach Angle (VAA) | 0.30 | Flatter = better |
| Release Extension | 0.20 | More = better |
| Velocity | 0.05 | Higher = better |
| Release Height | 0.05 | Lower = better |

True Run follows the same z-score and scaling framework as True Ride, calculated separately for RHP and LHP to account for the mirrored direction of arm-side run. Horizontal break (positive for RHP, negative for LHP) remains the top influencing factor for True Run, with HAA, a quantification of the lateral angles at which the ball crosses the strike zone, as the second highest. While greater extension and higher velocity remain rewarded with a low weight, a higher release point is rewarded for the True Run metric, as higher release points are usually associated with ride fastballs. The values of the five weighted components are: 

| Component | Weight | Direction (RHP) |
|---|---|---|
| Horizontal Break (HB) | 0.45 | Higher arm-side = better |
| Horizontal Approach Angle (HAA) | 0.30 | Higher arm-side = better |
| Release Extension | 0.15 | More = better |
| Velocity | 0.05 | Higher = better |
| Release Height | 0.05 | Higher = better |

*For LHP, HB and HAA signs are flipped to reward arm-side run in the 
opposite direction.*

---

## Data

- **Source:** MLB Statcast via `pybaseball`
- **Season:** 2025 (full season)
- **Volume:** 770,428 pitch records
- **Hitter sample:** Qualified MLB non-pitchers cross-referenced against Razzball projections data
- **Key Statcast fields used:** `pfx_x`, `pfx_z` (raw break), `release_spin_rate`, `vz0`, `vy0`, `vx0` (velocity components), `attack_angle`, `attack_direction`, `swing_path_tilt`, `estimated_ba_using_speedangle`, `barrel`

---

## Composite Metrics

### Mean True Ride (Vertical)
Mean True Ride measures how much a four-seam fastball fights gravity relative to other MLB fastballs on average, to identify pitchers whose fastballs arrive at the plate with a genuinely flatter trajectory, giving the most perceived "rise". 

**Top Mean True Ride pitchers (2025):** Jacob Misiorowski, Alexis Díaz, Jeremiah Estrada, Devin Williams, Jordan Romano

**Key Takeaways**: Top true ride fastballs vary between leveraging high average IVB (Estrada, Romano) and medium IVB with exceptionally flat VAA (Misiorowski, Díaz)

### Mean True Run (Horizontal)
Mean True Run applies the same philosophy horizontally: how much a fastball runs armside compared to other MLB fastballs on average, to identify pitchers whose fastballs arrive at the plate with greater armside movement, combined with angles produced by high HAA.  

**Top True Run RHP (2025):** Matt Bowman, McCade Brown, Ryan Walker, Victor Mederos

**Top True Run LHP (2025):** Hoby Milner, Kyle Backhus, Nick Lodolo, Kody Funderburk

**Key Takeaways**: Contrary to Mean True Ride, most top True Run pitchers among both LHP and RHP have similar approach styles: generating high HB from a low release point.

---

## Key Findings

### Hitter Performance vs. Elite Ride
Among hitters facing top-quintile True Ride fastballs (≥80th percentile), barrel rates and hits-per-pitch varied significantly. To better understand a hitter's ability to "square up" a fastball with elite ride, barrel rate was prioritized for analysis purposes. The most interesting takeaway from this portion of the study was that then-Blue Jays shortstop Bo Bichette had the highest barrel rate in the MLB vs both top-quintile True Ride and True Run fastballs in 2025, indicating a model swing type to combat outlier fastballs. Other elite ride fastball hitters in 2025 include Adam Frazier, Ezequiel Tovar, and Mike Trout. Poor ride fastball hitters include Matt Wallner, Isaac Collins, and suprisingly, Corey Seager. 

### Hitter Performance vs Elite Run
Since the majority of pitchers in our dataset with elite Mean True Run are relievers, analyzing over/underperformers against this pitch type bode especially useful for practical use, such as bullpen and pinch hitting decisions. Top performers in barrel rate against elite run RHP fastballs include expected appearances such as Bo Bichette (*80% barrel rate*) and Fernando Tatis Jr, as well as more under-the-radar players like Ty France and Matt Shaw. As for elite run LHP, data is likely skewed from relievers in this category often entering the game to face left handed batters, but the top ten performers are all left handed hitters, with Cedric Mullins and Jung Hoo Lee leading the way.


### Swing Mechanics Split vs Elite Ride Fastballs
Hitters were split into top-10 and bottom-10 groups by barrel rate against elite ride fastballs. Their swing mechanics showed the following mean differences:

| Metric | Top 10 | Bottom 10 |
|---|---|---|
| Attack Angle | -0.43° | +5.73° |
| Attack Direction | 13.06° | 5.70° |
| Swing Path Tilt | 33.48° | 28.73° |

### Statistical Significance (t-tests)
All three swing mechanic differences between top and bottom performers were statistically significant:

| Metric | t-statistic | p-value |
|---|---|---|
| Attack Angle | -3.742 | **0.0019** |
| Attack Direction | 2.717 | **0.0165** |
| Swing Path Tilt | 2.529 | **0.0215** |

Hitters who perform best against elite ride fastballs tend to have **lower, more level attack angles** and **greater swing path tilt** — mechanical signatures consistent with a swing designed to match the flatter trajectory of a true ride pitch.

---
## Player-Specific Insights

One of the most valuable outputs of this analysis is its ability to generate 
individual scouting and development narratives from swing mechanic and 
performance data. Three players stood out as particularly instructive case 
studies.

### Bo Bichette — Shape-Independent Fastball Proficiency
New Mets shortstop Bo Bichette ranked as the top performer in barrel rate 
against both elite True Ride and RHP True Run fastballs, the two most 
challenging pitch shapes examined in this study. His balanced attack direction 
and consistent swing-path tilt allow him to handle rising four-seams at the 
top of the zone and heavy arm-side run equally well. Bichette's results 
quantify what scouts have long described anecdotally: his deep turn, long bat 
path, and elite hand-eye coordination produce a swing that is capable 
of handling a variety of fastball shapes, a rare and highly valuable trait.

### Isaac Collins — High Contact, Low Barrel Rate Outlier
Now-Royals outfielder Isaac Collins presents one of the more analytically 
interesting profiles in the dataset. He appears in the top 10 in hits per 
pitch against elite True Ride fastballs, yet simultaneously ranks among the 
bottom 10 in barrel rate against both elite True Ride and LHP True Run fastballs. His 
swing-metric profile explains the contradiction: Collins consistently matches 
*location* rather than *movement* with his directional intent, producing high 
contact rates but limiting his ability to generate driven contact against 
pitches requiring a movement-adjusted bat path. Against elite LHP run 
specifically, his attack direction turns negative (–2.06°), meaning his bat 
path cuts across arm-side movement rather than matching it. Small adjustments, such 
as a slightly higher attack angle against ride, and a few degrees of positive 
attack direction against LHP run, could convert elite bat-to-ball skill into 
meaningfully higher quality contact without disrupting his contact-rate 
foundation.

### Junior Caminero — Split-Driven Development Opportunity
Rays star shortstop Junior Caminero finished ninth in AL MVP voting at age 22, but 
his performance against LHP True Run fastballs flags a clear developmental 
area. In 2025, Caminero recorded zero barrels against 33 elite-running fastballs
against left-handed pitchers. His attack angle against elite LHP run is steeply positive
(+7.85°) and his attack direction is strongly negative (–4.76°), a combination that 
produces a bat path working across arm-side horizontal movement rather than 
matching it. His relatively shallow swing-path tilt (~25.9°) further limits 
his ability to stay on these pitches through the contact zone. Modest 
adjustments, a more positive attack direction and a slightly reduced attack 
angle in run-heavy zones, could meaningfully reduce this vulnerability without 
altering the power profile that makes him one of the game's most exciting young 
hitters. Caminero's case illustrates how shape-specific swing mismatches can 
exist even in elite offensive profiles, and how this type of analysis can 
identify them precisely. From an opposing perspective, pitching coaches and coordinators
could use these findings to inform bullpen decisions against Caminero, such as using
a low-slot, high armside run lefty to face Caminero out of the bullpen when possible.

---

## Implications

- **Matchup evaluation:** A pitcher's True Ride percentile can flag hitters who are mechanically vulnerable based on their attack angle profile
- **Pitch design:** Pitchers seeking to develop a ride fastball can target IVB and VAA thresholds associated with elite True Ride scores
- **Hitting development:** Hitters whose attack angles trend high may need mechanical adjustments to handle elite rising fastball shapes

---

## Links

[View Full Research Paper](https://docs.google.com/document/d/1F70YUd7Yf9Abre2wxV-_AhaQDgZVOCGotVBlFdrEcCg/edit?tab=t.0#heading=h.8wwqq1g9de91){: .btn .btn--primary}
[View Jupyter Notebook on GitHub](https://github.com/jackhanaway/mlb-fastball-shape/blob/main/MLBvsFastballShape.ipynb){: .btn .btn--inverse}

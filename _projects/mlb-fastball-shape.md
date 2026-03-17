title: "MLB Fastball Shape & Hitter Performance"
excerpt: "A Python research project analyzing how elite fastball movement shapes — quantified through composite metrics built from Statcast data — create measurable, statistically significant differences in hitter performance and swing mechanics."
tools: "Python · pandas · pybaseball · Statcast · scipy"
header:
teaser: /assets/images/fastball-teaser.png
Summary
Not all fastballs are created equal — and not all hitters handle elite fastball shapes equally well. This project was built to move beyond traditional velocity-and-spin summaries and quantify fastball movement in a way that connects directly to what hitters actually struggle with.
Using Statcast pitch-tracking data pulled via the pybaseball library, I engineered two composite movement metrics — True Ride and True Run — that better capture the "active" movement component of a fastball than raw spin or break measurements alone. I then linked elite pitch-shape percentiles to hitter performance outcomes and swing mechanics, using t-tests and distributional analysis to identify statistically significant differences.
This project was completed as part of RSM 360 — Analytical Methods in Sports at the University of Tennessee.

Data

Source: MLB Statcast via pybaseball
Season: 2025 (full season)
Volume: 770,428 pitch records
Hitter sample: Qualified MLB non-pitchers cross-referenced against Razzball projections data
Key Statcast fields used: pfx_x, pfx_z (raw break), release_spin_rate, vz0, vy0, vx0 (velocity components), attack_angle, attack_direction, swing_path_tilt, estimated_ba_using_speedangle, barrel


Composite Metrics
True Ride (Vertical)
True Ride measures how much a four-seam fastball fights gravity relative to what a pitch thrown at that velocity should drop. It combines Induced Vertical Break (IVB) with Vertical Approach Angle (VAA) — scaled to league context — to identify pitchers whose fastballs arrive at the plate with a genuinely flatter trajectory.
Top True Ride pitchers (2025): Jacob Misiorowski, Alexis Díaz, Jeremiah Estrada, Devin Williams, Jordan Romano
True Run (Horizontal)
True Run applies the same philosophy horizontally. Using the FanGraphs HAA formula, it quantifies Horizontal Approach Angle (HAA) relative to league norms to identify pitchers whose fastballs cut or run in ways that deviate most sharply from hitter expectations.
Top True Run RHP (2025): Matt Bowman, McCade Brown, Ryan Walker, Victor Mederos
Top True Run LHP (2025): Hoby Milner, Kyle Backhus, Nick Lodolo, Kody Funderburk

Key Findings
Hitter Performance vs. Elite Ride
Among hitters facing top-quintile True Ride fastballs (≥80th percentile), barrel rates and hits-per-pitch varied significantly. Top performers against ride fastballs included Pete Alonso, Brent Rooker, Nico Hoerner, and Christian Yelich — with hits-per-pitch rates above .290.
Swing Mechanics Split
Hitters were split into top-10 and bottom-10 groups by performance against elite ride fastballs. Their swing mechanics showed the following mean differences:
MetricTop 10Bottom 10Attack Angle-0.43°+5.73°Attack Direction13.06°5.70°Swing Path Tilt33.48°28.73°
Statistical Significance (t-tests)
All three swing mechanic differences between top and bottom performers were statistically significant:
Metrict-statisticp-valueAttack Angle-3.7420.0019Attack Direction2.7170.0165Swing Path Tilt2.5290.0215
Hitters who perform best against elite ride fastballs tend to have lower, more level attack angles and greater swing path tilt — mechanical signatures consistent with a swing designed to match the flatter trajectory of a true ride pitch.

Implications
These findings have direct applications for both scouting and player development:

Matchup evaluation: A pitcher's True Ride percentile can flag hitters who are mechanically vulnerable based on their attack angle profile
Pitch design: Pitchers seeking to develop a ride fastball can target IVB and VAA thresholds associated with elite True Ride scores
Hitting development: Hitters whose attack angles trend high may need mechanical adjustments to handle elite rising fastball shapes


Links
View Full Research Paper{: .btn .btn--primary}
View Jupyter Notebook on GitHub{: .btn .btn--inverse}

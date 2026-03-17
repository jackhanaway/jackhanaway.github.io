---
title: "2025 Great Plains East Pitcher Evaluation Metric and Scouting Grades"
date: 2025-11-01
tags:
  - northwoods league
  - pitcher evaluation
  - sabermetrics
  - scouting
  - R
excerpt: "A comprehensive pitcher evaluation metric for the Great Plains East Division of the Northwoods League, integrating DICE-weighted statistical performance with scouting-informed context and individual player grades."
---

## Introduction

This past summer, I worked with the Eau Claire Express of the Northwoods League as an assistant coach and analytics coordinator,
where I spent every game of the summer with the team, creating scouting reports of opposing teams daily. Considering the volatile 
nature of the Northwoods (players coming in and out, getting recruited, draft workouts, etc.), it is sometimes difficult to truly 
evaluate a players' season performance using a tangible metric like WAR. The purpose of this project was to design a comprehensive 
player evaluation metric for the Great Plains East Division of the Northwoods League, aimed at capturing a player’s overall value 
by integrating statistical performance with scouting-informed context.

This project was inspired by learning to calculate advanced baseball metrics such as WAR and DICE (Defense-Independent Component ERA)
during my sport analytics coursework at the University of Tennessee. Though it was not possible to directly apply WAR formulas to 
the data available from the Northwoods League, I sought to create a pitching metric that captures both actual performance and sample 
size reliability.

---

## Methodology Overview

### Data Collection and Standardization

Data comes from publicly available individual player stat CSVs for the 2025 Great Plains East teams: the Eau Claire Express,
Duluth Huskies, La Crosse Loggers, Thunder Bay Border Cats, Rochester Honkers, and Waterloo Bucks. Data was cleaned in Microsoft 
Excel and analyzed in R Markdown (`GPE_Pitching.Rmd`). Pitchers with fewer than 15 innings pitched were excluded to maintain 
reasonable sample size while keeping small-innings pitchers for context. Though a benchmark such as 20 innings may have been a
better choice for sample size purposes, I wanted to include pitchers from Eau Claire with 15-20 innings so that I could better 
contextualize the scores. The final metric integrates a pitcher’s underlying performance (strikeouts, walks, and home runs allowed)
with a Defense-Independent Component ERA (DICE) adjustment. Each variable was standardized to a z-score, then combined using the 
following weighted formula:

```r
weights_pitching_v2 <- c(
  ERA = -0.15,
  WHIP = -0.20,
  K.9 = 0.10,
  BB = -0.05,
  H = -0.05,
  FPS. = 0.05,
  IP = 0.10,
  BAA = -0.10,
  DICE = -0.40
)
```
Each component of the pitching score was chosen based on its predictive relevance to pitcher effectiveness and stability within 
the Northwoods League. Weights reflect both the strength of correlation to run prevention and theoretical importance in evaluating 
pitcher skill.

ERA (−0.15): ERA captures overall run prevention, but since it is heavily team and defense-dependent, it was given a moderate 
negative weight. It still provides useful context but should not dominate the model.

WHIP (−0.20): WHIP directly measures baserunner suppression (walks + hits per inning). It’s more consistent than ERA and highly 
indicative of command and contact management, warranting a stronger negative weight.

K/9 (+0.10): Strikeout rate is a key marker of dominance and “stuff.” Since this model could be useful in recruiting pitchers at
the college level, I figured K/9 should have a decent weight to quantify how each pitcher's stuff faired against Northwoods 
competition. A moderate positive weight rewards pitchers who demonstrate the ability to miss bats consistently.

BB (−0.05): Walks indicate command issues but are already indirectly reflected in WHIP and DICE. A small negative weight prevents 
redundancy while still penalizing poor control.

H (−0.05): Hits allowed offer limited independent insight once WHIP and BAA are included. This variable was lightly weighted to 
maintain a small penalty for consistently hard contact.

FPS% (+0.05): First-pitch strike percentage measures early count leverage and pitch efficiency. A small positive weight encourages
pitchers who get ahead and control plate appearances. This metric was particularly relevant for our team on the coaching side, and 
since this metric seeks to evaluate performance inside of the Northwoods League, I figured it deserved inclusion.

IP (+0.10): Innings pitched reflects durability and sample size reliability. A moderate positive weight rewards pitchers who sustained
performance over longer outings and larger samples.

BAA (−0.10): Batting average against isolates opponent contact quality. A midrange negative weight penalizes pitchers allowing 
frequent base hits despite decent command metrics.

DICE (−0.40): The most heavily weighted component, DICE (Defense-Independent Component ERA) isolates skill from defensive and random
variance. Its strong negative weight ensures the model prioritizes sustainable, defense-independent performance. DICE focuses on 
outcomes a pitcher can control—strikeouts, walks, hit-by-pitches, and home runs—removing the noise created by team fielding or luck.
By emphasizing DICE, the metric rewards pitchers who consistently prevent runs regardless of team context or small sample fluctuations. DICE and its cousin FIP are among my favorite metrics for evaluating pitchers who can consistently "get it done", and control what they can control.

To improve interpretability, I decided to scale the score in a similar manner that stats like WRc+ and ERA+ are weighted in MLB 
context, so that 100 represents the league average, with each 15-point increment corresponding roughly to one standard deviation 
above or below average. Since this study only covers data from one of the four Northwoods League divisions, "league average" 
signifies average production from this division. This makes the metric intuitive and easy to interpret, similar to widely used 
statistics like OPS+ or ERA+, allowing quick comparison of pitchers’ performance relative to their peers.

**Interactive Plot of Pitching Score**

[GPE-Pitching-Interactive.html](https://jackhanaway.github.io/NWL-PEM/GPE-Pitching-Interactive.html)

**Pitching Score vs. Innings Pitched**

To visualize the relationship between pitcher workload and performance, I plotted the pitching score against total innings pitched.
Each point represents an individual pitcher in the Great Plains East Division, allowing comparison of both durability and overall 
effectiveness.

A linear regression was fitted to the data to examine any potential trend between innings pitched and pitching score. The resulting
R² of 0.06 reflects that a pitcher's innings pitched account for about 6-7% of the variation in Great Plains East pitching scores, 
though innings pitched serves as a good benchmark to contrast perfomance and durability, while visualizing outlier performers. 
The p-value of 0 confirms the statistical significance of the regression model in the R environment, though in practical terms it 
reflects the structure of the fitted model rather than a meaningful predictive relationship.

This plot highlights that high or low GPE Pitching Scores are not simply a function of innings pitched; rather, the scores reflect
underlying performance metrics and DICE adjustments. Pitchers with fewer innings can still achieve high scores if their underlying
metrics are strong, reinforcing the metric’s ability to capture quality of performance independent of sample size.

---

****Scouting Grade Overview****

In addition to the quantitative pitching score (GPE Pitch Score), I wanted to provide quantitative scouting grades for certain 
outlier performers. These grades provide context that the data alone cannot fully reflect, including pitch repertoire, delivery,
command, in-game decision-making, mound presence, and more factors. 

For this phase of the project, I will generate scouting reports for the top five pitchers based on their GPE Pitching Scores to 
contextualize their outlier scores. I will also be highlighting their strengths, areas for improvement, and overall potential. 
Additionally, a few honorable mentions will be included to recognize pitchers who excelled in specific aspects despite slightly 
lower overall scores. These scouting evaluations complement the metric-driven analysis, offering a comprehensive picture of player
performance in the Great Plains East Division.

### #1 – Dawson Hargrove, RHP | GPE Pitching Score: 455.2 | Team: Eau Claire Express | Current School: Southern Illinois (via Arkansas State) | Age: 23 | Height/Weight: 6'3/195 lbs 

In a relatively small sample size, Dawson Hargrove established himself as the most reliable pitcher for the Eau Claire this summer,
posting a 1.07 ERA over 25.1 innings in 7 appearances, including 3 starts. Hargroved averaged over a strikeout per inning in those 
25.1 innings pitched. A theme common to each of the top 5 scorers in the division, Hargrove has multiple unique aspects to his game
that helped him cement himself as an outlier performer. 

## Arsenal

Hargrove boasts a complete north to south arsenal with a healthy pitch mix from a high arm slot, including a 4 seam, splitter, 
slider, and 12-6 curveball. Hargrove's 4 seam fastball sits from 88-90 MPH, topping out at around 92. He excels at commanding the 
fastball up in the strike zone, and tunneling his offspeeds off of it. Hargrove's 4 seam routinely registers outlier induced 
vertical break, consistently topping 20 inches of IVB, and releases it from a near 7 foot release height on average. This shape 
complements perfectly with his offspeeds, the most effective of which being his ~80 MPH splitter. Hargrove executed his splitter 
for consistent strikes and low spin rate (1000-1300), combined with a fading, tumbling shape. Hargrove is also comfortable with a 
sweeping slider from 81-84 MPH, and a 12-6 curveball with heavy vertical drop.

## Intangibles

Having had the opportunity to work with Hargrove in Eau Claire, I was able to gauge two key proponents to his success: he knows how 
to use his arsenal, and he pitches with intensity. Hargrove's pitch arsenal and shapes resemble a Northwoods League equivalent of 
an MLB north-to-south arsenal, and his eye-popping GPE Pitching Score is a testament to his ability to execute it. Additionally, 
Hargrove had an impressive 66% first-pitch strike rate, good for 6th among qualified pitchers in the dataset, meaning he was able 
to get ahead early in the count to generatw whiffs. While some negative body language may occasionally seep through from a bad call,
Hargrove is on the mound looking to dominate his opponent, and has a glaring mound presence. Hargrove's lethal combination of a 
strong mental game and a refined north-south arsenal led to him dominating the Great Plains East division with the top GPE Pitching
Score. 

## MLB Comparison: Trey Yesavage

Hargrove's arsenal of high-IVB fastballs, complemented by a healthy mix of low or negative IVB offspeed offerings bears striking
similarities to Toronto's Trey Yesavage. The two share the same tall and lanky stature, though Hargrove will still need to grow into 
his body to reach his maximum velocity potential.   

### #2 – Zack Serup, LHP | GPE Pitching Score: 412.1 | Team: Rochester Honkers | School: Western Kentucky (via Ventura JC) | Age: 21| Height/Weight: 6'3/195 lbs

Zack Serup was undoubtedly the best pitching option for the Rochester Honkers this summer. In his sample size of 27.1 innings, Serup
posted a 0.66 ERA and 0.80 WHIP in 5 appearances, all starts. Serup offers a north to south arsenal, leaning heavily on his mid to 
upper 80s fastball, high-IVB fastball and gyro slider. Serup's outlier quality was his excellent command, issuing only 5 walks, the 
fewest of any pitcher above 25 IP. 

## Arsenal

Serup uses 4 pitches: 4-seam, slider, changeup, and a slow 12-6 cuveball. His primary offering is his 4-seam fastball ranging from 
84-87, topping 88 MPH, with excellent induced vertical break averaging 19-20 inches, occasionally reaching the 24-25 range. Serup's
slider has a unique shape, a slow sweeper with varying horizontal movement, and consistent IVB around 0-3 inches. Would occasionally
flash a slow get-me-over curve around 67-68 MPH, and is still developing feel for the changeup. 

## Intangibles

I faced Serup twice while coaching with the Express, and both times he was able to mow our lineup down leaning heavily on his 
fastball. Getting feedback from our hitters, they would insist that the fastball shape is flat, though I would try explain the pitch's 
ridewas leading to us swinging under the pitch, leading to popouts and strikeouts. Serup's offspeeds served as a way to set up his 
fastball,as the ride gave him deceptive perceived velocity. As far as his demeanor, Serup looked relaxed and at-ease while on the 
mound, letting the shape of his fastball play into our swings resulting in quick outs. 

## MLB Comparison: Alex Vesia

Though the two pitchers have contrasting mound presences, the similarities between these two arsenals are palpable. Serup and Vesia
both leverage their high IVB fastballs at a heavy usage rate, and find ways to get outs without overpowering reads from the radar
gun. These two pitchers also excel at using offspeeds to set up and enhance the shapes of their fastballs. Outlier ride fastballs 
proved effective in the Northwoods in 2025, but Serup will need to develop fastball velocity and feel for a pitch with armside run 
away from a righty to be able to pitch at the next level.

### #3 – Cohen Gomez, RHP | GPE Pitching Score: 372.1 | Team: Eau Claire Express | School: Stanford | Age: 20 | Height/Weight: 6'3/212 lbs

Cohen Gomez posted the highest GPE Pitching Score out of any pitcher in the dataset with more than 30 innings pitched. The big right
hander was a staple for Eau Claire's rotation this summer, pitching to a 3.70 ERA over 48.2 innings. Gomez had the 4th lowest DICE 
in the division, which was uncharacteristic of pitchers with his sample size. The graph below contrasts each pitcher's innings 
pitched on the x-axis, with their Defensive Independent Component ERA on the y-axis. What makes his result especially impressive is 
the larger workload (48.2 IP) compared to most other top performers, who generally logged between 15 and 25 innings. His outlier 
quality was his consistency over a significantly greater sample size than most, implying his efficiency was sustainable rather than 
the product of small-sample variance. 

[🔍 View Interactive DICE Plot](https://jackhanaway.github.io/NWL-PEM/GPE-DICE-Interactive.html)

## Arsenal

Gomez has a unique delivery, coming set in the windup by internally rotating his front hip, and rocking into a load in his back leg
before a high leg kick and over-the-top finish. At first glance, it almost looks like a balk, but this shuffle into his delivery
messes with hitter's timing and helps Gomez's load and repeatability in his delivery. Gomez primarily uses 5 pitches: 4 seam 
fastball, 2-seam fastball, slider, split-change, and 12-6 curveball, and as the summer progressed, Gomez learned how to sequence
his arsenal. Gomez's fastballs will sit from 87-90 MPH, reaching 91-92 with Eau Claire, and while there is more velocity in the tank
to be developed, he is able to sustain his velocity deep into games. His 4 seam generates good ride from a high release point, giving
him deceptive perceived velocity. His 2-seam shape is still developing, though it worked to show hitters a different fastball shape 
depending on hitter handedness, as well as tendencies against the 4-seam. Gomez's best offspeed is "death ball" slider, at around
80-83 MPH. His slider serves as more of a sharp, up to down gyro ball as opposed to a traditional sweeper, and Gomez was able to play
this pitch exceptionally off of his ride fastball. Gomez's split-change was also a formidable offering, as he could execute the 
changeup off of his fastball to both righties and lefties, as well as kill velocity and spin. In a league featuring a high 
concentration of developing arms, the ability of a pitcher to locate a changeup to a batter of his same handedness associated with 
good individual results from those pitchers. Gomez would also flash a 12-6 curveball at around 77 MPH, with good negative induced 
vertical break. Though it mostly served as a get-me-over this summer, it is Gomez's only pitch consistently above 2000 RPMs. If Gomez
is able to leverage his frame better to generate more velocity and spin rate, the curveball has potential to turn into a plus 
offering.

## Intangibles

Gomez's mound presence is the most polarizing and essential part to his pitching. His large frame, unique and repeatable windup, and
interesting arsenal made him a very tough at bat for Northwoods competition. Gomez starts his outings like a bull in a rodeo, with 
palpable intensity visible from his body language. When Gomez is pitching, his intensity brings out the best in his defense; his 
defenders want to make plays for him. Gomez is very vocal on the mound, though it is only ever directed positively towards his own 
team, and never at the opposition. Though scouts might reason that Gomez may not throw as hard as his frame suggests he should, his
pitch execution skills are elite, using his fastball to set up offspeeds, and vice versa. Coming from Stanford, Gomez is a smart 
pitcher with plenty of room to develop into his frame. If Eau Claire had made the Northwoods League championship game, I would vouch
to have Gomez starting that game.

## MLB Comparisons: Lance Lynn, Trevor Megill

Gomez is an interesting prospect, in the sense that he has proven collegiate level pitchability as an underclassman, despite having
clear potential to improve his velocity. His execution ability and mound presence draw comparisons to Lance Lynn, an old-fashioned 
pitcher who's pitchability enhanced his arsenal. I included Brewers closer Trevor Megill as a testament to Gomez's potential. Gomez 
is a big, strong arm with good work ethic; with proper development it would not suprise me to see Gomez reach the mid to upper 90s 
by his draft year. He also reflects the intensity of these two arms, in the sense that he serves as a field general on the baseball
field. Gomez was easily one of the arms most projectable to professional baseball that I saw in the Northwoods.

### #4 – Chris Petersen, RHP | GPE Pitching Score: 332.3 | Team: Waterloo Bucks | School: St. Thomas | Age: 19 | Height/Weight: 6'4/215 lbs

Chris Petersen was the highest scoring pitcher on the Waterloo Bucks, though my analysis on him is limited, as he only pitched 
during the first half of the season, and Eau Claire never got the chance to face him. Petersen started 5 games for the Bucks in 2025,
to the tune of a 2.12 ERA in just short of 30 innings, with an impressive 1.02 WHIP. 

## Arsenal

Petersen's primary offerings include a four-seam fastball, 12/6 curveball, splitter, and a developing slider. While sitting in the 
high 80s throughout the Northwoods season, Petersen's fastball proved effective with good ride and occasional cut. This release 
makes Petersen a potential candidate for a seam-shifted sinker to add an offering with more armside run. Petersen's curveball and 
splitter are both very strong offerings, as he has great command of his big-movement curveball with strong sweep and depth. 
The splitter is also a depthy pitch of which he has great feel, his pitching coach says it has the potential to turn into a 
plus-pitch. The slider is a developing pitch, as it has a gyro movement profile with velocity around 80, similar to his fastball. 
Petersen likely needs to add velocity to this pitch to create more of a cutter-slider hybrid to create a greater difference in 
movement profile to his curveball.

## Intangibles

Despite being only 19, Petersen has a consistent, easy delivery with polished command. Petersen has a starter's build and profile, 
and is able to hold his velocity throughout his starts. Good feel for his north/south arsenal, knows how to pitch, and is a mature
player for his age, a type of player that St. Thomas produces well. Projects to be a weekend starter in his redshirt freshman 
campaign at St. Thomas, and has a high floor and ceiling to his ability. 

### MLB Comparison: Zac Gallen

Petersen draws some comparisons to Diamondbacks ace Zac Gallen for a couple of reasons. Similar to Gallen, Petersen's best putaway 
pitch is his sharp, big-shaped curveball with sweep, and complement it with a cut-ride fastball and consistent fading pitch 
(splitter for Peterson, changeup for Gallen). Though neither of these pitchers will blow their opposition away with their fastball
velocity, but its shape pairs very well with the rest of their arsenal, allowing it to be an effective tunnel. I believe both 
pitchers could also benefit from tweaks to their slider shape to increase the effectiveness of the pitch relative to their arsenal.
Though I was unable to see Peterson pitch in person, through studying his film I believe Peterson will project into a high-level 
college starter with potential to pitch professionally. Very polished arm for 19 years old.

### #5 – Nate Vidlak, RHP | GPE Pitching Score: 324.3 | Team: Duluth Huskies | School: Stephen F. Austin (via College of Idaho) | Age: 22 | Height/Weight: 6'1/205 lbs

Nate Vidlak was the highest scoring pitcher to play for the Duluth Huskies this summer, and is one of the most unique pitchers I've
ever shared a field with. In 5 appearances (4 starts), Vidlak posted a miniscule 0.90 ERA over 20 innings pitched, with three of 
these appearances coming against Eau Claire. Vidlak's unique arsenal and strong game experience are a major part of the reason why
many of my hitters on Eau Claire believe Vidlak was the best pitcher in the Northwoods League this year. 

## Arsenal

The most evident outlier quality Vidlak possesses is his incredibly unique arsenal. His arsenal consists primarily of a cutting 
four-seam, sharp gyro slider, depthy splitter, and a 12-6 curveball with big shape. Vidlak's fastball is a very unusual look for 
hitters, as it has consistent gloveside cut and light sink, almost like a cutter/slider hybrid. When considering pitchers in the 
Northwoods with this type of fastball shape, Vidlak has outlier velocity, sustaining a consistent 90-92, topping out 93 with the 
unique offering. Bizarrely, Vidlak doesn't fully get behind the baseball, leading to this cutting shape, but this also implies that
with mechanical tweaks, Vidlak could potentially develop a high-velo true four seam fastball with a distinct cutter. It is worth 
noting that Vidlak is also the only pitcher in the top 5 of GPE Pitch Scores without a "ride" fastball. Vidlak's natural wrist 
supination on release also makes him a great candidate for a seam shift sinker, to add a pitch with legitimate armside run.
Vidlak's slider plays especially well of his unique fastball, mirroring shape and spin until the last second. The shape resembles 
a sharp gyro slider with additional sweep, and he throws it at 81-83 MPH. His splitter is another lethal offering off of his 
fastball, as he is able to kill spin well and create a late, tumbling movement effect. Though Vidlak does not use his slower 
12-6 curveball especially often, he uses it effectively. It served as an excellent show pitch on 0-0, as he could consistently 
land the pitch in the zone while hitters were focused on his other offerings. Vidlak has excellent command, only walking 7 batters
in 20 innings, and is able to throw any pitch in any count.

## Intangibles

Vidlak oozes experience on the mound. Before committing to Stephen A. Austin after his successful Northwoods League campaign, 
Vidlak pitched for 3 years at NAIA College of Idaho. His experience showed on the mound this summer, as Vidlak showed great feel 
for executing his arsenal, mixing up timing and looks, and using his funk to his advantage. Knowing the fact that any pitch could 
be coming at any point in the at-bat, it was very difficult for us to sustain consistent offense off of Vidlak. Combining strong 
mound presence, extensive mound time, and his unique arsenal, Vidlak was an outlier among pitchers in the division, and across the
Northwoods League this year.

## MLB Comparison: Corbin Burnes, Hurston Waldrep

Though Vidlak is very much his own pitcher, I found a few similarities between Vidlak's pitching style and these two MLB arms. 
My mind first goes to Vidlak's elite cut-fastball, which drew comparisons in shape to reliever David Robertson. While their cutter
shapes are similar, Vidlak's pitchability projects him as a cutter-heavy starter profile, and his execution of his secondaries off
of his cutter reminded me of Corbin Burnes. While both like to lean on the cutter over starts, both possess exceptional offspeeds,
each of which could be their best pitch any given game. Vidlak's execution of his splitter reminded me of watching Hurston Waldrep,
dating back to his days at Florida. Both of these are funky arms with devastating splitters when executed properly, and have an 
innate ability to mix up their offerings to bring out the best in each pitch. While Vidlak is an older college pitcher, I believe 
if he posts promising numbers in his jump to Division 1 baseball, his potential could carry him to a professional opportunity.

### Honorable Mentions

## HM1 – Kenny Fistler, RHP | GPE Pitching Score: 197.6 | Team: Eau Claire Express | School: Alma College (MI) | Age: 22 | Height/Weight: 5'11/170 lbs

Fistler's GPE Pitching Score of 197.6 was the highest out of any reliever among qualified pitchers in the division. Though his ERA 
suffered as a product of pitching with lingering injuries towards the end of the year, Fistler showed the ability to get high level
Division 1 hitters out time and time again over 45 innings pitched this summer. 

## Arsenal
The D3 sidearmer throws a uniquely wide array of pitches from his arm slot, including a four seam fastball (which played like a 
sinker/two seam), cutter, sweeper, and changeup. Fistler's fastball ranged from 78 to 87 MPH over the course of the season, and its
effectiveness was the same at any velocity. He anchored Eau Claire's bullpen over the entire season, excelling at getting timely 
ground balls and quick innings, even pitching up to 6 innings in relief at a time. Fistler's main offspeed fluctuated between his
sweeper, a traditional, slower (68-70 MPH) sidearm slider with heavy gloveside sweep, and his cutter, a more unique, gyro offering
with sharper gloveside cut and more velocity (74-78). His changeup served mainly as a putaway pitch in plus counts to lefties.

## Intangibles
Aside from being the highest scoring reliever in the dataset, I included Fistler as an honorable mention for multiple reasons. First,
his scoring helped me determine how to tweak the formula for my metric into a tool that effectively gauges both workload and 
performance. My early models did not favor Fistler or Walker Retz, both workhorse arms for Eau Claire, which led me to altering 
the model to reward greater sample sizes and rely on more percentage based stats, so arms that threw more innings don't get punished
for inevitably surrendering more runs. This rationale also emphasizes the status of outlier performers in each sample size. Innings
pitched, our sample size parameter, only accounts for ~6% of the variation in our regression plot, meaning that rating is minimally
dependent on innings pitched, though a pitcher is not punished for throwing more. This way, performers with extreme z-scores can 
truly be evaluated on performance relative to their sample size. Fistler threw the 5th most innings in the division, most by a 
reliever, and outperformed his expected rating. Secondly, Fistler served as a matchup advantage against Duluth Huskies 3B Ethan
Surowiec, winner of the Northwoods League Most Valuable Player and division leader in GPE Hitting Score, a separate analysis. 
Fistler's sinker was the perfect pitch to combat Surowiec's uppercut swing path, and the sidearmer was able to retire him numerous 
times in leverage innings while avoiding damage. 

## MLB Comparison: Darren O'Day
Fistler's stuff in all likelihood won't land him professional attention, but like the Orioles sidearmer did for many years, he can
get high-level hitters out at a consistent rate. He has already recorded two seasons of sub-3.00 ERA pitching in the MIAA conference,
and combined with his pitchability, command, and poise from a low slot, I have no doubt that he would remain effective at the D1 level.

## HM2 – Nick Terhaar, RHP | GPE Pitching Score: 257.9 | Team: Duluth Huskies | School: Iowa | Age: 18 | Height/Weight: 6'3/225 lbs

If there was one player from the Great Plains East this year that I could place a bet on to play Major League Baseball one day, 
it would be Duluth's Nick Terhaar. The Minnesota High School Pitcher of the Year in 2025 made his Northwoods League debut less than
a week after his final high school game, and his time in the Northwoods made it clear that he was an elite incoming freshman arm.
Despite being the youngest player in the entire division, Terhaar pitched to a 2.84 ERA across 31.2 innings of worked, getting 
better every start he made.

## Arsenal
Despite being fresh out of high school at the time, Terhaar boasted overpowering stuff against college level competition. He has a
simple, repeatable windup leading to a compact arm action, though still has plenty of room to refine. His fastball is elite, 
spanning from 91-93 MPH, reaching 95 on multiple occasions, and consistently breaking or nearing 20" of IVB. Side to side movement
of the fastball was variable, would occasionally cut the ball on the gloveside portion of the plate, and have it run more on the 
armside half. Terhaar used a slurve shaped breaking pitch and a changeup for his offspeeds. His breaking pitch sits from 78-80 MPH,
throws with a spiked grip, and while movement metrics vary, the pitch has consistently good vertical drop and gloveside sweep. It is
possible that he has a distinct slider and 12-6 curveball, though his Trackman data was not clear enough to gauge. Terhaar would 
lower his arm slot considerably, and throw the pitch with a noteable spike in his grip. Despite our hitters learning about the tip,
the offspeed still proved effective. Another interesting part to Terhaar's game was his advanced feel for an unusual changeup. 
Mainly a whiff pitch, Terhaar essentially kills 10 MPH off his fastball, and flips the orientation of his fastball's IVB and HB, 
to create a changeup he can throw to both sides of the plate, nearing 20" of horizontal break. With further polishing of his arsenal
at Iowa, he has potential to contribute leverage innings as a true freshman.

## Intangibles
When scouting against Terhaar for his first Northwoods League start, I didn't have any Synergy or Trackman data available for my 
scouting report, and admittedly became worried when the first pitch was a 93 MPH fastball with 20 inches of IVB, coming from a high
schooler. Though in this first start, it was clear that Terhaar was amped up for his first collegiate outing, and sped up his pace,
ultimately leading to him losing command, and eventually getting pulled after his first inning. However, he showed great improvement
in this aspect over the summer, becoming more calm and polished in every aspect of his game every start he made. This improvement 
culminated in multiple 10+ K outings to end his regular season. Still 18 years old, Terhaar is a raw prospect, but judging from how
his raw stuff played against collegiate level hitting this summer, combined with Iowa's strong track record for developing pitching
status, I believe Terhaar has the highest ceiling out of any pitcher in the division this past year.

## MLB Comparison: Hunter Brown
Though Terhaar still has time to develop before his draft year, his current arsenal shape and delivery point me to compare him to 
the Astros budding ace. Though Brown's arm angle is a few degrees higher than Terhaar's, both of these young arms have power 
arsenals, leaning on high-ride fastballs that play well up in the zone. Both these arms also have similar curveball shapes, and 
unique changeup profiles, generating lots of armside run from higher slots. Though Terhaar will need to find his exact arm slot 
and look into developing a sinker/ distinction of his offspeeds, the potential on this prospect is undeniable, much like his MLB 
counterpart. Barring any setbacks in his development, I believe Terhaar projects as a future MLB prospect.









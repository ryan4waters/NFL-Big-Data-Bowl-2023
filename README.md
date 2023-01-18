# NFL-Big-Data-Bowl-2023

Linemen on Pass Plays

## Introduction

An offense breaks the huddle on a critical third down. The defense comes out in a double-mugged pressure look with two off-ball linebackers aligned in the A-gaps. The offense changes the protections call based on the defensive alignment, trying to predict and account for potential pass rushers. Earlier in the game, when the defense showed this look, they rushed six defenders; the offense successfully countered with a full slide protection, putting the running back on the EDGE away from the slide. Recalling what worked earlier, the quarterback once again calls for the full slide protection as he anticipates a six-man pressure.

But this time, when the ball is snapped, both mugged linebackers drop into coverage - leaving five offensive linemen for three defensive linemen, with the EDGE in a favorable one-on-one matchup against the running back. The extra defenders in coverage delay the quarterback’s decision making, allowing the EDGE to beat the running back for a sack and forcing a punt.

Our project attempts to predict a defender’s probability of being a pass rusher based on their pre-snap positioning and movements. Pass Rush Probability (xPassRush) and its derivatives can be useful tools for coaches and pro scouts to help identify and prepare for opposing defensive tendencies.

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/xPassRushHeat.png)

The model correctly predicts a defender will pass rush 91% of the time while incorrectly predicting a coverage player will pass rush 6% of the time. The features in the model are the player’s distance from the line of scrimmage, horizontal distance from the ball, direction, and “crept distance”, i.e. the distance a defender walks up or “creeps” towards the line of scrimmage (where a positive distance means moving towards the LOS, and negative means moving away). Additionally, each player is assigned a “primary position” label, which is a grouping (EDGE, IDL, Off-Ball LB, Safety, CB) based on their most frequent alignments (as determined by PFF charting).

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/primary_pos_summary.png)

## Using Pre-Snap Indicators to Identify a Pass Rusher

The following graphic of a 3rd & 7 play (Q4 - 8:17) from the Dolphins-Patriots Week 1 matchup shows how the model predicts key defenders’ probability of rushing the passer.

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/bdb%2022%20graphic.png)

Prior to the snap, we know it is extremely likely 0-technique Christian Barmore (#90) is going to pass rush given his pre-snap positioning. But just using the naked eye, there is more uncertainty for the offense regarding Kyle Van Noy (#53), Adrian Phillips (#21), and D’onta Hightower (#54) aligned at LOLB, LILB, and ROLB respectively.

Despite both Van Noy and Hightower dropping into coverage, their perceived blitz threat combined with their wide alignments force the offensive tackles to kick slide hard off the ball to gain initial depth; once the tackles recognize that both drop into coverage, their focus shifts to Judon (#9) and Uche (#55).

With the RT now forced to recover and try to block Judon after Van Noy dropped into coverage, Judon is able to more easily gain inside leverage as he penetrates the B-gap. This allows Phillips to loop around the RT after he manipulates the RG with his original inside rush path, leading to pressure which ultimately forces an errant intercepted pass.

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/NEpressure.gif)

What pre-snap indicators could have given the Dolphins more information to be better prepared for this pressure?

Despite Van Noy’s alignment near the LOS, the model only assigned a 7.2% pass rush probability to him based on other pre-snap indicators, such as the horizontal distance from the ball and the crept distance. On the opposite end, Hightower’s pass rush prediction was essentially a coin flip (53.6%), which can also be attributed to his horizontal distance to the ball as seen in the figure above.

Phillips’s intentions were the most interesting of the defenders on the field to gauge. Despite being approximately 4 yards from the LOS his horizontal distance from the ball (1.8 yards) and crept distance (0.43 yards) showed he had a relatively high chance of rushing (71.6%).

Crept Distance has more signal as a pre-snap indicator for the pass rush probability of Off-Ball LBs, Safeties, and CBs compared to EDGEs or IDLs, which is intuitive as they are further from the LOS.

## Identifying The Most Unpredictable Schemes, Blitzers, & Bluffers: PROE & MSE

Coaches and pro scouts spend countless hours studying game tape to find tendencies. Pass Rush% Over Expectation (PROE; the difference in a player's actual pass rush and xPassRush) and Mean Squared Error (MSE) can show which players are deviating most from their expected role on a play.

The figure below shows the defenses with the most unpredictable pass rush schemes by MSE (mean squared error; measures how actual behavior deviates from model prediction, i.e. more unpredictable/versatile playcalling) and can aid an offensive coach in their preparation for teams with unpredictable pass rush schemes. Even prior to beginning their tape study, coaches can know that they’ll likely need to prepare to adjust their protections because of the irregularity of pressure.

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/teamdefense.png)

It’s no surprise a Todd Bowles’s coached unit led the NFL in unpredictability (MSE). The Buccaneers’ high MSE can be attributed to occasionally dropping their interior defensive linemen into coverage. Below is an example of Vita Vea (#50) and Ndamukong Suh (#93) dropping into coverage while CBs Carlton Davis (#24) and Ross Cockrell (#43) rush the passer. This scheme resulted in the highest MSE for a defense on a single play in the eight week sample (0.365).

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/TBhighestmse.gif)

MSE and PROE can also be used to analyze how defenses deploy specific position groups. The Dolphins’ Off-Ball LB unit, for example, had the highest MSE (0.15) among all defensive units in the sample, followed by the Patriots’ Off-Ball LBs (0.13) and Falcons’ EDGEs (0.13), meaning that these groups deviated the most from their expected behavior based on the model. The Dolphins’ Off-Ball LBs rushed the passer much more frequently than expected, with a PROE of +8.2%, while the Patriots LBs’ (PROE: Patriots: -0.1%) rush rate roughly matched up with their overall expectation. This high unpredictability in spite of a PROE near 0 likely means that the Patriots LBs often rushed the passer when expected not to and vice versa.

The figure below shows the players with the highest MSE among their respective primary position. In preparation for the 2021 Dolphins defense, an advance report could have noted that Andrew Van Ginkel (who primarily aligns as an EDGE defender) is likelier to drop into coverage than his pre-snap positioning indicates. Meanwhile his teammates, Off-Ball LB Sam Eguavoen and Safety Brandon Jones, have a tendency to blitz more often than expected.

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/mse_highs.png)

It’s important to note that tendency is relative to expectation. No one expects Vita Vea to drop into coverage routinely, as evidenced by his 96% actual pass rush percentage, but among IDL he is the most unpredictable and it’s something to consider when preparing for the Buccaneers.

## Coaches Application - Sample Advance Reports

Below are two examples of advance reports using xPassRush, PROE, and MSE to find tendencies for a team and a specific player.

In the team report, we analyze Dean Pees' Atlanta Falcons defense and his use of Simulated Pressures ([a pressure that brings a 2nd or 3rd level defender, a LB or DB, in exchange for dropping a 1st level defender on the DL](https://coachhoover.blogspot.com/2019/08/creepers-vs-simulated-pressures.html)). We focused on the Falcons because they had one of the higher MSE figures and as we explored deeper, their Sim Pressure usage (15.5%, 1st) stood out as to why they had such an unpredictable pass rush scheme. For the purpose of this analysis we included Creepers in our Simulated Pressure definition to increase the sample of plays.

## Dean Pees' & The Atlanta Falcons' Simulated Pressure Usage

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/ATL%20Sim%20Pressure%20Report.png)
The report includes the Falcons' tendency by number of pass rushers, their Simulated Pressure usage by down and yards to go, the core second and third level defenders blitzing to replace the dropping first level players, a heat map displaying where pass rushers and coverage players align pre-snap on Sim Pressures compared to other pass rush schemes, and a tendency summary distilling the most important information in bullet points.

In the player report, we analyze Packers’ Off-Ball LB De’Vondre Campbell in Mugged alignments (defined as aligning as an off-ball LB within 2 yards of the LOS). We found Campbell to be of interest as he had the most Mugged snaps (40) and one of the lowest PROE (-32%) when in this alignment.

## De'Vondre Campbell's Tendencies in Mugged Alignment

![image](https://github.com/ryan4waters/NFL-Big-Data-Bowl-2023/blob/xPassRush/BDB2023%20Viz/De'Vondre%20Campbell%20Mugged%20Alignment%20Report.png)

The report (please zoom in for further examination) includes Campbell’s tendencies (both in aggregate and split by role and pre-snap gap alignment), a heat map displaying where Campbell aligns relative to the LOS with his xPassRush indicated by color, and a tendency summary with the most important information in bullet points.

These reports are purely examples and the reports could be fully customized to what a coach deems most important moving forward. In addition to the report, we could send coaches a brief All-22 cutup featuring the situations analyzed to supplement the report.

## In a Coach's Words - by Cody Alexander

*Cody is a former Texas HS Football Coach & FBS Defensive GA.*

The NFL game is a passing one, with a premium on protecting the quarterback. Therefore, the offensive staff must understand how a defense will attack their protection on any given week. Inversely, defenses attempt to apply pressure and manipulation through alignment and pre-snap presentation. As straightforward as that may seem, the defense's advantage is through pressure. Offenses dictate alignments by formation, but defenses can counter that through force or coercion. When offenses sit down to study a defense, using PROE, MSE, and xPassRush will give the staff a clear picture of what they see on film.

One primary application for using these analytic points is to streamline the use of film to identify "tells" within a defense. For instance, using the Mugged Alignment chart will allow quarterbacks, the offensive line, and coaches to identify adequately when a defender is rushing and correlate that within film study and real-time during a game. These analytic points can also help coaches build better practice scenarios that simulate real-time game experiences.

Defensively the principal use for these analytic points is within self-scouting. As shown through the MSE bar graph, a team can see how predictable their perceived presentations and play calls are. For example, Tampa Bay's staff does an excellent job of changing up looks weekly to create perceived chaos on game day. Unpredictability can create hesitation, and in the NFL, that spells disaster. Defensive staffs can use this information to develop weekly studies of their blitz designs. Predictability when on defense is not a trait most coaches want.

Finally, using this data on game days can allow for real-time game adjustments, as both the offense and defense can track how their opponents react. For offenses, they can use the xPassRush to shift protections off any unique look presented that week. Defensively, coaches can identify what looks are creating or lacking in pressure and address playcalling accordingly.

## Conclusion & Further Exploration

We hope that xPassRush, MSE, and PROE can be valuable tools for coaches and pro scouts in their preparation for upcoming opponents and self-scouting.

With further development xPassRush could even become a player development tool that helps QB and OL take mental reps to pickup on the subtle nuances and tells for certain defender alignments. An app "game" could be created where a player has to predict which defender is going to rush the passer using the pre-snap information from the All-22. The player would input his prediction by checking off the players he expects to rush and summarize in a notes section what offensive protection call would then take place to block that group of pass rushers. Once that information is submitted the app would provide feedback to the player on whether his prediction was correct or not.

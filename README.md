This repository contains materials towards the completion of my introductory data science thesis with Springboard. 

This repository includes the R Markdown file for the final paper explaining this project, and a knitted html version of this paper is hosted at: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

This course was taught in the R programming language, and curriculum topics for this course included probability and descriptive statistics, data wrangling, data analysis, and an introduction to machine learning. 

#Data Story - Please <a href="https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/">see this hosted write-up</a> for the full story, but to summarize the problem, basketball is a popular global sport and coaches, fans, gamblers and more are all interested in the mathematical limits of when is too late for a comeback victory? Is it garbage time or not? Should the coach take out the star player or not? Should a fan leave early to beat traffic or not? 

#Data: 

This project uses data simulated by a model, and then various strategies of model validation. The core of this simulation is the Gameflow Chart, where time is on the x-axis, and score difference is the y-axis. For the final product, roughly 2.5 million basketball games were simulated and evaluated.

#Product: 

A web-app was developed and deployed to the R Shiny.io database, which depicts contour levels for various "altitudes" of probability, for each cell on the map of score-time endgame possibilities, allowing lay-users to easily compute (up to 13 minutes before the game ends) a validated probability that the leading team will win. 

#Users: 

- Basketball coaches will enjoy using this app to take the emotion and intuition out of determining when to subsitute-out relatively valueable players, and sub-in relatively less valuable players (or not), near the end of the game, to mitigate the risk of needless injury to star players, and extend their viable careers. 

- Basketball fans will enjoy using this app to more clearly determine whether a game is still competitive and worth watching, or if it's worth it to move on with their lives and maybe beat traffic out of the arena, without fear of missing a dramatically exciting comeback. 

- Basketball executives who are hungry to add statistical-economic "Moneyball" - style methods of analysis to basketball will enjoy using this app to evaluate and consult their coaches via a more numerically robust approach. 

- Gamblers on basketball placing late bets on games will be keen to use this app to add quantitative certainty to their approaches in calculating their opinion of the scoring and outcome odds of a game in progress. 

The final write-up is hosted with shiny R components at the link: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

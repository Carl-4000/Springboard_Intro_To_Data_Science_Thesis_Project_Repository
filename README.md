<h2>1. Copyright statement comment:</h2> 

All rights reserved.

<h2>2. Author Comment:</h2>

This repository contains materials towards the completion of Carl Larson's introductory data science course capstone with Springboard, with mentor Ryszard Czermiński. 

<a href="https://medium.com/@premiumwordsmith/visualizing-basketball-end-games-c8fdd4d757e2">A blog write-up is also hosted at Medium (roughly a 15 minute read).</a> 

<a href="https://youtu.be/mi9f62sXySA">A video presentation is included,</a> with a walkthrough of the project powerpoint provided in this file. 

This course was taught in the R programming language, and curriculum topics for this course included probability and descriptive statistics, data wrangling, data analysis, and an introduction to machine learning. 

<h3>Data Story</h3> While watching a timed sports game like basketball, you may have wondered near the end of the game, is it still competitive or are we in 'Garbage Time?' In other words, is there any mathematical chance that the trailing team could still come back and win given the current score and time remaining? 

- The problem statement is a simple classification: Is it 'Garbage Time' or not? 

The answer has implications such as- Should the coach take out the star player or not? Should a fan leave early to beat traffic or not? How should a busy Vegas oddsmaker weight bets late in a game? 

<h3>Product:</h3>

An HTML5-based web application was developed and deployed to the R Shiny.io database, which depicts contour levels for various "altitudes" of probability, for each cell on the map of score-time endgame possibilities, allowing lay-users to easily compute a validated probability that the leading team will win. 

<h2>3. File description comment:</h2> 

This repository includes the R Markdown file for the final paper explaining this project, and a knitted html version of this paper is hosted at: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

The final write-up is hosted with shiny R components at the link: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

<h2>4. Source and library statements</h2>

- require(shiny)
- require(data.table)
- require(dplyr)
- require(ggplot2)

<h2>5. Function definitions</h2>

- GameflowMaker4
- algo2app
- GFmapper
- ContMapSlicer
- HomeWinQApp
- finalScoreAverager2
- GFTsma2
- algo3app
- gftopo4
  
<h2>6. Executed statements (of Hooply application):</h2>
  
- contour()
- print()

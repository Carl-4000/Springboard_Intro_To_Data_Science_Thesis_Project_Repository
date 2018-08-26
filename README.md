<h2>1. Copyright statement comment:</h2> 

All rights reserved.

<h2>2. Author Comment:</h2>

This repository contains materials towards the completion of Carl Larson's introductory data science course capstone with Springboard, with special thanks to mentor Ryszard Czermiński. 

- <a href="https://medium.com/@premiumwordsmith/visualizing-basketball-end-games-c8fdd4d757e2">A blog write-up is also hosted at Medium (roughly a 15 minute read).</a> 

-WHY?- <a href="https://youtu.be/mi9f62sXySA">This video is a demonstration and presentation of Hooply,</a> serving as an introduction to Hooply and *why* people use it. 

-HOW?- <a href="https://youtu.be/Ny_V33HNlFo">This video</a> shows a walkthrough and explanation of the R code that went into the data behind the Hooply application, as well as the source code in Shiny R for the app itself, showing *how* it works. 

This course was taught in the R programming language, and curriculum topics for this course included probability and descriptive statistics, data wrangling, data analysis, and an introduction to machine learning. 

<h3>Data Story</h3> While watching a timed sports game like basketball, you may have wondered near the end of the game, is it still competitive or are we in 'Garbage Time?' In other words, is there any mathematical chance that the trailing team could still come back and win given the current score and time remaining? 

- The problem statement is a simple classification: Is it 'Garbage Time' or not? 

The answer has implications such as- Should the coach take out the star player or not? Should a fan leave early to beat traffic or not? How should a busy Vegas oddsmaker weight bets late in a game? 

<h3>Product:</h3>

<a href="https://carlrlarson.shinyapps.io/hooply_app/">An HTML5-based web application was developed and deployed to the R Shiny.io database,</a> which depicts contour levels for various "altitudes" of probability, for each cell on the map of score-time endgame possibilities, allowing lay-users to easily compute a validated probability that the leading team will win. A backup copy of the app is also hosted <a href="https://carlrlarson.shinyapps.io/hooply">via this link</a> (in case the first link is over-run by the web traffic cap). 

<h2>3. File description comment:</h2> 

The project whitepaper is hosted in this Github repository in both .RMD and .PDF format, and with a Shiny-responsive HTML version hosted at the link: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/ Please note this particular link can take a few minutes or more to load, so please be patient. 

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

1. Copyright statement comment: All rights are reserved by author.

2. Author Comment: 

This repository contains materials towards the completion of my introductory data science thesis with Springboard, and my mentor Ryszard Czermiński. 

<a href="https://medium.com/@premiumwordsmith/visualizing-basketball-end-games-c8fdd4d757e2">A blog write-up is also hosted at Medium (roughly a 15 minute read).</a> 

<a href="https://youtu.be/mi9f62sXySA">A video presentation is included,</a> with a walkthrough of the project powerpoint provided in this file. 

This course was taught in the R programming language, and curriculum topics for this course included probability and descriptive statistics, data wrangling, data analysis, and an introduction to machine learning. 

#Data Story - While watching a timed sports game like basketball, you may have wondered near the end of the game, is it still competitive or are we in 'Garbage Time?' In other words, is there any mathematical chance that the trailing team could still come back and win given the current score and time remaining? 

The problem statement is a simple classification: Is it 'Garbage Time' or not? 

The answer has implications such as- Should the coach take out the star player or not? Should a fan leave early to beat traffic or not? How should a busy Vegas oddsmaker weight bets late in a game? 

#Product: 

A web-app was developed and deployed to the R Shiny.io database, which depicts contour levels for various "altitudes" of probability, for each cell on the map of score-time endgame possibilities, allowing lay-users to easily compute a validated probability that the leading team will win. 

3. File description comment: This repository includes the R Markdown file for the final paper explaining this project, and a knitted html version of this paper is hosted at: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

The final write-up is hosted with shiny R components at the link: https://carlrlarson.shinyapps.io/Basketball_End-Game_Analysis_Springboard/

4. Source and library statements

require(shiny)
require(data.table)
require(dplyr)
require(ggplot2)

5. Function definitions (how Hooply was created)

  a.) # A crude but effective basketball game simulator
  GameflowMaker4 <- function(
  probA = c(abs(rnorm(1, 0.65, 0.005)), abs(rnorm(1, 0.09, 0.002)), abs(rnorm(1, 0.17, 0.005)), abs(rnorm(1, 0.09, 0.005))), 
  probB = c(abs(rnorm(1, 0.64, 0.005)), abs(rnorm(1, 0.10, 0.003)), abs(rnorm(1, 0.17, 0.005)), abs(rnorm(1, 0.09, 0.005))), 
  n = as.integer(abs(rnorm(1, 218, 22))),   #ad hoc game-length index variable
  dt = 0.005,   #min time between scores to allow for physics
  GL = 48){   #game length of 48 minutes per NBA rules
  sta = sample(0:3, n, replace=TRUE, prob=probA)
  stb = sample(0:3, n, replace=TRUE, prob=probB)
  w = which(sta > 0 & stb > 0)
  sta[w] = 0
  stb[w] = 0
  v = which(sta == 0 & stb == 0)
  sta <- sta[-c(v)] 
  stb <- stb[-c(v)]
  m <- length(sta)
  listA <- as.vector(seq(from=0, to=((m * dt)-dt), by=dt))
  listB <- as.vector(sort(runif(m, 0, c(GL-listA[m]))))
  MinLeft <- listA + listB
  MinLeft <- rev(MinLeft)
  scoreA <- cumsum(sta)
  scoreB <- cumsum(stb)
  df = data.table(MinLeft, scoreA, scoreB)
  if(df$scoreA[nrow(df)] == df$scoreB[nrow(df)])
  df = df[-c(nrow(df))]
  return(df)
}

  b.) # "h" for "hoop" which is the index, p for points which is score diff.
algo2app <- function(h=30, p=scdiff){
  m=90
  mec <- matrix(data = 0, nrow = 1, ncol = 5)
  for(i in 1:m){
  df <- GameflowMaker4()
  q <- (nrow(df) - h)
  mec[1,4] <-  mec[1,4] + as.numeric(df$MinLeft[q])
  mec[1,5] <- mec[1,5] + as.numeric(nrow(df))
  if(((df$scoreA[q]-p) > df$scoreB[q]) && (df$scoreA[nrow(df)] > df$scoreB[nrow(df)])) {
  mec[1,1] <- mec[1,1] + 1
  next}  #team A led at q and eventually won
  
  else if(((df$scoreB[q]-p) > df$scoreA[q]) && (df$scoreB[nrow(df)] > df$scoreA[nrow(df)])){
  mec[1,1] <- mec[1,1] + 1
  next}  #team B led at q and eventually won
  
  else if(((df$scoreA[q]-p) > df$scoreB[q]) && (df$scoreB[nrow(df)] > (df$scoreA[nrow(df)]))){
  mec[1,2] <- mec[1,2] + 1 
  next}  #team A led at q but team B came back and won
  
  else if(((df$scoreB[q]-p) > df$scoreA[q]) && (df$scoreA[nrow(df)] > df$scoreB[nrow(df)])){
  mec[1,2] <- mec[1,2] + 1
  next}  #team B led at q but team A came back and won

    else{mec[1,3] <- mec[1,3] + 1 #functional "q-ties" irrelevant
      next}
if(i==n)
  break}
  mec2 <- matrix(data=0, nrow = 1, ncol = 4)
  mec2[1,2] <- (mec[1,1]/(mec[1,1]+mec[1,2]))
  mec2[1,1] <- h
  mec2[1,3] <- (mec[1,4] / m)
  mec2[1,4] <- (mec[1,5] / m)
  colnames(mec2) <- c("Game_Action_index", "Leader_win_chance:", "Avg_sample_MinsLeft", "avg_Game_Actions")
  return(mec2)
}

  c.) # This runs for the argument "k" game-action index down to 1 creating a dataframe

GFmapper <- function(k=35){
  matx <- matrix(data = 0, nrow = k, ncol = 4)
  h <- k
  for(i in 1:k){
    h <- h
    dfJ <- as.matrix(algo2app(h=h))
    matx[i,1] <- dfJ[1,1]
    matx[i,2] <- dfJ[1,2]
    matx[i,3] <- dfJ[1,3]
    matx[i,4] <- dfJ[1,4]
    h <- h - 1
    if(h == 0){
      break
      }else{
        i <- i+ 1
        next}}
  return(matx)
}

  d.) # Holding score constant at the global variable "scdiff" we can see how probability of a comeback decreases over time
ContMapSlicer <- function(k = 35){

df <- as.data.frame(GFmapper(k=k))
colnames(df) <- c("Game_Action_index", "Leader_win_chance:", "Avg_sample_MinsLeft", "avg_Game_Actions")
#plot(df[1:k,3], df[1:k,2], type="b", main="Leading Team Win Chance",
#     xlab = "Minutes Left", ylab="Percent")
ggplot(df, aes(x=df[1:k,3], y=df[1:k,2])) +
  geom_point() +
  geom_smooth(span = 0.2) +
  geom_hline(yintercept = ConfThresh, linetype="dotted", color="red") +
  geom_vline(xintercept = MinzLeft, color="green") +
  labs(y="Percent", x="Minutes Left") +
  ggtitle("Leading Team Win Chance")
}

  e.) # Designed to print out home win percentage running m-many game simulations

HomeWinQApp <- function(m=50){
  
  mec <- matrix(data = 0, nrow = 1, ncol = 2)
    for(i in 1:m){
    df <- GameflowMaker4()
      if((df$scoreA[nrow(df)]) > df$scoreB[nrow(df)]){
        mec[1,1] <- mec[1,1] + 1   #away team won
        next} 
      else if(df$scoreA[nrow(df)] < df$scoreB[nrow(df)]){
        mec[1,2] <- mec[1,2] + 1     #home team won
      next 
      } else if(i==m){break}}
    mec2 <- matrix(data=0, nrow = 1, ncol = 2)
    mec2[1,1] <- (mec[1,1]/m) #away team record
    mec2[1,2] <- (mec[1,2]/m) #home team record
    colnames(mec2) <- c("Away Team Record", "Home Team Record")
    return(mec2)
}

  f.) # Creating a base function to use in a loop
finalScoreAverager2 <- function(n){
  df<-GameflowMaker4()
  AvgScore <- c(df$scoreA[nrow(df)], df$scoreB[nrow(df)])
  
  return(AvgScore)
}

  g.) # Game flow table score mass averager; works for m-values up past 10,000

GFTsma2 <- function(m=50){
  totalA <- 0
  totalB <- 0
  for(i in 1:m){
  tuple <- finalScoreAverager2()
  totalA <- totalA + tuple[1]
  totalB <- totalB + tuple[2]
  }
  totalA <- (totalA / m)
  totalB <- (totalB / m)
  return(c(totalA, totalB))
  }
  
    h.) #"h" for hoop and "p" for point difference.
algo3app <- function(h=MinzLeft, p=scdiff, m=50){
  
  mec <- matrix(data = 0, nrow = 1, ncol = 4)
  for(i in 1:m){
  df <- GameflowMaker4()
  q <- (nrow(df) - round(h / 0.4847 + 0.4733))
  mec[1,4] <-  mec[1,4] + as.numeric(df$MinLeft[q])
  
  if(((df$scoreA[q]-p) > df$scoreB[q]) && (df$scoreA[nrow(df)] > df$scoreB[nrow(df)])) {
  mec[1,1] <- mec[1,1] + 1
  next} #team A led at q and eventually won
  
  else if(((df$scoreB[q]-p) > df$scoreA[q]) && (df$scoreB[nrow(df)] > df$scoreA[nrow(df)])){
  mec[1,1] <- mec[1,1] + 1
  next} #team B led at q and eventually won
  
  else if(((df$scoreA[q]-p) > df$scoreB[q]) && (df$scoreB[nrow(df)] > (df$scoreA[nrow(df)]))){
  mec[1,2] <- mec[1,2] + 1 
  next} #team A led at q but team B came back and won
  
  else if(((df$scoreB[q]-p) > df$scoreA[q]) && (df$scoreA[nrow(df)] > df$scoreB[nrow(df)])){
  mec[1,2] <- mec[1,2] + 1
  next} #team B led at q but team A came back and won

    else{mec[1,3] <- mec[1,3] + 1 #functional "q-ties" irrelevant
      next}
if(i==n)
  break}
  mec2 <- matrix(data=0, nrow = 1, ncol = 3)
  mec2[1,2] <- (mec[1,1]/(mec[1,1]+mec[1,2]))
  mec2[1,1] <- q
  mec2[1,3] <- (mec[1,4] / m)
  colnames(mec2) <- c("Game_Action_index", "Leader_win_chance:", "Avg_sample_MinsLeft")
  return(mec2)
}

  i.) gftopo4 <- function(m=50){
  MinzLeft = MinzLeft
  scdiff = scdiff
  x_framer <- round(1.67 * (MinzLeft / 0.4847 + 0.4733))
  y_max <- (scdiff *2)
  y_min <- round(scdiff / 4)
  i <- 1 #x index set before looping
  dfz <- matrix(data = 0.50, nrow = x_framer, ncol = (y_max - y_min)) #z matrix
  xax <- matrix(data = 0, nrow = x_framer, ncol = 1) #x axis spacing
  yax <- as.vector(seq(from = y_min, to = y_max)) #y axis
  for(i in 1:x_framer){
    xvr <- 0  #average calculator, x-average
    j <- 1 #y index set before looping
    for(j in 1:(y_max-y_min)){
      df2 <- matrix(data=0, nrow=1, ncol=3)
      df2 <- algo3app(h=i, p=j, m=m)
      xvr <- xvr + df2[1,3]
      dfz[i, j] <- df2[1,2]
      if(j < (y_max+1)){
        j <- j + 1
      } else {
          break
        }
    }
    xax[i] <- xvr / (y_max + 1 - y_min)
    if(i < x_framer){
      i <- i + 1
    } else {
      break}
  }
  contour(x = xax, y = seq(from = y_min+1, to = y_max, length.out = ncol(dfz)), z = dfz, levels = seq(from = 0.9, to = 1, by = 0.025), col = topo.colors(5),
  xlab = "Minutes Remaining", ylab = "Point Lead")
  title("Simulated Win % of Leading Team", font = 4)
  legend(x=2,y=16,legend=c("90%", "92.5%", "95%", "97.5%", "100%"), fill = topo.colors(5), col = topo.colors(5))
  
  write.csv(dfz, "dfz3.csv")
  write.csv(xax, "xax3.csv")
  write.csv(yax, "yax3.csv")
}

6. Executed statements (of the Hooply application):
  
  a.) contour()
  
   b.) print()

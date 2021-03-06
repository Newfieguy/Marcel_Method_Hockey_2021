---
title: "Creating Marcel Method Hockey"
author: "Alex Walker"
date: "14/08/2020"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

library(tidyverse) 
library(here) 
library(readxl) 
library(stringr) 
library(dplyr) 
library(ggplot2) 
library(knitr)
library(kableExtra)
library(rmarkdown)

Loading every library known to man. 

season_1718 <- read_csv(here("data","1718_season.csv"))
season_1819 <- read_csv(here("data","1819_season.csv"))
season_1920 <- read_csv(here("data","1920_season.csv"))

Load in our 3 season we're going to be using for the Marcel Method

names(season_1718)

Things to note: In order to complete Marcel projections I need 1) Season weights (I'm sticking with 5/4/3) 2) I'll need to find each players average in the relevant seasons 3) I'll need to find the league average for F/D in each category 4) Regress the player usiong the league average at their position 5) Determine their projected TOI 6) Apply age adjustment 7) adjust based on 2020 baseline


##Finding the league averages for the stats we're going to use is the first order of business: 


lg_avg_1920 <- season_1920 %>%
          mutate(Pos = replace(Pos, Pos != "D", "F")) %>%
          group_by(Pos) %>%
          summarize(
          AVG_gp = mean(GP, na.rm = TRUE),
        lg_avg_G = mean(G, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_A = mean(A, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PTS = mean(PTS, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPG = mean(PPG, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPA = mean(PPA, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_S = mean(S, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_BLK = mean(BLK, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_HIT = mean(HIT, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PIM = mean(PIM, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOW = mean(FOW, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOL = mean(FOL, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_TOI = mean(TOI, na.rm = TRUE)/mean(GP, na.rm = TRUE),
          n = n())

lg_avg_1819 <- season_1819 %>%
          mutate(Pos = replace(Pos, Pos != "D", "F")) %>%
          group_by(Pos) %>%
          summarize(
          AVG_gp = mean(GP, na.rm = TRUE),
        lg_avg_G = mean(G, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_A = mean(A, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PTS = mean(PTS, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPG = mean(PPG, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPA = mean(PPA, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_S = mean(S, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_BLK = mean(BLK, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_HIT = mean(HIT, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PIM = mean(PIM, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOW = mean(FOW, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOL = mean(FOL, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_TOI = mean(TOI, na.rm = TRUE)/mean(GP, na.rm = TRUE),
          n = n())


lg_avg_1718 <- season_1718 %>%
          mutate(Pos = replace(Pos, Pos != "D", "F")) %>%
          group_by(Pos) %>%
          summarize(
          AVG_gp = mean(GP, na.rm = TRUE),
        lg_avg_G = mean(G, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_A = mean(A, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PTS = mean(PTS, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPG = mean(PPG, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PPA = mean(PPA, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_S = mean(S, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_BLK = mean(BLK, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_HIT = mean(HIT, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_PIM = mean(PIM, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOW = mean(FOW, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_FOL = mean(FOL, na.rm = TRUE)/mean(TOI, na.rm = TRUE),
        lg_avg_TOI = mean(TOI, na.rm = TRUE)/mean(GP, na.rm = TRUE),
          n = n())





---
title: "A Plain Markdown Post"
date: '2016-12-30T21:49:57-07:00'
---

This is a post written in plain Markdown (`*.md`) instead of R Markdown (`*.Rmd`). The major differences are:

```{r setup, include=FALSE, echo=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r, include=FALSE, echo=FALSE}
library(tidyverse)
library(nflscrapR)
library(devtools)
library(dplyr)
library(nflfastR)
library(stargazer)
library(ggplot2)
library(shiny)
library(na.tools)
library(ggimage)
library(nflreadr)
library(ggrepel)
library(scales)
```

```{r, echo=FALSE, include=FALSE}
#2021 Data
pbp <- load_pbp(2021)
write.csv(pbp, "/Users/drew/Desktop/pbp.csv")
PBP_Rolling_Data <- read.csv("/Users/drew/Desktop/pbp.csv")
pbp2021 <- PBP_Rolling_Data %>% 
	filter(!is_na(epa), play_type=="no_play" | play_type=="pass" | play_type=="run")

pbp2021 <- pbp2021 %>%
	mutate(
	pass = if_else(str_detect(desc, "( pass)|(sacked)|(scramble)"), 1, 0),
	rush = if_else(str_detect(desc, "(left end)|(left tackle)|(left guard)|(up the middle)|(right guard)|(right tackle)|(right end)") & pass == 0, 1, 0),
	success = ifelse(epa>0, 1 , 0)
	) 
```

```{r, echo=FALSE, include=FALSE}
#2020 Data
pbp <- load_pbp(2020)
write.csv(pbp, "/Users/drew/Desktop/202020.csv")
pbp2020 <- read.csv("/Users/drew/Desktop/202020.csv")

depth <- load_rosters(2021)
write.csv(depth, "/Users/drew/Documents/depth.csv")
depth <- read.csv("/Users/drew/Documents/depth.csv")
schedules <- load_schedules(2021)
```

```{r, echo=FALSE, include=FALSE}
#Stats 2021
stats <- load_player_stats(seasons = 2021)
write.csv(stats, "/Users/drew/Desktop/stats.csv")
stats_2021 <- read.csv("/Users/drew/Desktop/stats.csv")
```

```{r, echo=FALSE, include=FALSE}
#pfr_passing_2021 <- load_pfr_passing(2021)
#write.csv(pfr_passing_2021, "/Users/drew/Desktop/pfr_2021.csv")
#pfr_2021 <- read.csv("/Users/drew/Desktop/pfr_2021.csv")

#Next Gen Stats
nxt_gen_2021_receiving <- load_nextgen_stats(2021, stat_type = "receiving")
write.csv(nxt_gen_2021_receiving, "/Users/drew/Desktop/nxt_gen_2021_receiving.csv")
WR_nxt <- read.csv("/Users/drew/Desktop/nxt_gen_2021_receiving.csv")

nxt_gen_2021 <- load_nextgen_stats(2021)
write.csv(nxt_gen_2021, "/Users/drew/Desktop/nxt_gen_2021.csv")
nxt_2021 <- read.csv("/Users/drew/Desktop/nxt_gen_2021.csv")
```




## 1. QB Data

```{r, message=FALSE, include=FALSE}
x_cpoe <- pbp2021 %>%
  filter(!is.na(epa)) %>%
  group_by(id, name) %>%
  summarize(
    epa = mean(epa),
    cpoe = mean(cpoe, na.rm = T),
    n_dropbacks = sum(pass),
    n_plays = n(),
    team = last(posteam)
  ) %>%
  filter(n_dropbacks > 349 & n_plays > 349)

x_cpoe <- x_cpoe %>%
  left_join(teams_colors_logos, by = c('team' = 'team_abbr'))

```


```r
blogdown::new_post("Post Title", ext = '.Rmd')
```

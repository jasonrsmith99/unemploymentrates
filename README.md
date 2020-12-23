Unemployment Rates by Race
================

``` r
library(tidyverse)
library(lubridate)
library(directlabels)
library(ggthemes)
```

``` r
fredgraph <- read_csv(here::here("data", "fredgraph.csv"))
fredgraph <- fredgraph %>%
  mutate(DATE = ymd(DATE)) %>% 
  rename("Aggregate" = UNRATE,
         "African American" = LNS14000006,
         "Hispanic" = LNS14000009,
         "White" = LNS14000003,
         "Asian" = LNU04032183) %>% 
  pivot_longer(cols = 2:6, names_to = "group", values_to = "val") %>% 
  filter(year(DATE) != 2020) %>% 
  mutate(group = fct_relevel(group, "Aggregate", "African American", "Asian", "Hispanic", "White"))
```

``` r
graph <- fredgraph %>% 
  ggplot(aes(x = DATE, y = val, color = group, linetype = group))+
  geom_line(size = 1.5)+
  scale_x_date(expand = expansion(add = c(0, 1250)))+
  scale_color_manual(values = c("#e41a1c", "#377eb8", "#4daf4a", "#984ea3", "#ff7f00"))+
  scale_linetype_manual(values = c("dotted", "solid", "solid", "solid", "solid"))+
  theme_economist()+
  labs(title = "Unemployment Rate 2000-2019",
       subtitle = "by race",
       x = "Year",
       y = NULL,
       caption = "Unemployment calculated quarterly")+
  scale_y_continuous(labels = scales::label_percent(scale = 1))
  
direct.label(graph, list(last.qp, cex = 1))
```

![](README_files/figure-gfm/graphing-1.png)<!-- -->

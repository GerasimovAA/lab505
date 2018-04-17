# lab505
---
title: "lab5"
author: "Gerasimov Artem"
date: "`r format(Sys.time(), '%d %B, %Y')`"
output: 
  html_document: 
    self_contained: no

---


## Задание
Интерактивная карта-хороплет стран мира (GVis), показатель – любой из раздела «Aid Effectiveness» (Эффективность госпомощи) базы Всемирного банка.
# Интерактивная картограмма  
Построена на *данных по сельское хозяйству и развитии сельских районов за 2014 год* базы Всемирного банка.  

```{r, echo=TRUE, message=F, warning=FALSE, cashe=T, results='asis'}
library('WDI')
library('googleVis')
library('data.table')

# пакет с API для WorldBank
# загрузка данных по всем странам, 2014 год, показатель
# Aid Effectiveness
dat <- WDI(indicator = 'BX.GRT.EXTA.CD.WD', start = 2005, end = 2015)
DT <- data.table(country = dat$country, value = 'BX.GRT.EXTA.CD.WD')
# объект: таблица исходных данных
g.tbl <- gvisTable(data = DT, 
                   options = list(width = 220, height = 400))
  # объект: интерактивная карта
g.chart <- gvisGeoChart(data = DT, 
                        locationvar = 'country', 
                        colorvar = 'value', 
                        options = list(width = 600, 
                                       height = 400, 
                                       dataMode = 'regions'))
# размещаем таблицу и карту на одной панели (слева направо)
TG <- gvisMerge(g.tbl, g.chart, 
                horizontal = TRUE, 
                tableOptions = 'bgcolor=\"#CCCCCC\" cellspacing=10')

# вставляем результат в html-документ
print(TG, 'chart')
```


# Карта на основе leaflet  
На этой карте показаны 5 популярных стадионов России:

* Арена ЦСКА
* РЖД Арена
* Стадион Ростов Арена
* Стадион Санкт-Петербург
* Стадион ФК Краснодар

```{r, echo = T, message=FALSE, warning=F, results='asis'}
library(leaflet)
parks <- data.frame(place = c("Арена ЦСКА", "РЖД Арена",
                                 "Стадион Ростов Арена", "Стадион Санкт-Петербург", "Стадион ФК Краснодар"),
latitude = c(55.791744, 55.803611, 47.209482, 59.973137,45.044506 ),
longitude = c(37.516123, 37.741708, 39.737280, 30.220814,39.028921 ))
parks %>% leaflet() %>% addTiles() %>%
 addMarkers(popup = parks$place,
 clusterOptions = markerClusterOptions()) %>%
 addCircles(weight = 1, radius = 10)
```

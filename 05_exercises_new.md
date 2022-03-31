---
title: 'Weekly Exercises #5'
author: "Wenxuan Zhu"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
library(ggimage)
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels and alt text.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.

```r
data(garden_harvest)
```


```r
garden_plot <- garden_harvest %>% 
  ggplot(aes(x = weight, y = vegetable)) + 
  geom_line(color  = "blue") +
  labs(title = "Weight for vegetable in 2020", x = "Weight in grams", y = "species") +
  theme_minimal()

ggplotly(garden_plot)
```

```{=html}
<div id="htmlwidget-2b7278b5283122712ff9" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-2b7278b5283122712ff9">{"x":{"data":[{"x":[156,null,20,null,3,5,7,9,11,12,13,16,17,24,25,27,34,137,150,null,21,41,65,78,81,94,100,105,108,122,122,129,140,160,178,186,225,234,235,245,287,305,320,328,350,351,443,468,519,545,572,592,660,693,701,728,743,755,null,8,10,11,25,49,57,62,69,83,89,101,107,149,172,198,308,2209,2476,null,75,102,134,219,372,437,null,56,80,107,116,160,160,164,168,169,174,178,221,255,442,449,457,883,888,1023,1500,null,8,null,2,17,33,null,214,330,344,383,661,798,1564,1607,null,47,70,76,78,127,130,152,174,179,179,181,233,294,347,351,514,525,531,599,626,655,747,785,901,997,1029,1057,1130,1155,1327,1697,2888,null,109,483,527,1644,null,12,24,31,40,559,null,20,43,58,74,87,102,119,162,175,197,258,352,509,2322,null,10,28,39,60,61,71,108,113,117,121,127,128,128,137,145,163,173,280,305,383,null,191,null,8,10,12,13,14,15,16,17,18,18,19,19,20,22,23,23,30,38,39,39,45,47,48,52,52,53,56,56,58,60,61,61,65,67,67,73,73,75,79,80,81,82,83,85,87,89,91,92,94,95,95,98,99,99,107,111,111,122,123,130,134,137,139,144,147,189,216,217,322,null,19,33,50,77,91,102,113,118,126,129,137,182,183,195,289,null,8,19,34,40,40,40,48,71,75,121,140,148,165,207,252,285,333,336,425,433,457,526,561,625,743,793,798,null,60,68,70,81,84,84,89,92,112,115,144,169,192,252,270,328,370,627,1031,null,101,168,262,272,272,278,293,295,317,323,372,436,439,629,716,843,1527,1596,1718,null,1028,1109,1131,1154,1208,1300,1302,1311,1359,1570,1608,1743,2277,2342,2689,2882,2931,2990,3441,4372,4758,5000,6250,7050,7350,null,16,36,37,39,43,50,53,67,88,null,29,29,30,32,60,61,77,105,131,137,152,null,52,114,144,297,366,419,673,740,883,903,932,1026,1096,1101,1350,1393,2001,null,9,14,16,19,21,22,25,30,31,37,39,39,41,44,51,58,58,59,60,71,80,99,null,252,294,296,307,312,314,397,437,454,480,484,494,537,706,709,1172,1178,1183,1291,1558,1627,1655,1686,1785,1834,1923,1927,1950,2120,2143,2325,2710,3227,5150,null,17,19,23,37,40,77,85,88,93,113,null,13,19,23,24,29,32,88,96,178,228,232,256,302,309,310,466,517,null,24,31,40,54,64,64,67,70,73,75,81,86,91,95,97,100,102,105,106,115,118,126,131,136,137,140,148,148,153,155,156,160,162,168,175,178,179,181,184,197,200,201,203,209,211,212,213,216,216,217,218,219,220,223,228,230,230,231,233,236,238,238,240,241,243,247,252,254,255,258,261,264,265,266,269,271,272,290,290,291,292,302,303,304,305,306,306,307,307,307,308,308,309,312,315,316,316,317,320,327,328,330,332,333,335,336,339,339,344,346,354,356,359,359,360,363,364,364,375,380,382,386,387,393,393,397,413,413,418,421,428,430,435,436,436,436,442,442,446,451,460,463,477,477,483,484,488,490,493,494,497,506,506,509,509,525,526,543,560,562,562,563,564,566,578,579,588,589,591,593,599,599,608,610,611,615,615,619,629,632,633,642,646,659,666,670,674,678,692,704,710,711,714,714,715,725,727,737,752,754,764,773,781,791,801,801,802,805,819,819,822,833,842,857,859,861,871,872,898,993,997,1010,1026,1033,1042,1045,1052,1058,1097,1102,1131,1175,1178,1202,1234,1239,1265,1317,1377,1392,1400,1447,1537,1538,1542,1573,1601,1631,1649,1823,1886,1953,1977,1978,2006,2160,2377,2478,2738,2838,2899,2934,2934,null,76,81,110,134,145,164,175,252,344,371,393,427,443,445,457,492,660,731,834,859,920,1122,1151,1175,1215,1219,1300,1321,1542,1774,2436,2831,3244,3284,3600,3800,5700],"y":[1,null,2,null,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,null,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,null,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,null,6,6,6,6,6,6,null,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,null,8,null,9,9,9,null,10,10,10,10,10,10,10,10,null,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,null,12,12,12,12,null,13,13,13,13,13,null,14,14,14,14,14,14,14,14,14,14,14,14,14,14,null,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,null,16,null,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,null,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,null,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,null,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,null,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,null,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,null,23,23,23,23,23,23,23,23,23,null,24,24,24,24,24,24,24,24,24,24,24,null,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,null,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,null,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,null,28,28,28,28,28,28,28,28,28,28,null,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,null,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,null,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31],"text":["weight:  156<br />vegetable: apple",null,"weight:   20<br />vegetable: asparagus",null,"weight:    3<br />vegetable: basil","weight:    5<br />vegetable: basil","weight:    7<br />vegetable: basil","weight:    9<br />vegetable: basil","weight:   11<br />vegetable: basil","weight:   12<br />vegetable: basil","weight:   13<br />vegetable: basil","weight:   16<br />vegetable: basil","weight:   17<br />vegetable: basil","weight:   24<br />vegetable: basil","weight:   25<br />vegetable: basil","weight:   27<br />vegetable: basil","weight:   34<br />vegetable: basil","weight:  137<br />vegetable: basil","weight:  150<br />vegetable: basil",null,"weight:   21<br />vegetable: beans","weight:   41<br />vegetable: beans","weight:   65<br />vegetable: beans","weight:   78<br />vegetable: beans","weight:   81<br />vegetable: beans","weight:   94<br />vegetable: beans","weight:  100<br />vegetable: beans","weight:  105<br />vegetable: beans","weight:  108<br />vegetable: beans","weight:  122<br />vegetable: beans","weight:  122<br />vegetable: beans","weight:  129<br />vegetable: beans","weight:  140<br />vegetable: beans","weight:  160<br />vegetable: beans","weight:  178<br />vegetable: beans","weight:  186<br />vegetable: beans","weight:  225<br />vegetable: beans","weight:  234<br />vegetable: beans","weight:  235<br />vegetable: beans","weight:  245<br />vegetable: beans","weight:  287<br />vegetable: beans","weight:  305<br />vegetable: beans","weight:  320<br />vegetable: beans","weight:  328<br />vegetable: beans","weight:  350<br />vegetable: beans","weight:  351<br />vegetable: beans","weight:  443<br />vegetable: beans","weight:  468<br />vegetable: beans","weight:  519<br />vegetable: beans","weight:  545<br />vegetable: beans","weight:  572<br />vegetable: beans","weight:  592<br />vegetable: beans","weight:  660<br />vegetable: beans","weight:  693<br />vegetable: beans","weight:  701<br />vegetable: beans","weight:  728<br />vegetable: beans","weight:  743<br />vegetable: beans","weight:  755<br />vegetable: beans",null,"weight:    8<br />vegetable: beets","weight:   10<br />vegetable: beets","weight:   11<br />vegetable: beets","weight:   25<br />vegetable: beets","weight:   49<br />vegetable: beets","weight:   57<br />vegetable: beets","weight:   62<br />vegetable: beets","weight:   69<br />vegetable: beets","weight:   83<br />vegetable: beets","weight:   89<br />vegetable: beets","weight:  101<br />vegetable: beets","weight:  107<br />vegetable: beets","weight:  149<br />vegetable: beets","weight:  172<br />vegetable: beets","weight:  198<br />vegetable: beets","weight:  308<br />vegetable: beets","weight: 2209<br />vegetable: beets","weight: 2476<br />vegetable: beets",null,"weight:   75<br />vegetable: broccoli","weight:  102<br />vegetable: broccoli","weight:  134<br />vegetable: broccoli","weight:  219<br />vegetable: broccoli","weight:  372<br />vegetable: broccoli","weight:  437<br />vegetable: broccoli",null,"weight:   56<br />vegetable: carrots","weight:   80<br />vegetable: carrots","weight:  107<br />vegetable: carrots","weight:  116<br />vegetable: carrots","weight:  160<br />vegetable: carrots","weight:  160<br />vegetable: carrots","weight:  164<br />vegetable: carrots","weight:  168<br />vegetable: carrots","weight:  169<br />vegetable: carrots","weight:  174<br />vegetable: carrots","weight:  178<br />vegetable: carrots","weight:  221<br />vegetable: carrots","weight:  255<br />vegetable: carrots","weight:  442<br />vegetable: carrots","weight:  449<br />vegetable: carrots","weight:  457<br />vegetable: carrots","weight:  883<br />vegetable: carrots","weight:  888<br />vegetable: carrots","weight: 1023<br />vegetable: carrots","weight: 1500<br />vegetable: carrots",null,"weight:    8<br />vegetable: chives",null,"weight:    2<br />vegetable: cilantro","weight:   17<br />vegetable: cilantro","weight:   33<br />vegetable: cilantro",null,"weight:  214<br />vegetable: corn","weight:  330<br />vegetable: corn","weight:  344<br />vegetable: corn","weight:  383<br />vegetable: corn","weight:  661<br />vegetable: corn","weight:  798<br />vegetable: corn","weight: 1564<br />vegetable: corn","weight: 1607<br />vegetable: corn",null,"weight:   47<br />vegetable: cucumbers","weight:   70<br />vegetable: cucumbers","weight:   76<br />vegetable: cucumbers","weight:   78<br />vegetable: cucumbers","weight:  127<br />vegetable: cucumbers","weight:  130<br />vegetable: cucumbers","weight:  152<br />vegetable: cucumbers","weight:  174<br />vegetable: cucumbers","weight:  179<br />vegetable: cucumbers","weight:  179<br />vegetable: cucumbers","weight:  181<br />vegetable: cucumbers","weight:  233<br />vegetable: cucumbers","weight:  294<br />vegetable: cucumbers","weight:  347<br />vegetable: cucumbers","weight:  351<br />vegetable: cucumbers","weight:  514<br />vegetable: cucumbers","weight:  525<br />vegetable: cucumbers","weight:  531<br />vegetable: cucumbers","weight:  599<br />vegetable: cucumbers","weight:  626<br />vegetable: cucumbers","weight:  655<br />vegetable: cucumbers","weight:  747<br />vegetable: cucumbers","weight:  785<br />vegetable: cucumbers","weight:  901<br />vegetable: cucumbers","weight:  997<br />vegetable: cucumbers","weight: 1029<br />vegetable: cucumbers","weight: 1057<br />vegetable: cucumbers","weight: 1130<br />vegetable: cucumbers","weight: 1155<br />vegetable: cucumbers","weight: 1327<br />vegetable: cucumbers","weight: 1697<br />vegetable: cucumbers","weight: 2888<br />vegetable: cucumbers",null,"weight:  109<br />vegetable: edamame","weight:  483<br />vegetable: edamame","weight:  527<br />vegetable: edamame","weight: 1644<br />vegetable: edamame",null,"weight:   12<br />vegetable: hot peppers","weight:   24<br />vegetable: hot peppers","weight:   31<br />vegetable: hot peppers","weight:   40<br />vegetable: hot peppers","weight:  559<br />vegetable: hot peppers",null,"weight:   20<br />vegetable: jalapeño","weight:   43<br />vegetable: jalapeño","weight:   58<br />vegetable: jalapeño","weight:   74<br />vegetable: jalapeño","weight:   87<br />vegetable: jalapeño","weight:  102<br />vegetable: jalapeño","weight:  119<br />vegetable: jalapeño","weight:  162<br />vegetable: jalapeño","weight:  175<br />vegetable: jalapeño","weight:  197<br />vegetable: jalapeño","weight:  258<br />vegetable: jalapeño","weight:  352<br />vegetable: jalapeño","weight:  509<br />vegetable: jalapeño","weight: 2322<br />vegetable: jalapeño",null,"weight:   10<br />vegetable: kale","weight:   28<br />vegetable: kale","weight:   39<br />vegetable: kale","weight:   60<br />vegetable: kale","weight:   61<br />vegetable: kale","weight:   71<br />vegetable: kale","weight:  108<br />vegetable: kale","weight:  113<br />vegetable: kale","weight:  117<br />vegetable: kale","weight:  121<br />vegetable: kale","weight:  127<br />vegetable: kale","weight:  128<br />vegetable: kale","weight:  128<br />vegetable: kale","weight:  137<br />vegetable: kale","weight:  145<br />vegetable: kale","weight:  163<br />vegetable: kale","weight:  173<br />vegetable: kale","weight:  280<br />vegetable: kale","weight:  305<br />vegetable: kale","weight:  383<br />vegetable: kale",null,"weight:  191<br />vegetable: kohlrabi",null,"weight:    8<br />vegetable: lettuce","weight:   10<br />vegetable: lettuce","weight:   12<br />vegetable: lettuce","weight:   13<br />vegetable: lettuce","weight:   14<br />vegetable: lettuce","weight:   15<br />vegetable: lettuce","weight:   16<br />vegetable: lettuce","weight:   17<br />vegetable: lettuce","weight:   18<br />vegetable: lettuce","weight:   18<br />vegetable: lettuce","weight:   19<br />vegetable: lettuce","weight:   19<br />vegetable: lettuce","weight:   20<br />vegetable: lettuce","weight:   22<br />vegetable: lettuce","weight:   23<br />vegetable: lettuce","weight:   23<br />vegetable: lettuce","weight:   30<br />vegetable: lettuce","weight:   38<br />vegetable: lettuce","weight:   39<br />vegetable: lettuce","weight:   39<br />vegetable: lettuce","weight:   45<br />vegetable: lettuce","weight:   47<br />vegetable: lettuce","weight:   48<br />vegetable: lettuce","weight:   52<br />vegetable: lettuce","weight:   52<br />vegetable: lettuce","weight:   53<br />vegetable: lettuce","weight:   56<br />vegetable: lettuce","weight:   56<br />vegetable: lettuce","weight:   58<br />vegetable: lettuce","weight:   60<br />vegetable: lettuce","weight:   61<br />vegetable: lettuce","weight:   61<br />vegetable: lettuce","weight:   65<br />vegetable: lettuce","weight:   67<br />vegetable: lettuce","weight:   67<br />vegetable: lettuce","weight:   73<br />vegetable: lettuce","weight:   73<br />vegetable: lettuce","weight:   75<br />vegetable: lettuce","weight:   79<br />vegetable: lettuce","weight:   80<br />vegetable: lettuce","weight:   81<br />vegetable: lettuce","weight:   82<br />vegetable: lettuce","weight:   83<br />vegetable: lettuce","weight:   85<br />vegetable: lettuce","weight:   87<br />vegetable: lettuce","weight:   89<br />vegetable: lettuce","weight:   91<br />vegetable: lettuce","weight:   92<br />vegetable: lettuce","weight:   94<br />vegetable: lettuce","weight:   95<br />vegetable: lettuce","weight:   95<br />vegetable: lettuce","weight:   98<br />vegetable: lettuce","weight:   99<br />vegetable: lettuce","weight:   99<br />vegetable: lettuce","weight:  107<br />vegetable: lettuce","weight:  111<br />vegetable: lettuce","weight:  111<br />vegetable: lettuce","weight:  122<br />vegetable: lettuce","weight:  123<br />vegetable: lettuce","weight:  130<br />vegetable: lettuce","weight:  134<br />vegetable: lettuce","weight:  137<br />vegetable: lettuce","weight:  139<br />vegetable: lettuce","weight:  144<br />vegetable: lettuce","weight:  147<br />vegetable: lettuce","weight:  189<br />vegetable: lettuce","weight:  216<br />vegetable: lettuce","weight:  217<br />vegetable: lettuce","weight:  322<br />vegetable: lettuce",null,"weight:   19<br />vegetable: onions","weight:   33<br />vegetable: onions","weight:   50<br />vegetable: onions","weight:   77<br />vegetable: onions","weight:   91<br />vegetable: onions","weight:  102<br />vegetable: onions","weight:  113<br />vegetable: onions","weight:  118<br />vegetable: onions","weight:  126<br />vegetable: onions","weight:  129<br />vegetable: onions","weight:  137<br />vegetable: onions","weight:  182<br />vegetable: onions","weight:  183<br />vegetable: onions","weight:  195<br />vegetable: onions","weight:  289<br />vegetable: onions",null,"weight:    8<br />vegetable: peas","weight:   19<br />vegetable: peas","weight:   34<br />vegetable: peas","weight:   40<br />vegetable: peas","weight:   40<br />vegetable: peas","weight:   40<br />vegetable: peas","weight:   48<br />vegetable: peas","weight:   71<br />vegetable: peas","weight:   75<br />vegetable: peas","weight:  121<br />vegetable: peas","weight:  140<br />vegetable: peas","weight:  148<br />vegetable: peas","weight:  165<br />vegetable: peas","weight:  207<br />vegetable: peas","weight:  252<br />vegetable: peas","weight:  285<br />vegetable: peas","weight:  333<br />vegetable: peas","weight:  336<br />vegetable: peas","weight:  425<br />vegetable: peas","weight:  433<br />vegetable: peas","weight:  457<br />vegetable: peas","weight:  526<br />vegetable: peas","weight:  561<br />vegetable: peas","weight:  625<br />vegetable: peas","weight:  743<br />vegetable: peas","weight:  793<br />vegetable: peas","weight:  798<br />vegetable: peas",null,"weight:   60<br />vegetable: peppers","weight:   68<br />vegetable: peppers","weight:   70<br />vegetable: peppers","weight:   81<br />vegetable: peppers","weight:   84<br />vegetable: peppers","weight:   84<br />vegetable: peppers","weight:   89<br />vegetable: peppers","weight:   92<br />vegetable: peppers","weight:  112<br />vegetable: peppers","weight:  115<br />vegetable: peppers","weight:  144<br />vegetable: peppers","weight:  169<br />vegetable: peppers","weight:  192<br />vegetable: peppers","weight:  252<br />vegetable: peppers","weight:  270<br />vegetable: peppers","weight:  328<br />vegetable: peppers","weight:  370<br />vegetable: peppers","weight:  627<br />vegetable: peppers","weight: 1031<br />vegetable: peppers",null,"weight:  101<br />vegetable: potatoes","weight:  168<br />vegetable: potatoes","weight:  262<br />vegetable: potatoes","weight:  272<br />vegetable: potatoes","weight:  272<br />vegetable: potatoes","weight:  278<br />vegetable: potatoes","weight:  293<br />vegetable: potatoes","weight:  295<br />vegetable: potatoes","weight:  317<br />vegetable: potatoes","weight:  323<br />vegetable: potatoes","weight:  372<br />vegetable: potatoes","weight:  436<br />vegetable: potatoes","weight:  439<br />vegetable: potatoes","weight:  629<br />vegetable: potatoes","weight:  716<br />vegetable: potatoes","weight:  843<br />vegetable: potatoes","weight: 1527<br />vegetable: potatoes","weight: 1596<br />vegetable: potatoes","weight: 1718<br />vegetable: potatoes",null,"weight: 1028<br />vegetable: pumpkins","weight: 1109<br />vegetable: pumpkins","weight: 1131<br />vegetable: pumpkins","weight: 1154<br />vegetable: pumpkins","weight: 1208<br />vegetable: pumpkins","weight: 1300<br />vegetable: pumpkins","weight: 1302<br />vegetable: pumpkins","weight: 1311<br />vegetable: pumpkins","weight: 1359<br />vegetable: pumpkins","weight: 1570<br />vegetable: pumpkins","weight: 1608<br />vegetable: pumpkins","weight: 1743<br />vegetable: pumpkins","weight: 2277<br />vegetable: pumpkins","weight: 2342<br />vegetable: pumpkins","weight: 2689<br />vegetable: pumpkins","weight: 2882<br />vegetable: pumpkins","weight: 2931<br />vegetable: pumpkins","weight: 2990<br />vegetable: pumpkins","weight: 3441<br />vegetable: pumpkins","weight: 4372<br />vegetable: pumpkins","weight: 4758<br />vegetable: pumpkins","weight: 5000<br />vegetable: pumpkins","weight: 6250<br />vegetable: pumpkins","weight: 7050<br />vegetable: pumpkins","weight: 7350<br />vegetable: pumpkins",null,"weight:   16<br />vegetable: radish","weight:   36<br />vegetable: radish","weight:   37<br />vegetable: radish","weight:   39<br />vegetable: radish","weight:   43<br />vegetable: radish","weight:   50<br />vegetable: radish","weight:   53<br />vegetable: radish","weight:   67<br />vegetable: radish","weight:   88<br />vegetable: radish",null,"weight:   29<br />vegetable: raspberries","weight:   29<br />vegetable: raspberries","weight:   30<br />vegetable: raspberries","weight:   32<br />vegetable: raspberries","weight:   60<br />vegetable: raspberries","weight:   61<br />vegetable: raspberries","weight:   77<br />vegetable: raspberries","weight:  105<br />vegetable: raspberries","weight:  131<br />vegetable: raspberries","weight:  137<br />vegetable: raspberries","weight:  152<br />vegetable: raspberries",null,"weight:   52<br />vegetable: rutabaga","weight:  114<br />vegetable: rutabaga","weight:  144<br />vegetable: rutabaga","weight:  297<br />vegetable: rutabaga","weight:  366<br />vegetable: rutabaga","weight:  419<br />vegetable: rutabaga","weight:  673<br />vegetable: rutabaga","weight:  740<br />vegetable: rutabaga","weight:  883<br />vegetable: rutabaga","weight:  903<br />vegetable: rutabaga","weight:  932<br />vegetable: rutabaga","weight: 1026<br />vegetable: rutabaga","weight: 1096<br />vegetable: rutabaga","weight: 1101<br />vegetable: rutabaga","weight: 1350<br />vegetable: rutabaga","weight: 1393<br />vegetable: rutabaga","weight: 2001<br />vegetable: rutabaga",null,"weight:    9<br />vegetable: spinach","weight:   14<br />vegetable: spinach","weight:   16<br />vegetable: spinach","weight:   19<br />vegetable: spinach","weight:   21<br />vegetable: spinach","weight:   22<br />vegetable: spinach","weight:   25<br />vegetable: spinach","weight:   30<br />vegetable: spinach","weight:   31<br />vegetable: spinach","weight:   37<br />vegetable: spinach","weight:   39<br />vegetable: spinach","weight:   39<br />vegetable: spinach","weight:   41<br />vegetable: spinach","weight:   44<br />vegetable: spinach","weight:   51<br />vegetable: spinach","weight:   58<br />vegetable: spinach","weight:   58<br />vegetable: spinach","weight:   59<br />vegetable: spinach","weight:   60<br />vegetable: spinach","weight:   71<br />vegetable: spinach","weight:   80<br />vegetable: spinach","weight:   99<br />vegetable: spinach",null,"weight:  252<br />vegetable: squash","weight:  294<br />vegetable: squash","weight:  296<br />vegetable: squash","weight:  307<br />vegetable: squash","weight:  312<br />vegetable: squash","weight:  314<br />vegetable: squash","weight:  397<br />vegetable: squash","weight:  437<br />vegetable: squash","weight:  454<br />vegetable: squash","weight:  480<br />vegetable: squash","weight:  484<br />vegetable: squash","weight:  494<br />vegetable: squash","weight:  537<br />vegetable: squash","weight:  706<br />vegetable: squash","weight:  709<br />vegetable: squash","weight: 1172<br />vegetable: squash","weight: 1178<br />vegetable: squash","weight: 1183<br />vegetable: squash","weight: 1291<br />vegetable: squash","weight: 1558<br />vegetable: squash","weight: 1627<br />vegetable: squash","weight: 1655<br />vegetable: squash","weight: 1686<br />vegetable: squash","weight: 1785<br />vegetable: squash","weight: 1834<br />vegetable: squash","weight: 1923<br />vegetable: squash","weight: 1927<br />vegetable: squash","weight: 1950<br />vegetable: squash","weight: 2120<br />vegetable: squash","weight: 2143<br />vegetable: squash","weight: 2325<br />vegetable: squash","weight: 2710<br />vegetable: squash","weight: 3227<br />vegetable: squash","weight: 5150<br />vegetable: squash",null,"weight:   17<br />vegetable: strawberries","weight:   19<br />vegetable: strawberries","weight:   23<br />vegetable: strawberries","weight:   37<br />vegetable: strawberries","weight:   40<br />vegetable: strawberries","weight:   77<br />vegetable: strawberries","weight:   85<br />vegetable: strawberries","weight:   88<br />vegetable: strawberries","weight:   93<br />vegetable: strawberries","weight:  113<br />vegetable: strawberries",null,"weight:   13<br />vegetable: Swiss chard","weight:   19<br />vegetable: Swiss chard","weight:   23<br />vegetable: Swiss chard","weight:   24<br />vegetable: Swiss chard","weight:   29<br />vegetable: Swiss chard","weight:   32<br />vegetable: Swiss chard","weight:   88<br />vegetable: Swiss chard","weight:   96<br />vegetable: Swiss chard","weight:  178<br />vegetable: Swiss chard","weight:  228<br />vegetable: Swiss chard","weight:  232<br />vegetable: Swiss chard","weight:  256<br />vegetable: Swiss chard","weight:  302<br />vegetable: Swiss chard","weight:  309<br />vegetable: Swiss chard","weight:  310<br />vegetable: Swiss chard","weight:  466<br />vegetable: Swiss chard","weight:  517<br />vegetable: Swiss chard",null,"weight:   24<br />vegetable: tomatoes","weight:   31<br />vegetable: tomatoes","weight:   40<br />vegetable: tomatoes","weight:   54<br />vegetable: tomatoes","weight:   64<br />vegetable: tomatoes","weight:   64<br />vegetable: tomatoes","weight:   67<br />vegetable: tomatoes","weight:   70<br />vegetable: tomatoes","weight:   73<br />vegetable: tomatoes","weight:   75<br />vegetable: tomatoes","weight:   81<br />vegetable: tomatoes","weight:   86<br />vegetable: tomatoes","weight:   91<br />vegetable: tomatoes","weight:   95<br />vegetable: tomatoes","weight:   97<br />vegetable: tomatoes","weight:  100<br />vegetable: tomatoes","weight:  102<br />vegetable: tomatoes","weight:  105<br />vegetable: tomatoes","weight:  106<br />vegetable: tomatoes","weight:  115<br />vegetable: tomatoes","weight:  118<br />vegetable: tomatoes","weight:  126<br />vegetable: tomatoes","weight:  131<br />vegetable: tomatoes","weight:  136<br />vegetable: tomatoes","weight:  137<br />vegetable: tomatoes","weight:  140<br />vegetable: tomatoes","weight:  148<br />vegetable: tomatoes","weight:  148<br />vegetable: tomatoes","weight:  153<br />vegetable: tomatoes","weight:  155<br />vegetable: tomatoes","weight:  156<br />vegetable: tomatoes","weight:  160<br />vegetable: tomatoes","weight:  162<br />vegetable: tomatoes","weight:  168<br />vegetable: tomatoes","weight:  175<br />vegetable: tomatoes","weight:  178<br />vegetable: tomatoes","weight:  179<br />vegetable: tomatoes","weight:  181<br />vegetable: tomatoes","weight:  184<br />vegetable: tomatoes","weight:  197<br />vegetable: tomatoes","weight:  200<br />vegetable: tomatoes","weight:  201<br />vegetable: tomatoes","weight:  203<br />vegetable: tomatoes","weight:  209<br />vegetable: tomatoes","weight:  211<br />vegetable: tomatoes","weight:  212<br />vegetable: tomatoes","weight:  213<br />vegetable: tomatoes","weight:  216<br />vegetable: tomatoes","weight:  216<br />vegetable: tomatoes","weight:  217<br />vegetable: tomatoes","weight:  218<br />vegetable: tomatoes","weight:  219<br />vegetable: tomatoes","weight:  220<br />vegetable: tomatoes","weight:  223<br />vegetable: tomatoes","weight:  228<br />vegetable: tomatoes","weight:  230<br />vegetable: tomatoes","weight:  230<br />vegetable: tomatoes","weight:  231<br />vegetable: tomatoes","weight:  233<br />vegetable: tomatoes","weight:  236<br />vegetable: tomatoes","weight:  238<br />vegetable: tomatoes","weight:  238<br />vegetable: tomatoes","weight:  240<br />vegetable: tomatoes","weight:  241<br />vegetable: tomatoes","weight:  243<br />vegetable: tomatoes","weight:  247<br />vegetable: tomatoes","weight:  252<br />vegetable: tomatoes","weight:  254<br />vegetable: tomatoes","weight:  255<br />vegetable: tomatoes","weight:  258<br />vegetable: tomatoes","weight:  261<br />vegetable: tomatoes","weight:  264<br />vegetable: tomatoes","weight:  265<br />vegetable: tomatoes","weight:  266<br />vegetable: tomatoes","weight:  269<br />vegetable: tomatoes","weight:  271<br />vegetable: tomatoes","weight:  272<br />vegetable: tomatoes","weight:  290<br />vegetable: tomatoes","weight:  290<br />vegetable: tomatoes","weight:  291<br />vegetable: tomatoes","weight:  292<br />vegetable: tomatoes","weight:  302<br />vegetable: tomatoes","weight:  303<br />vegetable: tomatoes","weight:  304<br />vegetable: tomatoes","weight:  305<br />vegetable: tomatoes","weight:  306<br />vegetable: tomatoes","weight:  306<br />vegetable: tomatoes","weight:  307<br />vegetable: tomatoes","weight:  307<br />vegetable: tomatoes","weight:  307<br />vegetable: tomatoes","weight:  308<br />vegetable: tomatoes","weight:  308<br />vegetable: tomatoes","weight:  309<br />vegetable: tomatoes","weight:  312<br />vegetable: tomatoes","weight:  315<br />vegetable: tomatoes","weight:  316<br />vegetable: tomatoes","weight:  316<br />vegetable: tomatoes","weight:  317<br />vegetable: tomatoes","weight:  320<br />vegetable: tomatoes","weight:  327<br />vegetable: tomatoes","weight:  328<br />vegetable: tomatoes","weight:  330<br />vegetable: tomatoes","weight:  332<br />vegetable: tomatoes","weight:  333<br />vegetable: tomatoes","weight:  335<br />vegetable: tomatoes","weight:  336<br />vegetable: tomatoes","weight:  339<br />vegetable: tomatoes","weight:  339<br />vegetable: tomatoes","weight:  344<br />vegetable: tomatoes","weight:  346<br />vegetable: tomatoes","weight:  354<br />vegetable: tomatoes","weight:  356<br />vegetable: tomatoes","weight:  359<br />vegetable: tomatoes","weight:  359<br />vegetable: tomatoes","weight:  360<br />vegetable: tomatoes","weight:  363<br />vegetable: tomatoes","weight:  364<br />vegetable: tomatoes","weight:  364<br />vegetable: tomatoes","weight:  375<br />vegetable: tomatoes","weight:  380<br />vegetable: tomatoes","weight:  382<br />vegetable: tomatoes","weight:  386<br />vegetable: tomatoes","weight:  387<br />vegetable: tomatoes","weight:  393<br />vegetable: tomatoes","weight:  393<br />vegetable: tomatoes","weight:  397<br />vegetable: tomatoes","weight:  413<br />vegetable: tomatoes","weight:  413<br />vegetable: tomatoes","weight:  418<br />vegetable: tomatoes","weight:  421<br />vegetable: tomatoes","weight:  428<br />vegetable: tomatoes","weight:  430<br />vegetable: tomatoes","weight:  435<br />vegetable: tomatoes","weight:  436<br />vegetable: tomatoes","weight:  436<br />vegetable: tomatoes","weight:  436<br />vegetable: tomatoes","weight:  442<br />vegetable: tomatoes","weight:  442<br />vegetable: tomatoes","weight:  446<br />vegetable: tomatoes","weight:  451<br />vegetable: tomatoes","weight:  460<br />vegetable: tomatoes","weight:  463<br />vegetable: tomatoes","weight:  477<br />vegetable: tomatoes","weight:  477<br />vegetable: tomatoes","weight:  483<br />vegetable: tomatoes","weight:  484<br />vegetable: tomatoes","weight:  488<br />vegetable: tomatoes","weight:  490<br />vegetable: tomatoes","weight:  493<br />vegetable: tomatoes","weight:  494<br />vegetable: tomatoes","weight:  497<br />vegetable: tomatoes","weight:  506<br />vegetable: tomatoes","weight:  506<br />vegetable: tomatoes","weight:  509<br />vegetable: tomatoes","weight:  509<br />vegetable: tomatoes","weight:  525<br />vegetable: tomatoes","weight:  526<br />vegetable: tomatoes","weight:  543<br />vegetable: tomatoes","weight:  560<br />vegetable: tomatoes","weight:  562<br />vegetable: tomatoes","weight:  562<br />vegetable: tomatoes","weight:  563<br />vegetable: tomatoes","weight:  564<br />vegetable: tomatoes","weight:  566<br />vegetable: tomatoes","weight:  578<br />vegetable: tomatoes","weight:  579<br />vegetable: tomatoes","weight:  588<br />vegetable: tomatoes","weight:  589<br />vegetable: tomatoes","weight:  591<br />vegetable: tomatoes","weight:  593<br />vegetable: tomatoes","weight:  599<br />vegetable: tomatoes","weight:  599<br />vegetable: tomatoes","weight:  608<br />vegetable: tomatoes","weight:  610<br />vegetable: tomatoes","weight:  611<br />vegetable: tomatoes","weight:  615<br />vegetable: tomatoes","weight:  615<br />vegetable: tomatoes","weight:  619<br />vegetable: tomatoes","weight:  629<br />vegetable: tomatoes","weight:  632<br />vegetable: tomatoes","weight:  633<br />vegetable: tomatoes","weight:  642<br />vegetable: tomatoes","weight:  646<br />vegetable: tomatoes","weight:  659<br />vegetable: tomatoes","weight:  666<br />vegetable: tomatoes","weight:  670<br />vegetable: tomatoes","weight:  674<br />vegetable: tomatoes","weight:  678<br />vegetable: tomatoes","weight:  692<br />vegetable: tomatoes","weight:  704<br />vegetable: tomatoes","weight:  710<br />vegetable: tomatoes","weight:  711<br />vegetable: tomatoes","weight:  714<br />vegetable: tomatoes","weight:  714<br />vegetable: tomatoes","weight:  715<br />vegetable: tomatoes","weight:  725<br />vegetable: tomatoes","weight:  727<br />vegetable: tomatoes","weight:  737<br />vegetable: tomatoes","weight:  752<br />vegetable: tomatoes","weight:  754<br />vegetable: tomatoes","weight:  764<br />vegetable: tomatoes","weight:  773<br />vegetable: tomatoes","weight:  781<br />vegetable: tomatoes","weight:  791<br />vegetable: tomatoes","weight:  801<br />vegetable: tomatoes","weight:  801<br />vegetable: tomatoes","weight:  802<br />vegetable: tomatoes","weight:  805<br />vegetable: tomatoes","weight:  819<br />vegetable: tomatoes","weight:  819<br />vegetable: tomatoes","weight:  822<br />vegetable: tomatoes","weight:  833<br />vegetable: tomatoes","weight:  842<br />vegetable: tomatoes","weight:  857<br />vegetable: tomatoes","weight:  859<br />vegetable: tomatoes","weight:  861<br />vegetable: tomatoes","weight:  871<br />vegetable: tomatoes","weight:  872<br />vegetable: tomatoes","weight:  898<br />vegetable: tomatoes","weight:  993<br />vegetable: tomatoes","weight:  997<br />vegetable: tomatoes","weight: 1010<br />vegetable: tomatoes","weight: 1026<br />vegetable: tomatoes","weight: 1033<br />vegetable: tomatoes","weight: 1042<br />vegetable: tomatoes","weight: 1045<br />vegetable: tomatoes","weight: 1052<br />vegetable: tomatoes","weight: 1058<br />vegetable: tomatoes","weight: 1097<br />vegetable: tomatoes","weight: 1102<br />vegetable: tomatoes","weight: 1131<br />vegetable: tomatoes","weight: 1175<br />vegetable: tomatoes","weight: 1178<br />vegetable: tomatoes","weight: 1202<br />vegetable: tomatoes","weight: 1234<br />vegetable: tomatoes","weight: 1239<br />vegetable: tomatoes","weight: 1265<br />vegetable: tomatoes","weight: 1317<br />vegetable: tomatoes","weight: 1377<br />vegetable: tomatoes","weight: 1392<br />vegetable: tomatoes","weight: 1400<br />vegetable: tomatoes","weight: 1447<br />vegetable: tomatoes","weight: 1537<br />vegetable: tomatoes","weight: 1538<br />vegetable: tomatoes","weight: 1542<br />vegetable: tomatoes","weight: 1573<br />vegetable: tomatoes","weight: 1601<br />vegetable: tomatoes","weight: 1631<br />vegetable: tomatoes","weight: 1649<br />vegetable: tomatoes","weight: 1823<br />vegetable: tomatoes","weight: 1886<br />vegetable: tomatoes","weight: 1953<br />vegetable: tomatoes","weight: 1977<br />vegetable: tomatoes","weight: 1978<br />vegetable: tomatoes","weight: 2006<br />vegetable: tomatoes","weight: 2160<br />vegetable: tomatoes","weight: 2377<br />vegetable: tomatoes","weight: 2478<br />vegetable: tomatoes","weight: 2738<br />vegetable: tomatoes","weight: 2838<br />vegetable: tomatoes","weight: 2899<br />vegetable: tomatoes","weight: 2934<br />vegetable: tomatoes","weight: 2934<br />vegetable: tomatoes",null,"weight:   76<br />vegetable: zucchini","weight:   81<br />vegetable: zucchini","weight:  110<br />vegetable: zucchini","weight:  134<br />vegetable: zucchini","weight:  145<br />vegetable: zucchini","weight:  164<br />vegetable: zucchini","weight:  175<br />vegetable: zucchini","weight:  252<br />vegetable: zucchini","weight:  344<br />vegetable: zucchini","weight:  371<br />vegetable: zucchini","weight:  393<br />vegetable: zucchini","weight:  427<br />vegetable: zucchini","weight:  443<br />vegetable: zucchini","weight:  445<br />vegetable: zucchini","weight:  457<br />vegetable: zucchini","weight:  492<br />vegetable: zucchini","weight:  660<br />vegetable: zucchini","weight:  731<br />vegetable: zucchini","weight:  834<br />vegetable: zucchini","weight:  859<br />vegetable: zucchini","weight:  920<br />vegetable: zucchini","weight: 1122<br />vegetable: zucchini","weight: 1151<br />vegetable: zucchini","weight: 1175<br />vegetable: zucchini","weight: 1215<br />vegetable: zucchini","weight: 1219<br />vegetable: zucchini","weight: 1300<br />vegetable: zucchini","weight: 1321<br />vegetable: zucchini","weight: 1542<br />vegetable: zucchini","weight: 1774<br />vegetable: zucchini","weight: 2436<br />vegetable: zucchini","weight: 2831<br />vegetable: zucchini","weight: 3244<br />vegetable: zucchini","weight: 3284<br />vegetable: zucchini","weight: 3600<br />vegetable: zucchini","weight: 3800<br />vegetable: zucchini","weight: 5700<br />vegetable: zucchini"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,0,255,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":95.7077625570777},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Weight for vegetable in 2020","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-365.4,7717.4],"tickmode":"array","ticktext":["0","2000","4000","6000"],"tickvals":[0,2000,4000,6000],"categoryorder":"array","categoryarray":["0","2000","4000","6000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Weight in grams","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,31.6],"tickmode":"array","ticktext":["apple","asparagus","basil","beans","beets","broccoli","carrots","chives","cilantro","corn","cucumbers","edamame","hot peppers","jalapeño","kale","kohlrabi","lettuce","onions","peas","peppers","potatoes","pumpkins","radish","raspberries","rutabaga","spinach","squash","strawberries","Swiss chard","tomatoes","zucchini"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31],"categoryorder":"array","categoryarray":["apple","asparagus","basil","beans","beets","broccoli","carrots","chives","cilantro","corn","cucumbers","edamame","hot peppers","jalapeño","kale","kohlrabi","lettuce","onions","peas","peppers","potatoes","pumpkins","radish","raspberries","rutabaga","spinach","squash","strawberries","Swiss chard","tomatoes","zucchini"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"species","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"1cd869222640":{"x":{},"y":{},"type":"scatter"}},"cur_data":"1cd869222640","visdat":{"1cd869222640":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```



```r
garden2 <- garden_harvest %>%
  filter(vegetable == "beets") %>%
  group_by(variety, date) %>%
  summarize(weight_sum = sum(weight, na.rm = TRUE)) %>%
  mutate(weight_ibs = weight_sum * 0.0022, cum_weight = cumsum(weight_ibs)) %>%
  ggplot(aes(x = date, y = cum_weight, color = variety)) +
  labs(x = "date", y = "cumulative weight in ibs",
       title = "Total weight in grams harvests on each day for each variety of beet") +
  geom_line()

ggplotly(garden2)
```

```{=html}
<div id="htmlwidget-ccbe830f550c52c6b618" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-ccbe830f550c52c6b618">{"x":{"data":[{"x":[18450,18451,18463,18470,18487],"y":[0.1364,0.319,0.5544,0.8822,7.007],"text":["date: 2020-07-07<br />cum_weight: 0.1364<br />variety: Gourmet Golden","date: 2020-07-08<br />cum_weight: 0.3190<br />variety: Gourmet Golden","date: 2020-07-20<br />cum_weight: 0.5544<br />variety: Gourmet Golden","date: 2020-07-27<br />cum_weight: 0.8822<br />variety: Gourmet Golden","date: 2020-08-13<br />cum_weight: 7.0070<br />variety: Gourmet Golden"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Gourmet Golden","legendgroup":"Gourmet Golden","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434],"y":[0.0176,0.0726,0.0968,0.2222],"text":["date: 2020-06-11<br />cum_weight: 0.0176<br />variety: leaves","date: 2020-06-18<br />cum_weight: 0.0726<br />variety: leaves","date: 2020-06-19<br />cum_weight: 0.0968<br />variety: leaves","date: 2020-06-21<br />cum_weight: 0.2222<br />variety: leaves"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","name":"leaves","legendgroup":"leaves","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18450,18452,18455,18461,18470,18473,18487],"y":[0.022,0.1738,0.3696,0.748,0.8558,1.078,6.3734],"text":["date: 2020-07-07<br />cum_weight: 0.0220<br />variety: Sweet Merlin","date: 2020-07-09<br />cum_weight: 0.1738<br />variety: Sweet Merlin","date: 2020-07-12<br />cum_weight: 0.3696<br />variety: Sweet Merlin","date: 2020-07-18<br />cum_weight: 0.7480<br />variety: Sweet Merlin","date: 2020-07-27<br />cum_weight: 0.8558<br />variety: Sweet Merlin","date: 2020-07-30<br />cum_weight: 1.0780<br />variety: Sweet Merlin","date: 2020-08-13<br />cum_weight: 6.3734<br />variety: Sweet Merlin"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","name":"Sweet Merlin","legendgroup":"Sweet Merlin","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":31.4155251141553},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total weight in grams harvests on each day for each variety of beet","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18420.85,18490.15],"tickmode":"array","ticktext":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"tickvals":[18428,18444,18458,18475,18489],"categoryorder":"array","categoryarray":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.33187,7.35647],"tickmode":"array","ticktext":["0","2","4","6"],"tickvals":[0,2,4,6],"categoryorder":"array","categoryarray":["0","2","4","6"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"cumulative weight in ibs","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"title":{"text":"variety","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"1cd814dd741d":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"1cd814dd741d","visdat":{"1cd814dd741d":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>%
  filter(year == "2017") %>%
  group_by(arrival_station) %>% 
  mutate(cum_num_arriving_late = cumsum(num_arriving_late)) %>% 
  ungroup() %>%
  ggplot(aes(x = month, 
             y = cum_num_arriving_late, 
             color = delay_cause)) +
  geom_line() +
  transition_reveal(month)  +
  labs(title = "Cumulative arriving late for trains by delay cause in 2017",
       x = "",
       y = "")
```

![](05_exercises_new_files/figure-html/unnamed-chunk-4-1.gif)<!-- -->

```r
anim_save("train_arrive.gif")
```

```r
knitr::include_graphics("train_arrive.gif")
```

![](train_arrive.gif)<!-- -->


## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested weights (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.002) %>% 
  ungroup() %>% 
  complete(variety, 
           date, 
           fill = list(daily_harvest_lb = 0)) %>%
  mutate(variety = fct_reorder(variety, daily_harvest_lb, sum, 
                               .desc = FALSE)) %>%
  group_by(variety) %>% 
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>% 
  ggplot(aes(x = date, 
             y = cum_harvest_lb, 
             fill = variety)) +
  geom_area(position = "stack") +
  geom_text(aes(label = variety),
            position = "stack", 
            check_overlap = TRUE) +
  transition_reveal(date)  +
  labs(title = "Cumulative tomato weight by date",
       x = "",
       y = "")
```

![](05_exercises_new_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("garden_tomato.gif")
```


```r
knitr::include_graphics("garden_tomato.gif")
```

![](garden_tomato.gif)<!-- -->


## Maps, animation, and movement!

  4. Map Lisa's `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.3, bottom = 39.4, right = 3.05, top = 39.7), 
    zoom = 10)

ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
            aes(x = lon, y = lat, color = ele),
            size = 0.9) +
  scale_color_viridis_c(option = "rocket") +
  theme_map() +
  transition_reveal (along = time) + 
  theme(legend.background = element_blank()) +
  labs(title = "Mallorca bike riding", subtitle = "Time: {frame_along}")
```

![](05_exercises_new_files/figure-html/unnamed-chunk-8-1.gif)<!-- -->

```r
anim_save("mallorca_bike.gif")
```


```r
knitr::include_graphics("mallorca_bike.gif")
```

![](mallorca_bike.gif)<!-- -->


  
  5. In this exercise, you get to meet Lisa's sister, Heather! She is a proud Mac grad, currently works as a Data Scientist where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files putting them in swim, bike, run order (HINT: `bind_rows()`), 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama <-bind_rows(panama_swim ,
                   panama_bike ,
                   panama_run)
```


```r
panama_map <- get_stamenmap(
    bbox = c(left = min(panama$lon), 
             bottom = min(panama$lat), 
             right = max(panama$lon), 
             top = max(panama$lat)), 
    maptype = "terrain",
    zoom = 13
)
```



```r
ggmap(panama_map) +
  geom_path(data = panama,
            aes(x = lon, 
                y = lat)) +
  geom_point(data = panama,
             aes(x = lon, 
                 y = lat,
                 color = event), 
            show.legend = FALSE) +   
  scale_color_viridis_d(option = "rocket") +
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_reveal(along = time) +
  labs(title = "Heather's Pan Am Triathlon")
```

![](05_exercises_new_files/figure-html/unnamed-chunk-12-1.gif)<!-- -->

```r
anim_save("panama.gif")
```


```r
knitr::include_graphics("panama.gif")
```

![](panama.gif)<!-- -->


## COVID-19 data

  6. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for the the 15th of each month. So, filter only to those dates - there are some lubridate functions that can help you do this.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

covid_animation <- covid19 %>% 
  mutate(state = str_to_lower(state)) %>%
  separate(date, into = c("year", "month", "day")) %>% 
  filter(day == "15") %>% 
  mutate(date = make_date(year, month, day)) %>%
  group_by(date, state) %>%
  summarise(case_cum = sum(cases)) %>%
  left_join(census_pop_est_2018,
            by = "state") %>%
  mutate(cum_cases_per_10000 = (case_cum/est_pop_2018)*10000) %>%
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cum_cases_per_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  scale_fill_distiller(palette = "Spectral", 
                       direction = -1,
                       labels = scales::comma_format()) +
  labs(title = "COVID-19 Cases per 10,000 Residents",
       subtitle = "Date: {closest_state}",
       fill = "") +
  theme_map() +
  theme(legend.background = element_blank(),
        plot.title = element_text(hjust = 0.5, face = "bold", size = 14)) +
  transition_states(date, transition_length = 0)

animate(covid_animation, nframes = 200, end_pause = 10)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-14-1.gif)<!-- -->

```r
anim_save("covid_animation.gif")
```


```r
knitr::include_graphics("covid_animation.gif")
```

![](covid_animation.gif)<!-- -->



## Your first `shiny` app (for next week!)

  7. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. You should create a new project for the app, separate from the homework project. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' daily number of COVID cases per 100,000 over time. The x-axis will be date. You will have an input box where the user can choose which states to compare (`selectInput()`), a slider where the user can choose the date range, and a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
Put the link to your app here: 

https://wx-zhu.shinyapps.io/comp_112_hw_5_shiny/

  
## GitHub link

  8. Below, provide a link to your GitHub repo with this set of Weekly Exercises. 


https://github.com/wx-zhu/comp_112_hw_5.git



**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**

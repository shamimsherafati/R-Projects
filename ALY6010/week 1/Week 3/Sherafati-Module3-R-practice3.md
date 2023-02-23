---
title: 'ALY6010: R Practice 3'
subtitle: 'Module 3'
author: "Shamim Sherafati" 
date: "2022-11-20"
output:
  html_document: 
    toc: yes
    keep_md: yes
  pdf_document: default
---

### Importing packages

```r
#install.packages("plyr")
#install.packages("dplyr")
#install.packages("tidyr")
#install.packages("tidyverse")
#install.packages("psych")
#install.packages("ggpubr")
#install.packages("ggplot2")
#install.packages("plotly")
#install.packages("moments")
#install.packages('gmodels')
#install.packages("stringr")


library(plyr) 
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:plyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(tidyr)
library(tidyverse)
```

```
## ── Attaching packages
## ───────────────────────────────────────
## tidyverse 1.3.2 ──
```

```
## ✔ ggplot2 3.4.0     ✔ purrr   0.3.5
## ✔ tibble  3.1.8     ✔ stringr 1.4.1
## ✔ readr   2.1.3     ✔ forcats 0.5.2
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::arrange()   masks plyr::arrange()
## ✖ purrr::compact()   masks plyr::compact()
## ✖ dplyr::count()     masks plyr::count()
## ✖ dplyr::failwith()  masks plyr::failwith()
## ✖ dplyr::filter()    masks stats::filter()
## ✖ dplyr::id()        masks plyr::id()
## ✖ dplyr::lag()       masks stats::lag()
## ✖ dplyr::mutate()    masks plyr::mutate()
## ✖ dplyr::rename()    masks plyr::rename()
## ✖ dplyr::summarise() masks plyr::summarise()
## ✖ dplyr::summarize() masks plyr::summarize()
```

```r
library(psych)
```

```
## 
## Attaching package: 'psych'
## 
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

```r
library(ggpubr)
```

```
## 
## Attaching package: 'ggpubr'
## 
## The following object is masked from 'package:plyr':
## 
##     mutate
```

```r
library(ggplot2)
library(plotly)
```

```
## 
## Attaching package: 'plotly'
## 
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
## 
## The following objects are masked from 'package:plyr':
## 
##     arrange, mutate, rename, summarise
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
library(moments)
library(gmodels)
library(stringr)
```

### Importing data

```r
FoodSurvey = read.csv("~/Documents/Northeastern University/ALY 6010/dataset/Food Survey - Data.csv", header = TRUE)
FoodSurvey
```

```
##               Timestamp     Gender Boarding Grade Athlete
## 1   10/22/2020 11:36:55     Female      Day  12th      No
## 2   10/22/2020 11:36:58       Male Boarding  10th     Yes
## 3   10/22/2020 11:36:59     Female      Day   9th     Yes
## 4   10/22/2020 11:37:00       Male      Day  11th      No
## 5   10/22/2020 11:37:01     Female      Day  12th      No
## 6   10/22/2020 11:37:10       Male Boarding  11th     Yes
## 7   10/22/2020 11:37:24       Male Boarding  10th     Yes
## 8   10/22/2020 11:37:29     Female      Day  11th     Yes
## 9   10/22/2020 11:37:37     Female Boarding  12th     Yes
## 10  10/22/2020 11:37:40     Female Boarding  12th     Yes
## 11  10/22/2020 11:37:42       Male      Day   9th     Yes
## 12  10/22/2020 11:37:50       Male Boarding  10th      No
## 13  10/22/2020 11:37:59       Male      Day   9th     Yes
## 14  10/22/2020 11:38:10 Non-Binary      Day  12th     Yes
## 15  10/22/2020 11:38:14       Male Boarding  10th     Yes
## 16  10/22/2020 11:38:18     Female Boarding  12th      No
## 17  10/22/2020 11:38:20     Female Boarding  12th     Yes
## 18  10/22/2020 11:38:24     Female Boarding  10th     Yes
## 19  10/22/2020 11:38:26       Male Boarding  11th      No
## 20  10/22/2020 11:38:28     Female Boarding  12th      No
## 21  10/22/2020 11:38:36     Female Boarding  10th     Yes
## 22  10/22/2020 11:38:48     Female Boarding   9th     Yes
## 23  10/22/2020 11:38:52       Male      Day  12th     Yes
## 24  10/22/2020 11:38:56       Male      Day   9th     Yes
## 25  10/22/2020 11:39:00     Female Boarding  10th     Yes
## 26  10/22/2020 11:39:01     Female Boarding  11th      No
## 27  10/22/2020 11:39:07     Female Boarding  11th      No
## 28  10/22/2020 11:39:17     Female Boarding  12th     Yes
## 29  10/22/2020 11:39:26       Male Boarding  12th     Yes
## 30  10/22/2020 11:39:32       Male Boarding  11th     Yes
## 31  10/22/2020 11:39:37       Male Boarding  11th     Yes
## 32  10/22/2020 11:39:55     Female      Day  12th      No
## 33  10/22/2020 11:40:04       Male Boarding  10th     Yes
## 34  10/22/2020 11:40:10       Male Boarding  12th     Yes
## 35  10/22/2020 11:40:34     Female      Day  11th     Yes
## 36  10/22/2020 11:41:26     Female      Day   9th     Yes
## 37  10/22/2020 11:41:28     Female Boarding  11th      No
## 38  10/22/2020 11:41:34       Male      Day   9th     Yes
## 39  10/22/2020 11:42:05     Female Boarding  10th     Yes
## 40  10/22/2020 11:44:12     Female Boarding  12th     Yes
## 41  10/22/2020 11:44:48       Male      Day  10th     Yes
## 42  10/22/2020 11:44:59       Male      Day  10th     Yes
## 43  10/22/2020 11:45:08     Female Boarding   9th     Yes
## 44  10/22/2020 11:45:19       Male Boarding  11th      No
## 45  10/22/2020 11:45:35       Male Boarding  11th     Yes
## 46  10/22/2020 11:45:41       Male      Day           Yes
## 47  10/22/2020 11:45:56       Male Boarding  12th     Yes
## 48  10/22/2020 11:46:13       Male Boarding  11th     Yes
## 49  10/22/2020 11:47:19     Female Boarding   9th      No
## 50  10/22/2020 11:47:27     Female Boarding  11th     Yes
## 51  10/22/2020 11:47:35       Male Boarding  12th     Yes
## 52  10/22/2020 11:47:56     Female Boarding  11th     Yes
## 53  10/22/2020 11:48:41     Female      Day   9th     Yes
## 54  10/22/2020 11:48:44     Female Boarding   9th     Yes
## 55  10/22/2020 11:49:03     Female      Day  12th      No
## 56  10/22/2020 11:49:39       Male Boarding  12th     Yes
## 57  10/22/2020 11:49:46     Female      Day  12th      No
## 58  10/22/2020 11:49:50     Female      Day   9th     Yes
## 59  10/22/2020 11:50:07     Female Boarding  10th     Yes
## 60  10/22/2020 11:50:12       Male Boarding  11th     Yes
## 61  10/22/2020 11:50:14       Male Boarding  11th     Yes
## 62  10/22/2020 11:50:31     Female Boarding   9th      No
## 63  10/22/2020 11:51:46       Male Boarding  12th     Yes
## 64  10/22/2020 11:52:17       Male Boarding   9th     Yes
## 65  10/22/2020 11:52:18       Male Boarding   9th     Yes
## 66  10/22/2020 11:52:30     Female Boarding  11th      No
## 67  10/22/2020 11:52:45     Female      Day  12th     Yes
## 68  10/22/2020 11:56:12     Female Boarding  12th     Yes
## 69  10/22/2020 11:56:16       Male Boarding  11th     Yes
## 70  10/22/2020 11:57:49     Female      Day  10th      No
## 71  10/22/2020 11:58:28     Female Boarding   9th      No
## 72  10/22/2020 11:58:40       Male      Day  11th     Yes
## 73  10/22/2020 11:59:03     Female      Day  12th     Yes
## 74  10/22/2020 11:59:26       Male Boarding   9th     Yes
## 75  10/22/2020 11:59:43       Male Boarding  10th     Yes
## 76  10/22/2020 11:59:45       Male      Day  10th     Yes
## 77  10/22/2020 12:01:43     Female      Day   9th     Yes
## 78  10/22/2020 12:02:20       Male Boarding  12th     Yes
## 79  10/22/2020 12:03:22     Female      Day  10th      No
## 80  10/22/2020 12:05:13       Male      Day  10th     Yes
## 81  10/22/2020 12:10:45     Female Boarding  10th      No
## 82  10/22/2020 12:12:46       Male Boarding  10th     Yes
## 83  10/22/2020 12:14:09       Male Boarding  10th     Yes
## 84  10/22/2020 12:15:43       Male Boarding  11th     Yes
## 85  10/22/2020 12:17:08     Female Boarding  10th     Yes
## 86  10/22/2020 12:18:18     Female Boarding  10th     Yes
## 87  10/22/2020 12:23:53 Non-Binary      Day   9th      No
## 88  10/22/2020 12:26:07       Male Boarding  10th     Yes
## 89  10/22/2020 12:26:12     Female      Day  11th      No
## 90  10/22/2020 12:27:29       Male      Day  11th     Yes
## 91  10/22/2020 12:28:17       Male Boarding  12th      No
## 92  10/22/2020 12:29:39     Female Boarding  11th     Yes
## 93  10/22/2020 12:30:15     Female Boarding   9th     Yes
## 94  10/22/2020 12:35:22       Male Boarding  10th     Yes
## 95  10/22/2020 12:35:23       Male Boarding  11th     Yes
## 96  10/22/2020 12:36:17       Male      Day  12th      No
## 97  10/22/2020 12:38:38       Male      Day  11th      No
## 98  10/22/2020 12:39:36     Female Boarding  10th      No
## 99  10/22/2020 12:39:42     Female      Day   9th     Yes
## 100 10/22/2020 12:40:30       Male Boarding  11th      No
## 101 10/22/2020 12:41:01       Male Boarding  10th     Yes
## 102 10/22/2020 12:41:06     Female      Day  11th      No
## 103 10/22/2020 12:42:20       Male Boarding  11th     Yes
## 104 10/22/2020 12:44:45       Male Boarding  11th     Yes
## 105 10/22/2020 12:45:57       Male Boarding  12th     Yes
## 106 10/22/2020 12:47:51       Male Boarding  11th     Yes
## 107 10/22/2020 12:51:44       Male Boarding  11th     Yes
## 108 10/22/2020 12:57:00     Female Boarding   9th     Yes
## 109 10/22/2020 12:59:40       Male      Day  10th     Yes
## 110 10/22/2020 12:59:46       Male Boarding  12th     Yes
## 111 10/22/2020 13:01:03       Male Boarding  12th     Yes
## 112 10/22/2020 13:02:57     Female Boarding  11th     Yes
## 113 10/22/2020 13:04:22       Male Boarding  11th      No
## 114 10/22/2020 13:04:34     Female Boarding  10th      No
## 115 10/22/2020 13:11:39       Male Boarding  11th     Yes
## 116 10/22/2020 13:14:42       Male Boarding  12th      No
## 117 10/22/2020 13:40:43       Male Boarding  11th     Yes
## 118 10/22/2020 13:41:33       Male      Day  10th     Yes
## 119 10/22/2020 13:50:26     Female Boarding  11th      No
## 120 10/22/2020 13:50:58       Male Boarding  10th     Yes
## 121 10/22/2020 13:55:51     Female      Day  11th     Yes
## 122 10/22/2020 13:57:21     Female      Day   9th     Yes
## 123 10/22/2020 13:57:24       Male Boarding  12th     Yes
## 124 10/22/2020 13:59:24       Male Boarding  12th     Yes
## 125 10/22/2020 13:59:39       Male Boarding  10th     Yes
## 126 10/22/2020 14:00:37     Female Boarding  12th      No
## 127 10/22/2020 14:06:16       Male Boarding  10th     Yes
## 128 10/22/2020 14:14:03     Female Boarding  11th     Yes
## 129 10/22/2020 14:15:50       Male Boarding  11th     Yes
## 130 10/22/2020 14:18:55       Male      Day   9th      No
## 131 10/22/2020 14:21:35       Male Boarding  10th     Yes
## 132 10/22/2020 14:26:26       Male      Day  11th     Yes
## 133 10/22/2020 14:27:20     Female      Day  10th     Yes
## 134 10/22/2020 14:27:58       Male      Day  10th     Yes
## 135 10/22/2020 14:28:08     Female Boarding  12th     Yes
## 136 10/22/2020 14:30:57       Male Boarding  10th     Yes
## 137 10/22/2020 14:31:50       Male Boarding  11th     Yes
## 138 10/22/2020 14:56:43      Other      Day   9th     Yes
## 139 10/22/2020 15:00:32     Female Boarding   9th     Yes
## 140 10/22/2020 15:18:20     Female      Day   9th     Yes
## 141 10/22/2020 15:49:45     Female Boarding  11th     Yes
## 142 10/22/2020 16:01:32       Male Boarding  10th     Yes
## 143 10/22/2020 17:24:39       Male      Day  11th      No
## 144 10/22/2020 17:34:11     Female Boarding  10th      No
## 145 10/22/2020 17:47:59     Female      Day  10th     Yes
## 146 10/22/2020 18:09:12       Male      Day  12th      No
## 147 10/22/2020 18:23:39       Male Boarding  11th     Yes
## 148 10/22/2020 18:28:51       Male      Day  12th     Yes
## 149 10/22/2020 19:34:32     Female Boarding  10th     Yes
## 150 10/22/2020 19:46:38     Female Boarding  10th     Yes
## 151 10/22/2020 19:50:51       Male Boarding   9th     Yes
## 152 10/22/2020 19:57:47       Male      Day  10th     Yes
## 153 10/22/2020 20:40:06       Male Boarding  11th     Yes
## 154 10/22/2020 21:01:52     Female Boarding  10th     Yes
## 155 10/22/2020 21:07:53     Female      Day  10th     Yes
## 156 10/22/2020 21:31:12       Male      Day  11th     Yes
## 157 10/22/2020 21:35:17       Male Boarding  10th     Yes
## 158 10/22/2020 21:39:38       Male Boarding   9th        
## 159 10/22/2020 21:39:52       Male Boarding  12th     Yes
## 160  10/23/2020 5:52:55 Non-Binary      Day   9th      No
## 161  10/23/2020 8:02:17       Male      Day   9th      No
## 162  10/23/2020 8:51:19     Female      Day   9th     Yes
## 163 10/23/2020 10:39:15       Male Boarding  12th      No
## 164 10/23/2020 12:34:25     Female      Day  11th      No
## 165 10/23/2020 22:51:15       Male      Day   9th      No
## 166 10/24/2020 17:39:51     Female      Day  10th      No
## 167 10/25/2020 16:01:48       Male      Day   9th     Yes
##                  Activities DHBreakfast NDHBreakfast BClass DHBoxes NDHBoxes
## 1         None of the above           0            2     No       2        0
## 2                    Soccer           5            2     No       2        0
## 3                    Tennis           3            7    Yes       2        0
## 4         None of the above           0            4     No       2        0
## 5                   Musical           0            5    Yes       1        0
## 6                    Soccer           6            1    Yes       2        1
## 7   Pre-season conditioning           7            0    Yes       3        1
## 8                   Musical           0            7     No       1        0
## 9                    Tennis           2            0    Yes       2        0
## 10  Pre-season conditioning           4            3    Yes      NA       NA
## 11        None of the above           0            6     No       1        0
## 12        None of the above           7            0     No       2        0
## 13                   Soccer           0            7     No       3        0
## 14               Volleyball           0            4    Yes       2        0
## 15                 Football           5            2    Yes       2        1
## 16                  Musical           1            2    Yes       2        0
## 17               Volleyball           6            0    Yes       2        0
## 18               Volleyball           2            5     No       2        0
## 19                  Musical           6            1    Yes       2        1
## 20        None of the above           0            3    Yes       4        2
## 21  Pre-season conditioning           7            7    Yes       2        0
## 22        None of the above           2            1    Yes       2        0
## 23                   Soccer           0            7     No       4        0
## 24              Intramurals           0            7    Yes       3        0
## 25            Cross Country           2            1    Yes       2        1
## 26        None of the above           2            2     No       1        0
## 27                   Tennis           1            0    Yes       1        0
## 28        None of the above           1            7    Yes       2        1
## 29                  Musical           6            1    Yes       2        0
## 30                 Football           7            0    Yes       6        2
## 31                 Football           2            0    Yes       2        0
## 32        None of the above           6            7     No       3        0
## 33                 Football           7            3    Yes       5        2
## 34                   Soccer           7            0    Yes       3        0
## 35  Pre-season conditioning           0            1    Yes       1        0
## 36                   Tennis           1            6    Yes       1        0
## 37        None of the above           2            0     No       2        1
## 38              Intramurals           0            0    Yes       3        0
## 39            Cross Country           1            2    Yes       1        0
## 40                   Tennis           5            0    Yes       1        1
## 41                 Football           0            6    Yes       4        0
## 42                   Soccer           0            7     No       0        0
## 43        None of the above           7            0    Yes       1        0
## 44        None of the above           7            0     No       2        0
## 45              Intramurals           7            0     No       2        1
## 46                 Football           0            7    Yes       5       NA
## 47                 Football           2            2     No       1        0
## 48                   Soccer           3            6    Yes       5        1
## 49        None of the above           1            1    Yes       1        1
## 50                   Soccer           6            0     No       1        0
## 51                     Golf           7            7     No       9       10
## 52        None of the above           7            0     No       1        0
## 53                  Musical           1            6     No       1        0
## 54               Volleyball           7            0    Yes       2        0
## 55                  Musical           5            0     No       2        0
## 56                 Football           2            0    Yes       2        1
## 57        None of the above           5            2     No       1        0
## 58                   Tennis           7            7     No       1        0
## 59  Pre-season conditioning           5            2    Yes       4        1
## 60                 Football           6            1    Yes       4        2
## 61                 Football           7            2     No       3        1
## 62                  Musical           2            4    Yes       2        2
## 63  Pre-season conditioning           2            2    Yes       4        1
## 64                 Football           7            0    Yes       4        1
## 65            Cross Country           5            7    Yes       3        0
## 66        None of the above           5            2    Yes       1        0
## 67        None of the above           0            3     No       2        0
## 68               Volleyball           4            3    Yes       2        0
## 69                 Football           5            0    Yes       3        1
## 70                  Musical           0            1    Yes       1        0
## 71        None of the above           3            4    Yes       1        0
## 72        None of the above           0            7     No       3        0
## 73               Volleyball           2            5    Yes       2        0
## 74  Pre-season conditioning           4            0    Yes       3        2
## 75  Pre-season conditioning           6            0     No       4        0
## 76                     Golf           0            6    Yes       2        0
## 77            Cross Country           0            7     No       1        0
## 78                   Soccer           3            2    Yes       6        2
## 79        None of the above           3            5    Yes       2        0
## 80                 Football           1            2    Yes       3        0
## 81                  Musical           3            4    Yes       2        0
## 82                   Soccer           5            0    Yes       5        1
## 83                 Football           7            0    Yes       4        1
## 84                   Soccer           7            0    Yes       2        0
## 85        None of the above           2            3    Yes       2        0
## 86               Volleyball           1            0    Yes       2        0
## 87                  Musical           0            7     No       1        0
## 88  Pre-season conditioning           7            0    Yes       2        0
## 89                  Musical           0            1    Yes       1        1
## 90                 Football           0            7    Yes       2        0
## 91        None of the above           7            1    Yes       1        1
## 92                     Golf           7            0    Yes       2        1
## 93                  Musical           6            1     No       2        0
## 94                   Soccer           4            1     No       2        1
## 95            Cross Country           7            6    Yes       3        1
## 96        None of the above           0            0    Yes       1        0
## 97                  Musical           0            5    Yes       3        0
## 98                   Tennis           7            6     No       2        1
## 99               Volleyball           0            7     No       0        0
## 100                 Musical           0            0    Yes       2        1
## 101                Football           7            0    Yes       3        0
## 102                 Musical           0            0     No       2        0
## 103                Football           7            7     No       6        4
## 104 Pre-season conditioning           7            0    Yes       3        2
## 105                 Musical           2            2     No       3        0
## 106                Football           7            0     No       4        2
## 107                  Soccer           0            0    Yes       2        1
## 108           Cross Country           7            0     No       2        0
## 109             Intramurals           0            4    Yes       3        0
## 110 Pre-season conditioning           6            1     No       2        1
## 111                  Soccer           2            0    Yes       2        0
## 112                Football           5            7    Yes       2        0
## 113                 Musical           2            0     No       1        1
## 114                                   0            0     No       2        0
## 115             Intramurals           2            0    Yes       2        1
## 116       None of the above           2            0    Yes       3        1
## 117                Football           7            0     No       5        2
## 118       None of the above           0            7    Yes       4        0
## 119                    Golf           2            5     No       1        0
## 120                  Soccer           2            0    Yes       2        0
## 121                    Golf           4            3    Yes       2        1
## 122           Cross Country           0            7     No       1        0
## 123                Football           2            0     No       4        1
## 124                  Soccer           2            5     No       3        0
## 125                  Soccer           4            0    Yes       2        1
## 126           Cross Country           2            0    Yes       2        2
## 127 Pre-season conditioning           5            1    Yes       2        2
## 128           Cross Country           7            0     No       2        2
## 129                Football           7            7     No       2        0
## 130       None of the above           0            3    Yes       2        0
## 131       None of the above           7            0    Yes       3        2
## 132                Football           7            7    Yes       9        5
## 133       None of the above           5            0     No       1        0
## 134 Pre-season conditioning           6            1     No       5        1
## 135                    Golf           3            1    Yes       2        0
## 136                Football           4            3    Yes       4        1
## 137                  Soccer           7            0     No       3        0
## 138                  Soccer           3            4    Yes       2        0
## 139              Volleyball           4            0    Yes       2        1
## 140              Volleyball           0            6    Yes       1        0
## 141 Pre-season conditioning           7            0     No       2        0
## 142             Intramurals           6            5     No       3        0
## 143       None of the above           0            0     No       3        1
## 144       None of the above           7            2     No      NA       NA
## 145 Pre-season conditioning           0            3    Yes       2        0
## 146                 Musical           0            5     No       2        5
## 147             Intramurals           7            0    Yes       4        3
## 148       None of the above           0            1     No       5        0
## 149 Pre-season conditioning           0            0    Yes       1        0
## 150                 Musical           2            0     No       2        1
## 151       None of the above           4            3    Yes       2        0
## 152       None of the above           0            7     No       4        0
## 153                  Soccer           1            0    Yes       2        1
## 154              Volleyball           2            5    Yes       2        0
## 155              Volleyball           2            4     No       1        0
## 156                Football           1            6    Yes       3        1
## 157                  Soccer           6            2     No       2        1
## 158                Football           4            0    Yes       2        1
## 159                  Soccer           7            1     No       4        2
## 160                 Musical           0            0    Yes       1        0
## 161                 Musical           0            7     No       2        0
## 162                  Soccer           0            2    Yes       2        0
## 163       None of the above           2            0    Yes       2        1
## 164       None of the above           0            7     No       2        0
## 165           Cross Country           0            0     No       0        0
## 166 Pre-season conditioning           0            2    Yes       1        0
## 167           Cross Country           0            7     No       2        0
##     NDHMeals Nutrition Money
## 1          0         0  40.0
## 2          5         4  15.0
## 3         10         4  50.0
## 4          1         2   4.0
## 5          2         3  20.0
## 6          2         2    NA
## 7          2         3    NA
## 8          0         2    NA
## 9          4         2   5.0
## 10         4         3  50.0
## 11         0         4  25.0
## 12         2         1    NA
## 13         0         3  12.0
## 14         2         4    NA
## 15         3         4  10.0
## 16         1         2  12.0
## 17         2         3  20.0
## 18         5         5   5.0
## 19         1         4    NA
## 20        10         1  30.0
## 21         6         3    NA
## 22         4         3   8.0
## 23         0         5  30.0
## 24         3         3   3.0
## 25         2         4   3.0
## 26         5         2  30.0
## 27         1         0  45.0
## 28         9         4  20.0
## 29         6         4  10.0
## 30         5         4  12.0
## 31        10         3  20.0
## 32         0         5  30.0
## 33         6         2    NA
## 34         5         0  34.0
## 35         3         2  12.5
## 36        10         0  20.0
## 37         2         5  10.0
## 38         2         3   7.0
## 39         4         4  15.0
## 40         5         4   0.0
## 41         1         3  30.0
## 42         1         4    NA
## 43         4         3   7.0
## 44         0         4  10.0
## 45         4         3   5.0
## 46         4         4  10.0
## 47         3         3  28.0
## 48        10         5  10.0
## 49         3         2  50.0
## 50         3         0  17.5
## 51        10         5    NA
## 52         7         1 100.0
## 53         0         4  12.0
## 54         0         1  15.0
## 55         1         1  15.0
## 56         3         0  10.0
## 57         0         3  25.0
## 58         0         4    NA
## 59         4         0  25.0
## 60         8         5  20.0
## 61         2         4  15.0
## 62         7         2  10.0
## 63         5         4   7.0
## 64         3         3   7.0
## 65         4         4  15.0
## 66         2         3  20.0
## 67         0         2  20.0
## 68         4         1  40.0
## 69         4         0  10.0
## 70         2         0    NA
## 71         7         4  30.0
## 72         0         4   2.0
## 73         0         2  50.0
## 74        10         3  50.0
## 75         1         5   2.0
## 76         0         3  60.0
## 77         0         4    NA
## 78         3         3  20.0
## 79         0         1   2.0
## 80         0         4  12.0
## 81         5         1  20.0
## 82         3         5   5.0
## 83         3         3  15.0
## 84         2         5  20.0
## 85         3         4  30.0
## 86         1         0   3.0
## 87         0         2    NA
## 88         0         2   2.0
## 89         2         4  15.0
## 90         5         4  20.0
## 91         8         0   0.0
## 92         3         1  20.0
## 93         3         4   8.0
## 94         8         3  10.0
## 95         3         2   5.0
## 96         1         4   5.0
## 97         2         4  50.0
## 98         2        NA  20.0
## 99         0         3   3.0
## 100        5         3  15.0
## 101        1         3   5.0
## 102        1         1    NA
## 103        7         5    NA
## 104        0         0  25.0
## 105        5         4    NA
## 106       10         3   5.0
## 107        2         3    NA
## 108        0         5  30.0
## 109        0         0  50.0
## 110        2         4  15.0
## 111        0         0   5.0
## 112       10         0  40.0
## 113        7         3   0.0
## 114        6         0   7.0
## 115       10         0  20.0
## 116        2         4  20.0
## 117        6         5  15.0
## 118        0         2  10.0
## 119        5         5   1.5
## 120        0         1   5.0
## 121        4         1  30.0
## 122        3         2   3.0
## 123        5         5   5.0
## 124        0         5  40.0
## 125        4         0  50.0
## 126        3         2   5.0
## 127        1         4   3.0
## 128        0         3   0.0
## 129        3         5    NA
## 130        1         0  20.0
## 131        1         5    NA
## 132       10         5    NA
## 133        0         3   5.0
## 134        1         3  20.0
## 135        0         3  20.0
## 136        8         3  30.0
## 137        7         3  20.0
## 138        5         1    NA
## 139        2         0   7.0
## 140        4         3   6.0
## 141        4         0  15.0
## 142        6         5  25.0
## 143        0         2    NA
## 144        2         2  20.0
## 145        3         4  45.0
## 146        0         3   0.0
## 147        2         4  15.0
## 148        0         0    NA
## 149        0         1   2.0
## 150        2         3   5.0
## 151        2         1   3.0
## 152        0         4   0.0
## 153        4         3  20.0
## 154        5         5  20.0
## 155        0         5    NA
## 156        1         3  40.0
## 157        5         3  30.0
## 158        6         3   5.5
## 159        9         4  10.0
## 160        0         1  20.0
## 161        1         4    NA
## 162        2         0   0.0
## 163        5         1  25.0
## 164        0         3   0.0
## 165        0         0   1.0
## 166        0         4   0.0
## 167        0         3   3.0
```
### Initial Summary of Data set

```r
summary(FoodSurvey) #Initial summary
```

```
##   Timestamp            Gender            Boarding            Grade          
##  Length:167         Length:167         Length:167         Length:167        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##    Athlete           Activities         DHBreakfast    NDHBreakfast  
##  Length:167         Length:167         Min.   :0.00   Min.   :0.000  
##  Class :character   Class :character   1st Qu.:0.00   1st Qu.:0.000  
##  Mode  :character   Mode  :character   Median :2.00   Median :2.000  
##                                        Mean   :3.18   Mean   :2.563  
##                                        3rd Qu.:6.00   3rd Qu.:5.000  
##                                        Max.   :7.00   Max.   :7.000  
##                                                                      
##     BClass             DHBoxes         NDHBoxes          NDHMeals     
##  Length:167         Min.   :0.000   Min.   : 0.0000   Min.   : 0.000  
##  Class :character   1st Qu.:2.000   1st Qu.: 0.0000   1st Qu.: 0.500  
##  Mode  :character   Median :2.000   Median : 0.0000   Median : 2.000  
##                     Mean   :2.388   Mean   : 0.6159   Mean   : 3.102  
##                     3rd Qu.:3.000   3rd Qu.: 1.0000   3rd Qu.: 5.000  
##                     Max.   :9.000   Max.   :10.0000   Max.   :10.000  
##                     NA's   :2       NA's   :3                         
##    Nutrition         Money       
##  Min.   :0.000   Min.   :  0.00  
##  1st Qu.:2.000   1st Qu.:  5.00  
##  Median :3.000   Median : 15.00  
##  Mean   :2.747   Mean   : 17.43  
##  3rd Qu.:4.000   3rd Qu.: 25.00  
##  Max.   :5.000   Max.   :100.00  
##  NA's   :1       NA's   :26
```
### Data Frame

```r
df_foodsurvey <- as.data.frame(read.csv("~/Documents/Northeastern University/ALY 6010/dataset/Food Survey - Data.csv", header = TRUE))
df_foodsurvey
```

```
##               Timestamp     Gender Boarding Grade Athlete
## 1   10/22/2020 11:36:55     Female      Day  12th      No
## 2   10/22/2020 11:36:58       Male Boarding  10th     Yes
## 3   10/22/2020 11:36:59     Female      Day   9th     Yes
## 4   10/22/2020 11:37:00       Male      Day  11th      No
## 5   10/22/2020 11:37:01     Female      Day  12th      No
## 6   10/22/2020 11:37:10       Male Boarding  11th     Yes
## 7   10/22/2020 11:37:24       Male Boarding  10th     Yes
## 8   10/22/2020 11:37:29     Female      Day  11th     Yes
## 9   10/22/2020 11:37:37     Female Boarding  12th     Yes
## 10  10/22/2020 11:37:40     Female Boarding  12th     Yes
## 11  10/22/2020 11:37:42       Male      Day   9th     Yes
## 12  10/22/2020 11:37:50       Male Boarding  10th      No
## 13  10/22/2020 11:37:59       Male      Day   9th     Yes
## 14  10/22/2020 11:38:10 Non-Binary      Day  12th     Yes
## 15  10/22/2020 11:38:14       Male Boarding  10th     Yes
## 16  10/22/2020 11:38:18     Female Boarding  12th      No
## 17  10/22/2020 11:38:20     Female Boarding  12th     Yes
## 18  10/22/2020 11:38:24     Female Boarding  10th     Yes
## 19  10/22/2020 11:38:26       Male Boarding  11th      No
## 20  10/22/2020 11:38:28     Female Boarding  12th      No
## 21  10/22/2020 11:38:36     Female Boarding  10th     Yes
## 22  10/22/2020 11:38:48     Female Boarding   9th     Yes
## 23  10/22/2020 11:38:52       Male      Day  12th     Yes
## 24  10/22/2020 11:38:56       Male      Day   9th     Yes
## 25  10/22/2020 11:39:00     Female Boarding  10th     Yes
## 26  10/22/2020 11:39:01     Female Boarding  11th      No
## 27  10/22/2020 11:39:07     Female Boarding  11th      No
## 28  10/22/2020 11:39:17     Female Boarding  12th     Yes
## 29  10/22/2020 11:39:26       Male Boarding  12th     Yes
## 30  10/22/2020 11:39:32       Male Boarding  11th     Yes
## 31  10/22/2020 11:39:37       Male Boarding  11th     Yes
## 32  10/22/2020 11:39:55     Female      Day  12th      No
## 33  10/22/2020 11:40:04       Male Boarding  10th     Yes
## 34  10/22/2020 11:40:10       Male Boarding  12th     Yes
## 35  10/22/2020 11:40:34     Female      Day  11th     Yes
## 36  10/22/2020 11:41:26     Female      Day   9th     Yes
## 37  10/22/2020 11:41:28     Female Boarding  11th      No
## 38  10/22/2020 11:41:34       Male      Day   9th     Yes
## 39  10/22/2020 11:42:05     Female Boarding  10th     Yes
## 40  10/22/2020 11:44:12     Female Boarding  12th     Yes
## 41  10/22/2020 11:44:48       Male      Day  10th     Yes
## 42  10/22/2020 11:44:59       Male      Day  10th     Yes
## 43  10/22/2020 11:45:08     Female Boarding   9th     Yes
## 44  10/22/2020 11:45:19       Male Boarding  11th      No
## 45  10/22/2020 11:45:35       Male Boarding  11th     Yes
## 46  10/22/2020 11:45:41       Male      Day           Yes
## 47  10/22/2020 11:45:56       Male Boarding  12th     Yes
## 48  10/22/2020 11:46:13       Male Boarding  11th     Yes
## 49  10/22/2020 11:47:19     Female Boarding   9th      No
## 50  10/22/2020 11:47:27     Female Boarding  11th     Yes
## 51  10/22/2020 11:47:35       Male Boarding  12th     Yes
## 52  10/22/2020 11:47:56     Female Boarding  11th     Yes
## 53  10/22/2020 11:48:41     Female      Day   9th     Yes
## 54  10/22/2020 11:48:44     Female Boarding   9th     Yes
## 55  10/22/2020 11:49:03     Female      Day  12th      No
## 56  10/22/2020 11:49:39       Male Boarding  12th     Yes
## 57  10/22/2020 11:49:46     Female      Day  12th      No
## 58  10/22/2020 11:49:50     Female      Day   9th     Yes
## 59  10/22/2020 11:50:07     Female Boarding  10th     Yes
## 60  10/22/2020 11:50:12       Male Boarding  11th     Yes
## 61  10/22/2020 11:50:14       Male Boarding  11th     Yes
## 62  10/22/2020 11:50:31     Female Boarding   9th      No
## 63  10/22/2020 11:51:46       Male Boarding  12th     Yes
## 64  10/22/2020 11:52:17       Male Boarding   9th     Yes
## 65  10/22/2020 11:52:18       Male Boarding   9th     Yes
## 66  10/22/2020 11:52:30     Female Boarding  11th      No
## 67  10/22/2020 11:52:45     Female      Day  12th     Yes
## 68  10/22/2020 11:56:12     Female Boarding  12th     Yes
## 69  10/22/2020 11:56:16       Male Boarding  11th     Yes
## 70  10/22/2020 11:57:49     Female      Day  10th      No
## 71  10/22/2020 11:58:28     Female Boarding   9th      No
## 72  10/22/2020 11:58:40       Male      Day  11th     Yes
## 73  10/22/2020 11:59:03     Female      Day  12th     Yes
## 74  10/22/2020 11:59:26       Male Boarding   9th     Yes
## 75  10/22/2020 11:59:43       Male Boarding  10th     Yes
## 76  10/22/2020 11:59:45       Male      Day  10th     Yes
## 77  10/22/2020 12:01:43     Female      Day   9th     Yes
## 78  10/22/2020 12:02:20       Male Boarding  12th     Yes
## 79  10/22/2020 12:03:22     Female      Day  10th      No
## 80  10/22/2020 12:05:13       Male      Day  10th     Yes
## 81  10/22/2020 12:10:45     Female Boarding  10th      No
## 82  10/22/2020 12:12:46       Male Boarding  10th     Yes
## 83  10/22/2020 12:14:09       Male Boarding  10th     Yes
## 84  10/22/2020 12:15:43       Male Boarding  11th     Yes
## 85  10/22/2020 12:17:08     Female Boarding  10th     Yes
## 86  10/22/2020 12:18:18     Female Boarding  10th     Yes
## 87  10/22/2020 12:23:53 Non-Binary      Day   9th      No
## 88  10/22/2020 12:26:07       Male Boarding  10th     Yes
## 89  10/22/2020 12:26:12     Female      Day  11th      No
## 90  10/22/2020 12:27:29       Male      Day  11th     Yes
## 91  10/22/2020 12:28:17       Male Boarding  12th      No
## 92  10/22/2020 12:29:39     Female Boarding  11th     Yes
## 93  10/22/2020 12:30:15     Female Boarding   9th     Yes
## 94  10/22/2020 12:35:22       Male Boarding  10th     Yes
## 95  10/22/2020 12:35:23       Male Boarding  11th     Yes
## 96  10/22/2020 12:36:17       Male      Day  12th      No
## 97  10/22/2020 12:38:38       Male      Day  11th      No
## 98  10/22/2020 12:39:36     Female Boarding  10th      No
## 99  10/22/2020 12:39:42     Female      Day   9th     Yes
## 100 10/22/2020 12:40:30       Male Boarding  11th      No
## 101 10/22/2020 12:41:01       Male Boarding  10th     Yes
## 102 10/22/2020 12:41:06     Female      Day  11th      No
## 103 10/22/2020 12:42:20       Male Boarding  11th     Yes
## 104 10/22/2020 12:44:45       Male Boarding  11th     Yes
## 105 10/22/2020 12:45:57       Male Boarding  12th     Yes
## 106 10/22/2020 12:47:51       Male Boarding  11th     Yes
## 107 10/22/2020 12:51:44       Male Boarding  11th     Yes
## 108 10/22/2020 12:57:00     Female Boarding   9th     Yes
## 109 10/22/2020 12:59:40       Male      Day  10th     Yes
## 110 10/22/2020 12:59:46       Male Boarding  12th     Yes
## 111 10/22/2020 13:01:03       Male Boarding  12th     Yes
## 112 10/22/2020 13:02:57     Female Boarding  11th     Yes
## 113 10/22/2020 13:04:22       Male Boarding  11th      No
## 114 10/22/2020 13:04:34     Female Boarding  10th      No
## 115 10/22/2020 13:11:39       Male Boarding  11th     Yes
## 116 10/22/2020 13:14:42       Male Boarding  12th      No
## 117 10/22/2020 13:40:43       Male Boarding  11th     Yes
## 118 10/22/2020 13:41:33       Male      Day  10th     Yes
## 119 10/22/2020 13:50:26     Female Boarding  11th      No
## 120 10/22/2020 13:50:58       Male Boarding  10th     Yes
## 121 10/22/2020 13:55:51     Female      Day  11th     Yes
## 122 10/22/2020 13:57:21     Female      Day   9th     Yes
## 123 10/22/2020 13:57:24       Male Boarding  12th     Yes
## 124 10/22/2020 13:59:24       Male Boarding  12th     Yes
## 125 10/22/2020 13:59:39       Male Boarding  10th     Yes
## 126 10/22/2020 14:00:37     Female Boarding  12th      No
## 127 10/22/2020 14:06:16       Male Boarding  10th     Yes
## 128 10/22/2020 14:14:03     Female Boarding  11th     Yes
## 129 10/22/2020 14:15:50       Male Boarding  11th     Yes
## 130 10/22/2020 14:18:55       Male      Day   9th      No
## 131 10/22/2020 14:21:35       Male Boarding  10th     Yes
## 132 10/22/2020 14:26:26       Male      Day  11th     Yes
## 133 10/22/2020 14:27:20     Female      Day  10th     Yes
## 134 10/22/2020 14:27:58       Male      Day  10th     Yes
## 135 10/22/2020 14:28:08     Female Boarding  12th     Yes
## 136 10/22/2020 14:30:57       Male Boarding  10th     Yes
## 137 10/22/2020 14:31:50       Male Boarding  11th     Yes
## 138 10/22/2020 14:56:43      Other      Day   9th     Yes
## 139 10/22/2020 15:00:32     Female Boarding   9th     Yes
## 140 10/22/2020 15:18:20     Female      Day   9th     Yes
## 141 10/22/2020 15:49:45     Female Boarding  11th     Yes
## 142 10/22/2020 16:01:32       Male Boarding  10th     Yes
## 143 10/22/2020 17:24:39       Male      Day  11th      No
## 144 10/22/2020 17:34:11     Female Boarding  10th      No
## 145 10/22/2020 17:47:59     Female      Day  10th     Yes
## 146 10/22/2020 18:09:12       Male      Day  12th      No
## 147 10/22/2020 18:23:39       Male Boarding  11th     Yes
## 148 10/22/2020 18:28:51       Male      Day  12th     Yes
## 149 10/22/2020 19:34:32     Female Boarding  10th     Yes
## 150 10/22/2020 19:46:38     Female Boarding  10th     Yes
## 151 10/22/2020 19:50:51       Male Boarding   9th     Yes
## 152 10/22/2020 19:57:47       Male      Day  10th     Yes
## 153 10/22/2020 20:40:06       Male Boarding  11th     Yes
## 154 10/22/2020 21:01:52     Female Boarding  10th     Yes
## 155 10/22/2020 21:07:53     Female      Day  10th     Yes
## 156 10/22/2020 21:31:12       Male      Day  11th     Yes
## 157 10/22/2020 21:35:17       Male Boarding  10th     Yes
## 158 10/22/2020 21:39:38       Male Boarding   9th        
## 159 10/22/2020 21:39:52       Male Boarding  12th     Yes
## 160  10/23/2020 5:52:55 Non-Binary      Day   9th      No
## 161  10/23/2020 8:02:17       Male      Day   9th      No
## 162  10/23/2020 8:51:19     Female      Day   9th     Yes
## 163 10/23/2020 10:39:15       Male Boarding  12th      No
## 164 10/23/2020 12:34:25     Female      Day  11th      No
## 165 10/23/2020 22:51:15       Male      Day   9th      No
## 166 10/24/2020 17:39:51     Female      Day  10th      No
## 167 10/25/2020 16:01:48       Male      Day   9th     Yes
##                  Activities DHBreakfast NDHBreakfast BClass DHBoxes NDHBoxes
## 1         None of the above           0            2     No       2        0
## 2                    Soccer           5            2     No       2        0
## 3                    Tennis           3            7    Yes       2        0
## 4         None of the above           0            4     No       2        0
## 5                   Musical           0            5    Yes       1        0
## 6                    Soccer           6            1    Yes       2        1
## 7   Pre-season conditioning           7            0    Yes       3        1
## 8                   Musical           0            7     No       1        0
## 9                    Tennis           2            0    Yes       2        0
## 10  Pre-season conditioning           4            3    Yes      NA       NA
## 11        None of the above           0            6     No       1        0
## 12        None of the above           7            0     No       2        0
## 13                   Soccer           0            7     No       3        0
## 14               Volleyball           0            4    Yes       2        0
## 15                 Football           5            2    Yes       2        1
## 16                  Musical           1            2    Yes       2        0
## 17               Volleyball           6            0    Yes       2        0
## 18               Volleyball           2            5     No       2        0
## 19                  Musical           6            1    Yes       2        1
## 20        None of the above           0            3    Yes       4        2
## 21  Pre-season conditioning           7            7    Yes       2        0
## 22        None of the above           2            1    Yes       2        0
## 23                   Soccer           0            7     No       4        0
## 24              Intramurals           0            7    Yes       3        0
## 25            Cross Country           2            1    Yes       2        1
## 26        None of the above           2            2     No       1        0
## 27                   Tennis           1            0    Yes       1        0
## 28        None of the above           1            7    Yes       2        1
## 29                  Musical           6            1    Yes       2        0
## 30                 Football           7            0    Yes       6        2
## 31                 Football           2            0    Yes       2        0
## 32        None of the above           6            7     No       3        0
## 33                 Football           7            3    Yes       5        2
## 34                   Soccer           7            0    Yes       3        0
## 35  Pre-season conditioning           0            1    Yes       1        0
## 36                   Tennis           1            6    Yes       1        0
## 37        None of the above           2            0     No       2        1
## 38              Intramurals           0            0    Yes       3        0
## 39            Cross Country           1            2    Yes       1        0
## 40                   Tennis           5            0    Yes       1        1
## 41                 Football           0            6    Yes       4        0
## 42                   Soccer           0            7     No       0        0
## 43        None of the above           7            0    Yes       1        0
## 44        None of the above           7            0     No       2        0
## 45              Intramurals           7            0     No       2        1
## 46                 Football           0            7    Yes       5       NA
## 47                 Football           2            2     No       1        0
## 48                   Soccer           3            6    Yes       5        1
## 49        None of the above           1            1    Yes       1        1
## 50                   Soccer           6            0     No       1        0
## 51                     Golf           7            7     No       9       10
## 52        None of the above           7            0     No       1        0
## 53                  Musical           1            6     No       1        0
## 54               Volleyball           7            0    Yes       2        0
## 55                  Musical           5            0     No       2        0
## 56                 Football           2            0    Yes       2        1
## 57        None of the above           5            2     No       1        0
## 58                   Tennis           7            7     No       1        0
## 59  Pre-season conditioning           5            2    Yes       4        1
## 60                 Football           6            1    Yes       4        2
## 61                 Football           7            2     No       3        1
## 62                  Musical           2            4    Yes       2        2
## 63  Pre-season conditioning           2            2    Yes       4        1
## 64                 Football           7            0    Yes       4        1
## 65            Cross Country           5            7    Yes       3        0
## 66        None of the above           5            2    Yes       1        0
## 67        None of the above           0            3     No       2        0
## 68               Volleyball           4            3    Yes       2        0
## 69                 Football           5            0    Yes       3        1
## 70                  Musical           0            1    Yes       1        0
## 71        None of the above           3            4    Yes       1        0
## 72        None of the above           0            7     No       3        0
## 73               Volleyball           2            5    Yes       2        0
## 74  Pre-season conditioning           4            0    Yes       3        2
## 75  Pre-season conditioning           6            0     No       4        0
## 76                     Golf           0            6    Yes       2        0
## 77            Cross Country           0            7     No       1        0
## 78                   Soccer           3            2    Yes       6        2
## 79        None of the above           3            5    Yes       2        0
## 80                 Football           1            2    Yes       3        0
## 81                  Musical           3            4    Yes       2        0
## 82                   Soccer           5            0    Yes       5        1
## 83                 Football           7            0    Yes       4        1
## 84                   Soccer           7            0    Yes       2        0
## 85        None of the above           2            3    Yes       2        0
## 86               Volleyball           1            0    Yes       2        0
## 87                  Musical           0            7     No       1        0
## 88  Pre-season conditioning           7            0    Yes       2        0
## 89                  Musical           0            1    Yes       1        1
## 90                 Football           0            7    Yes       2        0
## 91        None of the above           7            1    Yes       1        1
## 92                     Golf           7            0    Yes       2        1
## 93                  Musical           6            1     No       2        0
## 94                   Soccer           4            1     No       2        1
## 95            Cross Country           7            6    Yes       3        1
## 96        None of the above           0            0    Yes       1        0
## 97                  Musical           0            5    Yes       3        0
## 98                   Tennis           7            6     No       2        1
## 99               Volleyball           0            7     No       0        0
## 100                 Musical           0            0    Yes       2        1
## 101                Football           7            0    Yes       3        0
## 102                 Musical           0            0     No       2        0
## 103                Football           7            7     No       6        4
## 104 Pre-season conditioning           7            0    Yes       3        2
## 105                 Musical           2            2     No       3        0
## 106                Football           7            0     No       4        2
## 107                  Soccer           0            0    Yes       2        1
## 108           Cross Country           7            0     No       2        0
## 109             Intramurals           0            4    Yes       3        0
## 110 Pre-season conditioning           6            1     No       2        1
## 111                  Soccer           2            0    Yes       2        0
## 112                Football           5            7    Yes       2        0
## 113                 Musical           2            0     No       1        1
## 114                                   0            0     No       2        0
## 115             Intramurals           2            0    Yes       2        1
## 116       None of the above           2            0    Yes       3        1
## 117                Football           7            0     No       5        2
## 118       None of the above           0            7    Yes       4        0
## 119                    Golf           2            5     No       1        0
## 120                  Soccer           2            0    Yes       2        0
## 121                    Golf           4            3    Yes       2        1
## 122           Cross Country           0            7     No       1        0
## 123                Football           2            0     No       4        1
## 124                  Soccer           2            5     No       3        0
## 125                  Soccer           4            0    Yes       2        1
## 126           Cross Country           2            0    Yes       2        2
## 127 Pre-season conditioning           5            1    Yes       2        2
## 128           Cross Country           7            0     No       2        2
## 129                Football           7            7     No       2        0
## 130       None of the above           0            3    Yes       2        0
## 131       None of the above           7            0    Yes       3        2
## 132                Football           7            7    Yes       9        5
## 133       None of the above           5            0     No       1        0
## 134 Pre-season conditioning           6            1     No       5        1
## 135                    Golf           3            1    Yes       2        0
## 136                Football           4            3    Yes       4        1
## 137                  Soccer           7            0     No       3        0
## 138                  Soccer           3            4    Yes       2        0
## 139              Volleyball           4            0    Yes       2        1
## 140              Volleyball           0            6    Yes       1        0
## 141 Pre-season conditioning           7            0     No       2        0
## 142             Intramurals           6            5     No       3        0
## 143       None of the above           0            0     No       3        1
## 144       None of the above           7            2     No      NA       NA
## 145 Pre-season conditioning           0            3    Yes       2        0
## 146                 Musical           0            5     No       2        5
## 147             Intramurals           7            0    Yes       4        3
## 148       None of the above           0            1     No       5        0
## 149 Pre-season conditioning           0            0    Yes       1        0
## 150                 Musical           2            0     No       2        1
## 151       None of the above           4            3    Yes       2        0
## 152       None of the above           0            7     No       4        0
## 153                  Soccer           1            0    Yes       2        1
## 154              Volleyball           2            5    Yes       2        0
## 155              Volleyball           2            4     No       1        0
## 156                Football           1            6    Yes       3        1
## 157                  Soccer           6            2     No       2        1
## 158                Football           4            0    Yes       2        1
## 159                  Soccer           7            1     No       4        2
## 160                 Musical           0            0    Yes       1        0
## 161                 Musical           0            7     No       2        0
## 162                  Soccer           0            2    Yes       2        0
## 163       None of the above           2            0    Yes       2        1
## 164       None of the above           0            7     No       2        0
## 165           Cross Country           0            0     No       0        0
## 166 Pre-season conditioning           0            2    Yes       1        0
## 167           Cross Country           0            7     No       2        0
##     NDHMeals Nutrition Money
## 1          0         0  40.0
## 2          5         4  15.0
## 3         10         4  50.0
## 4          1         2   4.0
## 5          2         3  20.0
## 6          2         2    NA
## 7          2         3    NA
## 8          0         2    NA
## 9          4         2   5.0
## 10         4         3  50.0
## 11         0         4  25.0
## 12         2         1    NA
## 13         0         3  12.0
## 14         2         4    NA
## 15         3         4  10.0
## 16         1         2  12.0
## 17         2         3  20.0
## 18         5         5   5.0
## 19         1         4    NA
## 20        10         1  30.0
## 21         6         3    NA
## 22         4         3   8.0
## 23         0         5  30.0
## 24         3         3   3.0
## 25         2         4   3.0
## 26         5         2  30.0
## 27         1         0  45.0
## 28         9         4  20.0
## 29         6         4  10.0
## 30         5         4  12.0
## 31        10         3  20.0
## 32         0         5  30.0
## 33         6         2    NA
## 34         5         0  34.0
## 35         3         2  12.5
## 36        10         0  20.0
## 37         2         5  10.0
## 38         2         3   7.0
## 39         4         4  15.0
## 40         5         4   0.0
## 41         1         3  30.0
## 42         1         4    NA
## 43         4         3   7.0
## 44         0         4  10.0
## 45         4         3   5.0
## 46         4         4  10.0
## 47         3         3  28.0
## 48        10         5  10.0
## 49         3         2  50.0
## 50         3         0  17.5
## 51        10         5    NA
## 52         7         1 100.0
## 53         0         4  12.0
## 54         0         1  15.0
## 55         1         1  15.0
## 56         3         0  10.0
## 57         0         3  25.0
## 58         0         4    NA
## 59         4         0  25.0
## 60         8         5  20.0
## 61         2         4  15.0
## 62         7         2  10.0
## 63         5         4   7.0
## 64         3         3   7.0
## 65         4         4  15.0
## 66         2         3  20.0
## 67         0         2  20.0
## 68         4         1  40.0
## 69         4         0  10.0
## 70         2         0    NA
## 71         7         4  30.0
## 72         0         4   2.0
## 73         0         2  50.0
## 74        10         3  50.0
## 75         1         5   2.0
## 76         0         3  60.0
## 77         0         4    NA
## 78         3         3  20.0
## 79         0         1   2.0
## 80         0         4  12.0
## 81         5         1  20.0
## 82         3         5   5.0
## 83         3         3  15.0
## 84         2         5  20.0
## 85         3         4  30.0
## 86         1         0   3.0
## 87         0         2    NA
## 88         0         2   2.0
## 89         2         4  15.0
## 90         5         4  20.0
## 91         8         0   0.0
## 92         3         1  20.0
## 93         3         4   8.0
## 94         8         3  10.0
## 95         3         2   5.0
## 96         1         4   5.0
## 97         2         4  50.0
## 98         2        NA  20.0
## 99         0         3   3.0
## 100        5         3  15.0
## 101        1         3   5.0
## 102        1         1    NA
## 103        7         5    NA
## 104        0         0  25.0
## 105        5         4    NA
## 106       10         3   5.0
## 107        2         3    NA
## 108        0         5  30.0
## 109        0         0  50.0
## 110        2         4  15.0
## 111        0         0   5.0
## 112       10         0  40.0
## 113        7         3   0.0
## 114        6         0   7.0
## 115       10         0  20.0
## 116        2         4  20.0
## 117        6         5  15.0
## 118        0         2  10.0
## 119        5         5   1.5
## 120        0         1   5.0
## 121        4         1  30.0
## 122        3         2   3.0
## 123        5         5   5.0
## 124        0         5  40.0
## 125        4         0  50.0
## 126        3         2   5.0
## 127        1         4   3.0
## 128        0         3   0.0
## 129        3         5    NA
## 130        1         0  20.0
## 131        1         5    NA
## 132       10         5    NA
## 133        0         3   5.0
## 134        1         3  20.0
## 135        0         3  20.0
## 136        8         3  30.0
## 137        7         3  20.0
## 138        5         1    NA
## 139        2         0   7.0
## 140        4         3   6.0
## 141        4         0  15.0
## 142        6         5  25.0
## 143        0         2    NA
## 144        2         2  20.0
## 145        3         4  45.0
## 146        0         3   0.0
## 147        2         4  15.0
## 148        0         0    NA
## 149        0         1   2.0
## 150        2         3   5.0
## 151        2         1   3.0
## 152        0         4   0.0
## 153        4         3  20.0
## 154        5         5  20.0
## 155        0         5    NA
## 156        1         3  40.0
## 157        5         3  30.0
## 158        6         3   5.5
## 159        9         4  10.0
## 160        0         1  20.0
## 161        1         4    NA
## 162        2         0   0.0
## 163        5         1  25.0
## 164        0         3   0.0
## 165        0         0   1.0
## 166        0         4   0.0
## 167        0         3   3.0
```

### Data Cleaning

```r
# Rename column 
names(FoodSurvey)[names(FoodSurvey) == 'DHBreakfast'] <- 'breakfast.in.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHBreakfast'] <- 'breakfast.NOTin.DH'
names(FoodSurvey)[names(FoodSurvey) == 'BClass'] <- 'breakfast.in.Class'
names(FoodSurvey)[names(FoodSurvey) == 'DHBoxes'] <- 'BoxOfFood.in.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHBoxes'] <- 'BoxOfFood.NOTin.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHMeals'] <- 'Meal.in.Dorm'

head(FoodSurvey)
```

```
##             Timestamp Gender Boarding Grade Athlete        Activities
## 1 10/22/2020 11:36:55 Female      Day  12th      No None of the above
## 2 10/22/2020 11:36:58   Male Boarding  10th     Yes            Soccer
## 3 10/22/2020 11:36:59 Female      Day   9th     Yes            Tennis
## 4 10/22/2020 11:37:00   Male      Day  11th      No None of the above
## 5 10/22/2020 11:37:01 Female      Day  12th      No           Musical
## 6 10/22/2020 11:37:10   Male Boarding  11th     Yes            Soccer
##   breakfast.in.DH breakfast.NOTin.DH breakfast.in.Class BoxOfFood.in.DH
## 1               0                  2                 No               2
## 2               5                  2                 No               2
## 3               3                  7                Yes               2
## 4               0                  4                 No               2
## 5               0                  5                Yes               1
## 6               6                  1                Yes               2
##   BoxOfFood.NOTin.DH Meal.in.Dorm Nutrition Money
## 1                  0            0         0    40
## 2                  0            5         4    15
## 3                  0           10         4    50
## 4                  0            1         2     4
## 5                  0            2         3    20
## 6                  1            2         2    NA
```

```r
# Drop column "Timestamp "
FoodSurvey_drop <- select(FoodSurvey, -1)
head(FoodSurvey_drop)
```

```
##   Gender Boarding Grade Athlete        Activities breakfast.in.DH
## 1 Female      Day  12th      No None of the above               0
## 2   Male Boarding  10th     Yes            Soccer               5
## 3 Female      Day   9th     Yes            Tennis               3
## 4   Male      Day  11th      No None of the above               0
## 5 Female      Day  12th      No           Musical               0
## 6   Male Boarding  11th     Yes            Soccer               6
##   breakfast.NOTin.DH breakfast.in.Class BoxOfFood.in.DH BoxOfFood.NOTin.DH
## 1                  2                 No               2                  0
## 2                  2                 No               2                  0
## 3                  7                Yes               2                  0
## 4                  4                 No               2                  0
## 5                  5                Yes               1                  0
## 6                  1                Yes               2                  1
##   Meal.in.Dorm Nutrition Money
## 1            0         0    40
## 2            5         4    15
## 3           10         4    50
## 4            1         2     4
## 5            2         3    20
## 6            2         2    NA
```

```r
# delete 'NA' value
Data_FoodSurvey <- na.omit(FoodSurvey_drop)

head(Data_FoodSurvey)
```

```
##   Gender Boarding Grade Athlete        Activities breakfast.in.DH
## 1 Female      Day  12th      No None of the above               0
## 2   Male Boarding  10th     Yes            Soccer               5
## 3 Female      Day   9th     Yes            Tennis               3
## 4   Male      Day  11th      No None of the above               0
## 5 Female      Day  12th      No           Musical               0
## 9 Female Boarding  12th     Yes            Tennis               2
##   breakfast.NOTin.DH breakfast.in.Class BoxOfFood.in.DH BoxOfFood.NOTin.DH
## 1                  2                 No               2                  0
## 2                  2                 No               2                  0
## 3                  7                Yes               2                  0
## 4                  4                 No               2                  0
## 5                  5                Yes               1                  0
## 9                  0                Yes               2                  0
##   Meal.in.Dorm Nutrition Money
## 1            0         0    40
## 2            5         4    15
## 3           10         4    50
## 4            1         2     4
## 5            2         3    20
## 9            4         2     5
```
### Final Summary

```r
summary(Data_FoodSurvey) #get final summary after cleaning data
```

```
##     Gender            Boarding            Grade             Athlete         
##  Length:137         Length:137         Length:137         Length:137        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##   Activities        breakfast.in.DH breakfast.NOTin.DH breakfast.in.Class
##  Length:137         Min.   :0.000   Min.   :0.000      Length:137        
##  Class :character   1st Qu.:0.000   1st Qu.:0.000      Class :character  
##  Mode  :character   Median :2.000   Median :1.000      Mode  :character  
##                     Mean   :3.095   Mean   :2.277                        
##                     3rd Qu.:6.000   3rd Qu.:5.000                        
##                     Max.   :7.000   Max.   :7.000                        
##  BoxOfFood.in.DH BoxOfFood.NOTin.DH  Meal.in.Dorm     Nutrition    
##  Min.   :0.000   Min.   :0.0000     Min.   : 0.00   Min.   :0.000  
##  1st Qu.:2.000   1st Qu.:0.0000     1st Qu.: 0.00   1st Qu.:1.000  
##  Median :2.000   Median :0.0000     Median : 3.00   Median :3.000  
##  Mean   :2.299   Mean   :0.5255     Mean   : 3.19   Mean   :2.679  
##  3rd Qu.:3.000   3rd Qu.:1.0000     3rd Qu.: 5.00   3rd Qu.:4.000  
##  Max.   :6.000   Max.   :5.0000     Max.   :10.00   Max.   :5.000  
##      Money      
##  Min.   :  0.0  
##  1st Qu.:  5.0  
##  Median : 15.0  
##  Mean   : 17.2  
##  3rd Qu.: 25.0  
##  Max.   :100.0
```


### string of data after cleaning

```r
str(Data_FoodSurvey)
```

```
## 'data.frame':	137 obs. of  13 variables:
##  $ Gender            : chr  "Female" "Male" "Female" "Male" ...
##  $ Boarding          : chr  "Day" "Boarding" "Day" "Day" ...
##  $ Grade             : chr  "12th" "10th" "9th" "11th" ...
##  $ Athlete           : chr  "No" "Yes" "Yes" "No" ...
##  $ Activities        : chr  "None of the above" "Soccer" "Tennis" "None of the above" ...
##  $ breakfast.in.DH   : int  0 5 3 0 0 2 0 0 5 1 ...
##  $ breakfast.NOTin.DH: int  2 2 7 4 5 0 6 7 2 2 ...
##  $ breakfast.in.Class: chr  "No" "No" "Yes" "No" ...
##  $ BoxOfFood.in.DH   : int  2 2 2 2 1 2 1 3 2 2 ...
##  $ BoxOfFood.NOTin.DH: int  0 0 0 0 0 0 0 0 1 0 ...
##  $ Meal.in.Dorm      : int  0 5 10 1 2 4 0 0 3 1 ...
##  $ Nutrition         : int  0 4 4 2 3 2 4 3 4 2 ...
##  $ Money             : num  40 15 50 4 20 5 25 12 10 12 ...
##  - attr(*, "na.action")= 'omit' Named int [1:30] 6 7 8 10 12 14 19 21 33 42 ...
##   ..- attr(*, "names")= chr [1:30] "6" "7" "8" "10" ...
```
### Summary of Nutrition data


```r
summary(Data_FoodSurvey$Nutrition)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.000   1.000   3.000   2.679   4.000   5.000
```

### A. One Sample t-test of Nutrition with two sided tailed


```r
mean(Data_FoodSurvey$Nutrition) #get the mean of Nutrition
```

```
## [1] 2.678832
```

```r
sd (Data_FoodSurvey$Nutrition) #get the Standard Deviation of Nutrition
```

```
## [1] 1.603921
```

```r
t.test(Data_FoodSurvey$Nutrition, mu = 0, alternative = "two.sided")
```

```
## 
## 	One Sample t-test
## 
## data:  Data_FoodSurvey$Nutrition
## t = 19.549, df = 136, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##  2.407843 2.949822
## sample estimates:
## mean of x 
##  2.678832
```

```r
#If the p-value in a null hypothesis is greater than the significance level, then the null hypothesis is accepted
#If the p-value in an alternate hypothesis is smaller than the significance level, then the alternative hypothesis is accepted
```
### Visualize Nutrition using box plots for test-A


```r
ggboxplot(Data_FoodSurvey$Nutrition, 
          ylab = "Nutrition", xlab = FALSE,
          ggtheme = theme_minimal())
```

![](Sherafati-Module3-R-practice3_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

### Preliminary  test to check one-sample t-test assumptions for test-A


```r
shapiro.test(Data_FoodSurvey$Nutrition) # => p-value < 2.2e-16
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  Data_FoodSurvey$Nutrition
## W = 0.89561, p-value = 2.392e-08
```

### Visual inspection for test-A


```r
ggqqplot(Data_FoodSurvey$Nutrition, ylab = "Nutrition",
         ggtheme = theme_minimal()) #From the normality plots, we conclude that the data may come from normal distributions.
```

```
## Warning: The following aesthetics were dropped during statistical transformation: sample
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
## The following aesthetics were dropped during statistical transformation: sample
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

![](Sherafati-Module3-R-practice3_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

### Compute one-sample t-test for test-A


```r
#We want to know, if the average Nutrition of the students differs from 5 (two-tailed test)?
# One-sample t-test
test1 <- t.test(Data_FoodSurvey$Nutrition, mu = 5)
# Printing the results
test1 
```

```
## 
## 	One Sample t-test
## 
## data:  Data_FoodSurvey$Nutrition
## t = -16.939, df = 136, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 5
## 95 percent confidence interval:
##  2.407843 2.949822
## sample estimates:
## mean of x 
##  2.678832
```

### B. One Sample t-test of Having Meal.in.Dorm with right tailed


```r
mean(Data_FoodSurvey$Meal.in.Dorm) #get the mean of Meal.in.Dorm
```

```
## [1] 3.189781
```

```r
sd (Data_FoodSurvey$Meal.in.Dorm) #get the Standard Deviation of Meal.in.Dorm
```

```
## [1] 2.959364
```

```r
#We want to know, if the average Nutrition of the students greater than 10 (greater-tailed test)?
test2 <- t.test(Data_FoodSurvey$Meal.in.Dorm, mu = 10, alternative = 'greater') #One Sample t-test of Meal.in.Dorm
test2
```

```
## 
## 	One Sample t-test
## 
## data:  Data_FoodSurvey$Meal.in.Dorm
## t = -26.935, df = 136, p-value = 1
## alternative hypothesis: true mean is greater than 10
## 95 percent confidence interval:
##  2.771051      Inf
## sample estimates:
## mean of x 
##  3.189781
```

### Visualize of Having Meal.in.Dorm using box plots for test-B


```r
ggboxplot(Data_FoodSurvey$Meal.in.Dorm, 
          ylab = "Meal.in.Dorm", xlab = FALSE,
          ggtheme = theme_minimal())
```

![](Sherafati-Module3-R-practice3_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

### Preliminary  test to check one-sample t-test assumptions for test-B


```r
shapiro.test(Data_FoodSurvey$Meal.in.Dorm) # => p-value = 1
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  Data_FoodSurvey$Meal.in.Dorm
## W = 0.88627, p-value = 7.902e-09
```

### Visual inspection for test-B


```r
ggqqplot(Data_FoodSurvey$Meal.in.Dorm, ylab = "Meal in Dorm",
         ggtheme = theme_minimal()) #From the normality plots, we conclude that the data may come from normal distributions.
```

```
## Warning: The following aesthetics were dropped during statistical transformation: sample
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
## The following aesthetics were dropped during statistical transformation: sample
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

![](Sherafati-Module3-R-practice3_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

### C. One Sample t-test of  breakfast.in.Dinng Hall with left tailed


```r
mean(Data_FoodSurvey$breakfast.in.DH) #get the mean of breakfast.in.Dinng Hall
```

```
## [1] 3.094891
```

```r
sd(Data_FoodSurvey$breakfast.in.DH) #get the standard deviation of breakfast.in.Dinng Hall
```

```
## [1] 2.645427
```

```r
##We want to know, if the average Nutrition of the students less than 7 (less-tailed test)?
test3 <- t.test(Data_FoodSurvey$breakfast.in.DH, mu = 7, alternative = 'less') #One Sample t-test of breakfast.in.DH with 
test3
```

```
## 
## 	One Sample t-test
## 
## data:  Data_FoodSurvey$breakfast.in.DH
## t = -17.278, df = 136, p-value < 2.2e-16
## alternative hypothesis: true mean is less than 7
## 95 percent confidence interval:
##    -Inf 3.4692
## sample estimates:
## mean of x 
##  3.094891
```

### Visualize of having breakfast in dining hall using box plots for test-C


```r
ggboxplot(Data_FoodSurvey$breakfast.in.DH, 
          ylab = "breakfast in dining hall", xlab = FALSE,
          ggtheme = theme_minimal())
```

![](Sherafati-Module3-R-practice3_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

### Preliminary  test to check one-sample t-test assumptions for test-C


```r
shapiro.test(Data_FoodSurvey$breakfast.in.DH) # => p-value < 2.2e-16
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  Data_FoodSurvey$breakfast.in.DH
## W = 0.86192, p-value = 5.637e-10
```

### Compute one-sample t-test for test-C


```r
#We want to know, if the average of breakfast.in.DH of the students less than 7 (left-tailed test)?
# One-sample t-test
test2 <- t.test(Data_FoodSurvey$breakfast.in.DH, mu = 7)
# Printing the results
test2
```

```
## 
## 	One Sample t-test
## 
## data:  Data_FoodSurvey$breakfast.in.DH
## t = -17.278, df = 136, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 7
## 95 percent confidence interval:
##  2.647934 3.541847
## sample estimates:
## mean of x 
##  3.094891
```

### Hypothesis testing for p-value


```r
A <- Data_FoodSurvey$BoxOfFood.NOTin.DH  #create data A for one variable n dataset

B <- Data_FoodSurvey$breakfast.NOTin.DH # #create data B for another variable n dataset

wilcox.test(A,B, paired = TRUE, correct = TRUE)  #Wilcoxon rank sum exact test for A and B 
```

```
## 
## 	Wilcoxon signed rank test with continuity correction
## 
## data:  A and B
## V = 776.5, p-value = 1.441e-09
## alternative hypothesis: true location shift is not equal to 0
```

### P-value test for Nutrition


```r
t = (mean(Data_FoodSurvey$Nutrition)-2.5)/(sd(Data_FoodSurvey$Nutrition)/sqrt(length(Data_FoodSurvey$Nutrition)))
2*pt(-abs(t),df=length(Data_FoodSurvey$Nutrition)-1)
```

```
## [1] 0.1940841
```

```r
pt(t, df=136, lower.tail=T)
```

```
## [1] 0.902958
```

```r
pt(t, df=136, lower.tail=F)
```

```
## [1] 0.09704203
```

### P-value test for breakfast.in.DH


```r
t = (mean(Data_FoodSurvey$breakfast.in.DH)-2.5)/(sd(Data_FoodSurvey$breakfast.in.DH)/sqrt(length(Data_FoodSurvey$breakfast.in.DH)))
2*pt(-abs(t),df=length(Data_FoodSurvey$breakfast.in.DH)-1)
```

```
## [1] 0.009466932
```

```r
pt(t, df=136, lower.tail=T)
```

```
## [1] 0.9952665
```

```r
pt(t, df=136, lower.tail=F)
```

```
## [1] 0.004733466
```

### P-value test for Meal.in.Dorm


```r
t = (mean(Data_FoodSurvey$Meal.in.Dorm)-2.5)/(sd(Data_FoodSurvey$Meal.in.Dorm)/sqrt(length(Data_FoodSurvey$Meal.in.Dorm)))
2*pt(-abs(t),df=length(Data_FoodSurvey$Meal.in.Dorm)-1)
```

```
## [1] 0.007209708
```

```r
pt(t, df=136, lower.tail=T)
```

```
## [1] 0.9963951
```

```r
pt(t, df=136, lower.tail=F)
```

```
## [1] 0.003604854
```

### References

I.	D. Iacobucci, On p-values, Journal of Consumer Research 32 (June 2005), no. 1, 6–12. http://www.journals.uchicago.edu/cgi-bin/resolve?JCR320199PDF 

II.	R. Mason, D. Lind, and W. Marchal, Statistics, An Introduction; 5th edition; Duxbury Press; Brooks/Cole Publishing Company; 1998. 

III.	R. Savage and K. W. Deutsch, A Statistical Model of the Gross Analysis of Transaction Flows Econometrica 28 (1960), no. 3 551–572. 

IV.	Y. M. M. Bishop and S. E. Fienberg, Incomplete Two-Dimensional Contingency Tables, Biometrics 25 (1969), no. 1, 119–128. 




### Appendix

```r
#install.packages("plyr")
#install.packages("dplyr")
#install.packages("tidyr")
#install.packages("tidyverse")
#install.packages("psych")
#install.packages("ggpubr")
#install.packages("ggplot2")
#install.packages("plotly")
#install.packages("moments")
#install.packages('gmodels')
#install.packages("stringr")


library(plyr) 
library(dplyr)
library(tidyr)
library(tidyverse)
library(psych)
library(ggpubr)
library(ggplot2)
library(plotly)
library(moments)
library(gmodels)
library(stringr)

FoodSurvey = read.csv("~/Documents/Northeastern University/ALY 6010/dataset/Food Survey - Data.csv", header = TRUE)
FoodSurvey

summary(FoodSurvey) #Initial summary

df_foodsurvey <- as.data.frame(read.csv("~/Documents/Northeastern University/ALY 6010/dataset/Food Survey - Data.csv", header = TRUE))
df_foodsurvey

# Rename column 
names(FoodSurvey)[names(FoodSurvey) == 'DHBreakfast'] <- 'breakfast.in.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHBreakfast'] <- 'breakfast.NOTin.DH'
names(FoodSurvey)[names(FoodSurvey) == 'BClass'] <- 'breakfast.in.Class'
names(FoodSurvey)[names(FoodSurvey) == 'DHBoxes'] <- 'BoxOfFood.in.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHBoxes'] <- 'BoxOfFood.NOTin.DH'
names(FoodSurvey)[names(FoodSurvey) == 'NDHMeals'] <- 'Meal.in.Dorm'

head(FoodSurvey)

# Drop column "Timestamp "
FoodSurvey_drop <- select(FoodSurvey, -1)
head(FoodSurvey_drop)

# delete 'NA' value
Data_FoodSurvey <- na.omit(FoodSurvey_drop)

head(Data_FoodSurvey)

summary(Data_FoodSurvey) #get final summary after cleaning data

str(Data_FoodSurvey)

summary(Data_FoodSurvey$Nutrition)

mean(Data_FoodSurvey$Nutrition) #get the mean of Nutrition
sd (Data_FoodSurvey$Nutrition) #get the Standard Deviation of Nutrition

t.test(Data_FoodSurvey$Nutrition, mu = 0, alternative = "two.sided")

#If the p-value in a null hypothesis is greater than the significance level, then the null hypothesis is accepted
#If the p-value in an alternate hypothesis is smaller than the significance level, then the alternative hypothesis is accepted

ggboxplot(Data_FoodSurvey$Nutrition, 
          ylab = "Nutrition", xlab = FALSE,
          ggtheme = theme_minimal())

shapiro.test(Data_FoodSurvey$Nutrition) # => p-value < 2.2e-16

ggqqplot(Data_FoodSurvey$Nutrition, ylab = "Nutrition",
         ggtheme = theme_minimal()) #From the normality plots, we conclude that the data may come from normal distributions.

#We want to know, if the average Nutrition of the students differs from 5 (two-tailed test)?
# One-sample t-test
test1 <- t.test(Data_FoodSurvey$Nutrition, mu = 5)
# Printing the results
test1 

mean(Data_FoodSurvey$Meal.in.Dorm) #get the mean of Meal.in.Dorm
sd (Data_FoodSurvey$Meal.in.Dorm) #get the Standard Deviation of Meal.in.Dorm

#We want to know, if the average Nutrition of the students greater than 10 (greater-tailed test)?
test2 <- t.test(Data_FoodSurvey$Meal.in.Dorm, mu = 10, alternative = 'greater') #One Sample t-test of Meal.in.Dorm
test2

ggboxplot(Data_FoodSurvey$Meal.in.Dorm, 
          ylab = "Meal.in.Dorm", xlab = FALSE,
          ggtheme = theme_minimal())

shapiro.test(Data_FoodSurvey$Meal.in.Dorm) # => p-value = 1

ggqqplot(Data_FoodSurvey$Meal.in.Dorm, ylab = "Meal in Dorm",
         ggtheme = theme_minimal()) #From the normality plots, we conclude that the data may come from normal distributions.

mean(Data_FoodSurvey$breakfast.in.DH) #get the mean of breakfast.in.Dinng Hall
sd(Data_FoodSurvey$breakfast.in.DH) #get the standard deviation of breakfast.in.Dinng Hall

##We want to know, if the average Nutrition of the students less than 7 (less-tailed test)?
test3 <- t.test(Data_FoodSurvey$breakfast.in.DH, mu = 7, alternative = 'less') #One Sample t-test of breakfast.in.DH with 
test3

ggboxplot(Data_FoodSurvey$breakfast.in.DH, 
          ylab = "breakfast in dining hall", xlab = FALSE,
          ggtheme = theme_minimal())

shapiro.test(Data_FoodSurvey$breakfast.in.DH) # => p-value < 2.2e-16

#We want to know, if the average of breakfast.in.DH of the students less than 7 (left-tailed test)?
# One-sample t-test
test2 <- t.test(Data_FoodSurvey$breakfast.in.DH, mu = 7)
# Printing the results
test2

A <- Data_FoodSurvey$BoxOfFood.NOTin.DH  #create data A for one variable n dataset

B <- Data_FoodSurvey$breakfast.NOTin.DH # #create data B for another variable n dataset

wilcox.test(A,B, paired = TRUE, correct = TRUE)  #Wilcoxon rank sum exact test for A and B 


t = (mean(Data_FoodSurvey$Nutrition)-2.5)/(sd(Data_FoodSurvey$Nutrition)/sqrt(length(Data_FoodSurvey$Nutrition)))
2*pt(-abs(t),df=length(Data_FoodSurvey$Nutrition)-1)
pt(t, df=136, lower.tail=T)
pt(t, df=136, lower.tail=F)

t = (mean(Data_FoodSurvey$breakfast.in.DH)-2.5)/(sd(Data_FoodSurvey$breakfast.in.DH)/sqrt(length(Data_FoodSurvey$breakfast.in.DH)))
2*pt(-abs(t),df=length(Data_FoodSurvey$breakfast.in.DH)-1)
pt(t, df=136, lower.tail=T)
pt(t, df=136, lower.tail=F)

t = (mean(Data_FoodSurvey$Meal.in.Dorm)-2.5)/(sd(Data_FoodSurvey$Meal.in.Dorm)/sqrt(length(Data_FoodSurvey$Meal.in.Dorm)))
2*pt(-abs(t),df=length(Data_FoodSurvey$Meal.in.Dorm)-1)
pt(t, df=136, lower.tail=T)
pt(t, df=136, lower.tail=F)

## NA
```


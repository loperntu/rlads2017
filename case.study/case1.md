---
title: "案例學習一"
output: html_notebook
---

========================================================

## 程式知識

每次打開 `RStudio` 的範例檔，會看到`summary(cars)` 與 `plot(cars)` 的釋例，這些是什麼呢？

> 為了達成某種目的，學習電腦可以理解的語言，用該語言「說出」指令，希望電腦遵循，達到目的。所謂程式碼就是一連串的指令。食譜常常是個好的譬喻。


The Data Science Process
資料科學的食譜是什麼？

![](figure/ds_process.png)


#### 簡化一點的圖解

![概念就只有這樣了](figure/ds_flow.png)

## Data Science: R(Studio) way

![](figure/ds_flow_r.png)



## 我們要學習的，就是從「步驟」到「程式碼」。
- 所以你要了解 R 語言的「詞彙」和「語法」。


```r
# example
log(3)
```

```
## [1] 1.098612
```

```r
3^2
```

```
## [1] 9
```



- 如果你「說錯話」了（很大的機率是你要負責），就要學著去**除錯**（debugging）。
    - 注意：說錯話還可分成「語法錯誤」還是「語意錯誤」。


## 學著從電腦的角度來思考
- 目前的電腦需要明確、循序漸進的指令。

舉例來說，妳如何判斷圖中誰最高？
![](figure/persons.png)


那你覺得電腦如何判斷？

- 先從第一個人的身高開始。
- 假定他是「最高的人」。
- 將其他人的身高和目前「最高的人」做逐一的比較。
- 每次只要有人身高超越目前「最高的人」，他就取代變成目前「最高的人」。
- 比較完之後，就找出來了。


當然簡單的程式碼單純只處理一個事件。


```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```


不過，利用 $\mathcal{R}$ 內建的函式已經可以做很多事了。


```r
# 可以先利用內建 (built-in) 的數據來親近 R
#data()
#head()
#tail()
#names()
#dim()
#cor()
#plot()
#par() 用來設定畫圖相關的參數
par(mfrow=c(1,2))
```


# 小組案例練習：幫助台灣的流浪狗

- 各縣市的流浪狗資料
    - `captured` 1999-2008 捕捉數目
    - `adoptedR` 被捕捉的流浪狗被認養的比例
    - `killedR` 被安樂死的比例
    - `unknownR` 狀況不明的比例
    - `population` 各縣市 2008 人口數
    - `graduate` 研究所畢業者人數
    - `farmArea` 各縣市耕地佔總面積的比例
    - `divorced` 離婚者所佔的比例
    - `unemployed` 失業率
    - `crimeR` 每 10 萬人刑事案件數目
    - `oldR` 老年福利金額佔年度支出比例
    - `computerR` 平均每 100 個家庭的電腦數目


> 從這筆資料可以看出什麼訊息？


```r
# read.table(): imports a data table from a file and create a data frame
# dogs <- read.table("c:/rlads2017/dogs.txt", header = TRUE)
dogs <- read.table("../data/txt/dogs.txt",header = TRUE)
```

```
## Warning in file(file, "rt"): 無法開啟檔案 '../data/txt/dogs.txt' ：No such
## file or directory
```

```
## Error in file(file, "rt"): 無法開啟連結
```

```r
#dogs <- read.table("https://raw.githubusercontent.com/loperntu/rlads2017/master/data/txt/dogs.txt", header = TRUE)
dogs
```

```
## Error in eval(expr, envir, enclos): 找不到物件 'dogs'
```

為方便計算，可以先將 `graduate` 作比例化。


```r
dogs$graduate <- round(dogs$graduate * 100 / dogs$population, 2)
```

```
## Error in eval(expr, envir, enclos): 找不到物件 'dogs'
```

```r
dogs
```

```
## Error in eval(expr, envir, enclos): 找不到物件 'dogs'
```



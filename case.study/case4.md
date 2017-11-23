---
title: "案例學習四"
output: 
  html_notebook:
    toc: true
---


```r
needed <- c("jiebaR", "tidytext", "gutenbergr", "rvest", "wordcloud2")
install.packages(needed)
```

```
## Error in contrib.url(repos, "source"): trying to use CRAN without setting a mirror
```

## Text as Data






## REGEX



## EDA of Textual Data


### 中文的處理


```r
library(gutenbergr)
```

```
## Warning: package 'gutenbergr' was built under R version 3.2.5
```

```r
library(jiebaR)
```

```
## Warning: package 'jiebaR' was built under R version 3.2.5
```

```
## Loading required package: jiebaRD
```

```r
library(jiebaRD)
library(wordcloud)
```

```
## Loading required package: RColorBrewer
```


- 為何需要斷詞


```r
# 建立斷詞類型
cutter <- worker()
cutter 
```

```
## Worker Type:  Jieba Segment
## 
## Default Method  :  mix
## Detect Encoding :  TRUE
## Default Encoding:  UTF-8
## Keep Symbols    :  FALSE
## Output Path     :  
## Write File      :  TRUE
## By Lines        :  FALSE
## Max Word Length :  20
## Max Read Lines  :  1e+05
## 
## Fixed Model Components:  
## 
## $dict
## [1] "/Library/Frameworks/R.framework/Versions/3.2/Resources/library/jiebaRD/dict/jieba.dict.utf8"
## 
## $user
## [1] "/Library/Frameworks/R.framework/Versions/3.2/Resources/library/jiebaRD/dict/user.dict.utf8"
## 
## $hmm
## [1] "/Library/Frameworks/R.framework/Versions/3.2/Resources/library/jiebaRD/dict/hmm_model.utf8"
## 
## $stop_word
## NULL
## 
## $user_weight
## [1] "max"
## 
## $timestamp
## [1] 1508970209
## 
## $default $detect $encoding $symbol $output $write $lines $bylines can be reset.
```


- 斷詞簡單例子

```r
cutter["台大最近的新聞怎麼這麼多"]
```

```
## [1] "台大" "最近" "的"   "新聞" "怎麼" "這麼" "多"
```


```r
cutter["據台大語言所小編謝舒凱表示，宅宅也是非常用功 der"]
```

```
##  [1] "據"     "台大"   "語言所" "小編"   "謝舒凱" "表示"   "宅宅"  
##  [8] "也"     "是"     "非常"   "用功"   "der"
```




```r
cutter = worker(stop_word ="stop.txt")
```

```
## Error in worker(stop_word = "stop.txt"): There is no such file for stop words.
```

```r
cutter["這是一段測試用文字，請不要再戰長庚資管了"]
```

```
##  [1] "這是" "一段" "測試" "用"   "文字" "請"   "不要" "再戰" "長庚" "資管"
## [11] "了"
```

- 詞類標記


```r
tagger <- worker("tag")
tagger["台大最近的新聞怎麼這麼多"]
```

```
##     ns      f     uj      n      r      r      m 
## "台大" "最近"   "的" "新聞" "怎麼" "這麼"   "多"
```





```r
library(rvest)
```

```
## Warning: package 'rvest' was built under R version 3.2.5
```

```
## Loading required package: xml2
```

```
## Warning: package 'xml2' was built under R version 3.2.5
```

```r
PTTNBA <- "https://www.ptt.cc/bbs/NBA/index.html"
pttContent <- read_html(PTTNBA)
post_title <- pttContent %>% 
  html_nodes(".title") %>% 
  html_text()
post_title
```

```
##  [1] "\n\t\t\t\n\t\t\t\t[新聞] 破32年紀錄　詹皇改打控衛：從小就會解\n\t\t\t\n\t\t\t"    
##  [2] "\n\t\t\t\n\t\t\t\tRe: [討論] 為甚麼得分王不改回加總計分方式?\n\t\t\t\n\t\t\t"     
##  [3] "\n\t\t\t\n\t\t\t\t[花邊] Melo談為何頻繁出手長兩分：有空位我就投\n\t\t\t\n\t\t\t"  
##  [4] "\n\t\t\t\n\t\t\t\t(本文已被刪除) <ss2121647>\n\t\t\t\n\t\t\t"                     
##  [5] "\n\t\t\t\n\t\t\t\t[新聞] NBA／曾經考慮加盟尼克 厄文：因為離家近\n\t\t\t\n\t\t\t"  
##  [6] "\n\t\t\t\n\t\t\t\tRe: [討論] 為甚麼得分王不改回加總計分方式?\n\t\t\t\n\t\t\t"     
##  [7] "\n\t\t\t\n\t\t\t\t[花邊] 易建聯最難以啟齒的過往 美媒公佈「椅子\n\t\t\t\n\t\t\t"   
##  [8] "\n\t\t\t\n\t\t\t\tRe: [討論] 字母哥這身體素質怎麼15順位才被選到？\n\t\t\t\n\t\t\t"
##  [9] "\n\t\t\t\n\t\t\t\t[討論] 為何Wade難以融入騎士先發陣容?\n\t\t\t\n\t\t\t"           
## [10] "\n\t\t\t\n\t\t\t\t[情報] ★今日排名(2017.10.25)★\n\t\t\t\n\t\t\t"                
## [11] "\n\t\t\t\n\t\t\t\t[新聞] 戈塔陣前叫囂 鮑爾：我習以為常\n\t\t\t\n\t\t\t"           
## [12] "\n\t\t\t\n\t\t\t\t[花邊] Wade老婆: 我只能排在韋德心中的第三位\n\t\t\t\n\t\t\t"    
## [13] "\n\t\t\t\n\t\t\t\t[新聞] NBA／詹皇領軍騎士來勢洶洶 籃網主帥：\n\t\t\t\n\t\t\t"    
## [14] "\n\t\t\t\n\t\t\t\t[情報] Markelle Fultz 將缺席接下來三場比賽\n\t\t\t\n\t\t\t"     
## [15] "\n\t\t\t\n\t\t\t\t(本文已被刪除) [TimmyDD]\n\t\t\t\n\t\t\t"                       
## [16] "\n\t\t\t\n\t\t\t\t[情報] Tony Parker被馬刺下放到發展聯盟\n\t\t\t\n\t\t\t"         
## [17] "\n\t\t\t\n\t\t\t\t[情報] 現役球員 目前進名人堂的機率\n\t\t\t\n\t\t\t"             
## [18] "\n\t\t\t\n\t\t\t\tRe: [情報] Tony Parker被馬刺下放到發展聯盟\n\t\t\t\n\t\t\t"     
## [19] "\n\t\t\t\n\t\t\t\t[情報] CP25今天對戰小牛輪休\n\t\t\t\n\t\t\t"                    
## [20] "\n\t\t\t\n\t\t\t\t[情報] 2017-18 自由球員市場異動整理表 (9/26)\n\t\t\t\n\t\t\t"   
## [21] "\n\t\t\t\n\t\t\t\t[公告] 板規v6.2\n\t\t\t\n\t\t\t"                                
## [22] "\n\t\t\t\n\t\t\t\t[情報] 2017-18 例行賽賽程 (10/18-31)\n\t\t\t\n\t\t\t"           
## [23] "\n\t\t\t\n\t\t\t\t[情報] NBA Standings (171025)\n\t\t\t\n\t\t\t"                  
## [24] "\n\t\t\t\n\t\t\t\t[情報] ★今日排名(2017.10.25)★\n\t\t\t\n\t\t\t"
```


```r
cutter <- worker()
cutter[post_title] ## 不分行輸出
```

```
##   [1] "新聞"       "破"         "32"         "年"         "紀錄"      
##   [6] "　"         "詹"         "皇"         "改"         "打"        
##  [11] "控"         "衛"         "從小"       "就會解"     "Re"        
##  [16] "討論"       "為"         "甚麼"       "得分王"     "不"        
##  [21] "改回"       "加"         "總計"       "分"         "方式"      
##  [26] "花邊"       "Melo"       "談為何"     "頻繁"       "出手"      
##  [31] "長"         "兩分"       "有"         "空位"       "我"        
##  [36] "就"         "投"         "本文"       "已"         "被"        
##  [41] "刪除"       "ss2121647"  "新聞"       "NBA"        "曾經"      
##  [46] "考慮"       "加盟"       "尼克"       "厄文"       "因"        
##  [51] "為"         "離家近"     "Re"         "討論"       "為"        
##  [56] "甚麼"       "得分王"     "不"         "改回"       "加"        
##  [61] "總計"       "分"         "方式"       "花邊"       "易建聯"    
##  [66] "最"         "難以"       "啟齒"       "的"         "過往"      
##  [71] "美媒"       "公佈"       "椅子"       "Re"         "討論"      
##  [76] "字母"       "哥"         "這"         "身體素質"   "怎麼"      
##  [81] "15"         "順位"       "才"         "被"         "選到"      
##  [86] "討論"       "為何"       "Wade"       "難以"       "融入"      
##  [91] "騎士"       "先發"       "陣容"       "情報"       "今日"      
##  [96] "排名"       "2017.10.25" "新聞"       "戈塔"       "陣前"      
## [101] "叫囂"       "鮑爾"       "我"         "習"         "以"        
## [106] "為"         "常"         "花邊"       "Wade"       "老婆"      
## [111] "我"         "只能"       "排在"       "韋德"       "心中"      
## [116] "的"         "第三位"     "新聞"       "NBA"        "詹"        
## [121] "皇"         "領軍"       "騎士"       "來勢洶洶"   "籃網"      
## [126] "主帥"       "情報"       "Markelle"   "Fultz"      "將"        
## [131] "缺席"       "接下來"     "三場"       "比賽"       "本文"      
## [136] "已"         "被"         "刪除"       "TimmyDD"    "情報"      
## [141] "Tony"       "Parker"     "被"         "馬刺"       "下"        
## [146] "放到"       "發展"       "聯盟"       "情報"       "現役"      
## [151] "球員"       "目前"       "進"         "名人堂"     "的"        
## [156] "機率"       "Re"         "情報"       "Tony"       "Parker"    
## [161] "被"         "馬刺"       "下"         "放到"       "發展"      
## [166] "聯盟"       "情報"       "CP25"       "今天"       "對戰"      
## [171] "小牛"       "輪休"       "情報"       "2017-18"    "自由"      
## [176] "球員"       "市場"       "異動"       "整理表"     "9"         
## [181] "26"         "公告"       "板規"       "v6"         "2"         
## [186] "情報"       "2017-18"    "例行"       "賽"         "賽程"      
## [191] "10"         "18-31"      "情報"       "NBA"        "Standings" 
## [196] "171025"     "情報"       "今日"       "排名"       "2017.10.25"
```





```r
cutter$bylines<-T
cutter[post_title] ## 分行輸出
```

```
## [[1]]
##  [1] "新聞"   "破"     "32"     "年"     "紀錄"   "　"     "詹"    
##  [8] "皇"     "改"     "打"     "控"     "衛"     "從小"   "就會解"
## 
## [[2]]
##  [1] "Re"     "討論"   "為"     "甚麼"   "得分王" "不"     "改回"  
##  [8] "加"     "總計"   "分"     "方式"  
## 
## [[3]]
##  [1] "花邊"   "Melo"   "談為何" "頻繁"   "出手"   "長"     "兩分"  
##  [8] "有"     "空位"   "我"     "就"     "投"    
## 
## [[4]]
## [1] "本文"      "已"        "被"        "刪除"      "ss2121647"
## 
## [[5]]
##  [1] "新聞"   "NBA"    "曾經"   "考慮"   "加盟"   "尼克"   "厄文"  
##  [8] "因"     "為"     "離家近"
## 
## [[6]]
##  [1] "Re"     "討論"   "為"     "甚麼"   "得分王" "不"     "改回"  
##  [8] "加"     "總計"   "分"     "方式"  
## 
## [[7]]
##  [1] "花邊"   "易建聯" "最"     "難以"   "啟齒"   "的"     "過往"  
##  [8] "美媒"   "公佈"   "椅子"  
## 
## [[8]]
##  [1] "Re"       "討論"     "字母"     "哥"       "這"       "身體素質"
##  [7] "怎麼"     "15"       "順位"     "才"       "被"       "選到"    
## 
## [[9]]
## [1] "討論" "為何" "Wade" "難以" "融入" "騎士" "先發" "陣容"
## 
## [[10]]
## [1] "情報"       "今日"       "排名"       "2017.10.25"
## 
## [[11]]
##  [1] "新聞" "戈塔" "陣前" "叫囂" "鮑爾" "我"   "習"   "以"   "為"   "常"  
## 
## [[12]]
##  [1] "花邊"   "Wade"   "老婆"   "我"     "只能"   "排在"   "韋德"  
##  [8] "心中"   "的"     "第三位"
## 
## [[13]]
## [1] "新聞"     "NBA"      "詹"       "皇"       "領軍"     "騎士"    
## [7] "來勢洶洶" "籃網"     "主帥"    
## 
## [[14]]
## [1] "情報"     "Markelle" "Fultz"    "將"       "缺席"     "接下來"  
## [7] "三場"     "比賽"    
## 
## [[15]]
## [1] "本文"    "已"      "被"      "刪除"    "TimmyDD"
## 
## [[16]]
## [1] "情報"   "Tony"   "Parker" "被"     "馬刺"   "下"     "放到"   "發展"  
## [9] "聯盟"  
## 
## [[17]]
## [1] "情報"   "現役"   "球員"   "目前"   "進"     "名人堂" "的"     "機率"  
## 
## [[18]]
##  [1] "Re"     "情報"   "Tony"   "Parker" "被"     "馬刺"   "下"    
##  [8] "放到"   "發展"   "聯盟"  
## 
## [[19]]
## [1] "情報" "CP25" "今天" "對戰" "小牛" "輪休"
## 
## [[20]]
## [1] "情報"    "2017-18" "自由"    "球員"    "市場"    "異動"    "整理表" 
## [8] "9"       "26"     
## 
## [[21]]
## [1] "公告" "板規" "v6"   "2"   
## 
## [[22]]
## [1] "情報"    "2017-18" "例行"    "賽"      "賽程"    "10"      "18-31"  
## 
## [[23]]
## [1] "情報"      "NBA"       "Standings" "171025"   
## 
## [[24]]
## [1] "情報"       "今日"       "排名"       "2017.10.25"
```


```r
new_user_word(cutter,'Markelle Fultz',"n")
```

```
## [1] TRUE
```

- 最常出現的詞彙


```r
cutter <- worker()
sort(table(cutter[post_title]),decreasing = T)
```

```
## 
##       情報         被       討論         為       新聞         Re 
##         10          5          4          4          4          4 
##         的       花邊         我        NBA    2017-18 2017.10.25 
##          3          3          3          3          2          2 
##       本文         不     得分王       發展       方式       放到 
##          2          2          2          2          2          2 
##         分       改回         皇         加       今日       聯盟 
##          2          2          2          2          2          2 
##       馬刺       難以       排名       騎士       球員       刪除 
##          2          2          2          2          2          2 
##       甚麼         下         已         詹       總計     Parker 
##          2          2          2          2          2          2 
##       Tony       Wade         　         10         15     171025 
##          2          2          1          1          1          1 
##      18-31          2         26         32          9       板規 
##          1          1          1          1          1          1 
##       鮑爾       比賽         才       曾經         常       出手 
##          1          1          1          1          1          1 
##       從小         打     第三位       對戰       厄文         改 
##          1          1          1          1          1          1 
##       戈塔         哥       公佈       公告       過往       機率 
##          1          1          1          1          1          1 
##       紀錄       加盟         將       叫囂     接下來       今天 
##          1          1          1          1          1          1 
##         進         就     就會解       考慮       空位         控 
##          1          1          1          1          1          1 
##   來勢洶洶       籃網       老婆     離家近       例行       兩分 
##          1          1          1          1          1          1 
##       領軍       輪休       美媒     名人堂       目前       尼克 
##          1          1          1          1          1          1 
##         年       排在       頻繁         破       啟齒       缺席 
##          1          1          1          1          1          1 
##       融入         賽       賽程       三場   身體素質       市場 
##          1          1          1          1          1          1 
##       順位     談為何         投       韋德       為何         衛 
##          1          1          1          1          1          1 
##         習       先發       現役       小牛       心中       選到 
##          1          1          1          1          1          1 
##         以       椅子     易建聯       異動         因         有 
##          1          1          1          1          1          1 
##       怎麼         長         這       陣前       陣容     整理表 
##          1          1          1          1          1          1 
##       只能       主帥       字母       自由         最       CP25 
##          1          1          1          1          1          1 
##      Fultz   Markelle       Melo  ss2121647  Standings    TimmyDD 
##          1          1          1          1          1          1 
##         v6 
##          1
```



```r
library(wordcloud2)
```

```
## Warning: package 'wordcloud2' was built under R version 3.2.5
```

```r
wordcloud2(demoFreq, size = 1,shape = 'star')
```

```
## Error in loadNamespace(name): there is no package called 'webshot'
```

```r
#wordcloud2(demoFreq, size = 2, minRotation = -pi/6, maxRotation = -pi/6, rotateRatio = 1)
#wordcloud2(demoFreq, size = 2, minRotation = -pi/2, maxRotation = -pi/2)
```



```r
letterCloud(demoFreq, word = "R", size = 2)
```

```
## Error in loadNamespace(name): there is no package called 'webshot'
```


```r
## Sys.setlocale("LC_CTYPE","eng")

wordcloud2(demoFreqC, size = 2, fontFamily = "微软雅黑",
           color = "random-light", backgroundColor = "grey")
```

```
## Error in loadNamespace(name): there is no package called 'webshot'
```

```r
## STHeiti ??
```

> 把上述表格轉為 data frame 再 wordcloud




### 案例練習：批踢踢的？版



### 案例練習：分析魯迅的吶喊

https//www.gutenberg.org








---------
# 參考
- http://qinwenfeng.com/jiebaR/
- https://cran.r-project.org/web/packages/wordcloud2/vignettes/wordcloud.html




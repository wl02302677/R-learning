# R-learning
第一單元：商業分析、機率與程式


rm(list=ls(all=T))
knitr::opts_chunk$set(comment = NA)
knitr::opts_knit$set(global.par = TRUE)
par(cex=0.8)
options(scipen=20, digits=4, width=90)
安裝、載入一些基本的套件

if(!require(devtools)) install.packages("devtools")
if(!require(devtools)) devtools::install_github("hadley/emo")
if(!require(pacman)) install.packages("pacman")
pacman::p_load(magrittr)

【A】 商業分析的基礎 Business Analytics Fundamentals
A1. 機率與策略的期望報酬 Probability & Expected Payoff of a Strategy
Given a strategy which may lead to n different outcomes …

pi : the probability of the i-th outcome, i∈[1,n]
vi : the payoff of the i-th outcome
π : the expected payoff of the strategy
π=∑i=1npi×vi(1)
分別把報酬(vi)和機率(pi)放在v和p這兩個數值向量之中

v = c(100, 50, -50, 0)
p = c(0.1, 0.2, 0.3, 0.4)
算出π的值，放在payoff這個物件裡面，並且把它列印出來

payoff = sum(p * v)
payoff
[1] 5


A2. 策略選項與決策 Choicing from Several Startegy
Given K strategies, we look for the startegy with the largest expected payoff.

vi : the payoff of the i-th outcome
pj,i : the probability of the i-th outcome in the j-th strategy
將機率(pj,i)放在p這個數值向量之中 (重複使用同一個名稱)

v = c(100, 50, -50, 0)
p = matrix(c(
  0.1,0.2,0.3,0.4,
  0.2,0.3,0.4,0.1,
  0.1,0.1,0.1,0.5,
  0.2,0.5,0.1,0.2
  ), byrow=F, nrow=4)
πj : the expected payoff of the j-th strategy, j∈[1,K]
πj=∑i=1npj,i×vi(2)

算出π的值，放在payoff這個物件裡面，並且把它列印出來

payoff = crossprod(p, v)
payoff
     [,1]
[1,]    5
[2,]   15
[3,]   10
[4,]   40
現在payoff是一個4x1的數值矩陣

payoff = colSums(p * v)
payoff
[1]  5 15 10 40
現在payoff是一個長度為4的數值向量


【B】 R語言簡介 Introduction to R
B1. 『值』的種類 Types of Values
Examples of automic (scaler) object

noTimes = 3L                   # 整數 integer
myWeight = 75.2                # 數值 numeric
isAsian = TRUE                 # 邏輯 Boolin
myName = "Tony Chuo"           # 字串 character 
date1 = as.Date("2019-01-01")  # 日期 Date
B2. 基本資料結構 - 向量 Vector
💡: 向量是R的基本資料結構
    ※ noTime、myWeight、isAsian、myName、date1 其實都是長度為1的向量物件

Examples of Vector Object

noBuy = c(3L, 5L, 1L, 1L, 3L)               # 整數向量
height = c(175, 168, 180, 181, 169)         # 數值向量
isMale = c(FALSE, TRUE, FALSE, TRUE, TRUE)  # 邏輯向量
Example of Character Vector

name = c("Amy", "Bob", "Cindy", "Danny", "Edward")  # 字串向量
Example of Factor vector

gender = factor( c("F", "M", "F", "M", "M") )     # 類別向量
skin_color = factor( c("black", "black", "white", "yellow", "white") )
B3. 運算符號 (Operator)
💡: 早期的R，其最主要的目的就是要簡化向量運算
    ※ 四則運算和內建功能大多都可以直接作用在向量上面

c(1, 2, 3, 4) * c(1, 10, 100, 1000)
[1]    1   20  300 4000
連續的整數

1:6
[1] 1 2 3 4 5 6
次方運算和科學記號

10^(-2:3)
[1]    0.01    0.10    1.00   10.00  100.00 1000.00
當向量不一樣長時 …

c(100, 200, 300, 400) / c(10, 20)
[1] 10 10 30 20
單值：長度為1的向量

c(100, 200, 300, 400) / 10
[1] 10 20 30 40
c(10,20,30,40,50,60,70,80) + c(1, 2, 3)
Warning in c(10, 20, 30, 40, 50, 60, 70, 80) + c(1, 2, 3): 較長的物件長度並非較短物件長度
的倍數
[1] 11 22 33 41 52 63 71 82
指定物件的名稱： = 和 <- Assignment Operator

Prob = c(0.1, 0.2, 0.3, 0.4)
Value = c(120, 100, -50, -60)
Prob * Value
[1]  12  20 -15 -24
B4. 功能與參數 Function & Argument
The Expected Payoff is： ∑p×v

expPayoff = sum(Prob * Value)
expPayoff
[1] -7
內建功能通常第一個參數都是向量物件

sqrt(1:9)
[1] 1.000 1.414 1.732 2.000 2.236 2.449 2.646 2.828 3.000
💡: R的功能通常都有很多個參數，我們需要注意參數的：
    ※ 名稱
    ※ 位置
    ※ 預設值

log(1000)
[1] 6.908
help(log)
log(x=1000, base=10)
[1] 3
log(1000,10)
[1] 3
B5. 連續呼叫功能
round(sqrt(1:9), 3)
[1] 1.000 1.414 1.732 2.000 2.236 2.449 2.646 2.828 3.000
sqrt(1:9) %>% round(3) 
[1] 1.000 1.414 1.732 2.000 2.236 2.449 2.646 2.828 3.000
B6. 資料框 DataFrame
df = data.frame(
  noBuy = c(3L, 5L, 1L, 1L, 3L),
  height = c(175, 168, 180, 181, 169),
  isMale = c(FALSE, TRUE, FALSE, TRUE, TRUE),
  name = c("Amy", "Bob", "Cindy", "Danny", "Edward"),
  gender = factor( c("F", "M", "F", "M", "M") ),
  skin_color = factor( c("black", "black", "white", "yellow", "white") )
  )

df
  noBuy height isMale   name gender skin_color
1     3    175  FALSE    Amy      F      black
2     5    168   TRUE    Bob      M      black
3     1    180  FALSE  Cindy      F      white
4     1    181   TRUE  Danny      M     yellow
5     3    169   TRUE Edward      M      white
mean(df$height)
[1] 174.6
tapply(df$height, df$gender, mean)
    F     M 
177.5 172.7 




# Expression of Emotions in 20th Century Chinese Books
Ryan Cheung  
Thursday, August 07, 2014  

## Load in data 读入数据

```r
neg <- read.table("negsentiment.txt", header = T)
pos <- read.table("possentiment.txt", header = T)
total <- read.table("summaryout.txt", header = T)
```

## 计算各年的情感值 Calculate mood scores
### 以总1gram频次正则化


```r
freqneg <- neg[,2]/total[,3]
freqpos <- pos[,2]/total[,3]
sent <- freqpos - freqneg
poslm <- lm(freqpos~pos[,1])
neglm <- lm(-freqneg~neg[,1])
neglmp <- lm(freqneg~neg[,1])
sentlm <- lm(sent~pos[,1])
```
### 绘制图

负面情感词各年频次：

```r
plot(neg[,1], freqneg, pch = 19, col = 'blue')
abline(neglmp$coefficients[1],neglmp$coefficients[2], col = 'blue', lty = 2, lwd = 2)
```

![plot of chunk unnamed-chunk-3](./analysis_files/figure-html/unnamed-chunk-3.png) 

正面情感词各年频次：

```r
plot(pos[,1], freqpos, pch = 19, col = 'red')
abline(poslm$coefficients[1],poslm$coefficients[2], col = 'red', lty = 2, lwd = 2)
```

![plot of chunk unnamed-chunk-4](./analysis_files/figure-html/unnamed-chunk-4.png) 

情感值（正面情感各年频次-负面情感各年频次）随年份的变化：

```r
plot(pos[,1], sent, pch = 19, col = 'black')
abline(sentlm$coefficients[1],sentlm$coefficients[2], col = 'red', lty = 2, lwd = 2)
```

![plot of chunk unnamed-chunk-5](./analysis_files/figure-html/unnamed-chunk-5.png) 

三者放在一张图上：

```r
plot( 1900:2000, seq(-0.008,0.012,0.02/100), type = "n", xlab = "year", ylab = "sentiment") 
points(pos[,1], sent, col = 'black', pch = 19)
abline(sentlm$coefficients[1],sentlm$coefficients[2], col = 'black', lty = 2, lwd = 2)
points(neg[,1], -freqneg, col = 'blue', pch = 19)
abline(neglm$coefficients[1],neglm$coefficients[2], col = 'blue', lty = 2, lwd = 2)
points(pos[,1], freqpos, col = 'red', pch = 19)
abline(poslm$coefficients[1],poslm$coefficients[2], col = 'red', lty = 2, lwd = 2)
```

![plot of chunk unnamed-chunk-6](./analysis_files/figure-html/unnamed-chunk-6.png) 





---
title: "Expression of Emotions in 20th Century Chinese Books"
author: "Ryan Cheung"
date: "Thursday, August 07, 2014"
output:
  html_document:
    keep_md: yes
---

## Load in data ��������

```r
neg <- read.table("negsentiment.txt", header = T)
pos <- read.table("possentiment.txt", header = T)
total <- read.table("summaryout.txt", header = T)
clusters <- read.csv("Clusters.csv", header = T)
```


## �����������ֵ Calculate mood scores
### ����1gramƵ������


```r
freqneg <- neg[, 2]/total[, 3]
negzscore <- (freqneg - mean(freqneg))/sd(freqneg)
neglm <- lm(-freqneg ~ neg[, 1])
neglmp <- lm(freqneg ~ neg[, 1])
freqpos <- pos[, 2]/total[, 3]
poslm <- lm(freqpos ~ pos[, 1])
poszscore <- (freqpos - mean(freqpos))/sd(freqpos)
sent <- freqpos - freqneg
sentlm <- lm(sent ~ pos[, 1])
sentzscore <- (sent - mean(sent))/sd(sent)
df <- as.data.frame(cbind(pos[, 1], freqneg, negzscore, freqpos, poszscore, 
    sent, sentzscore, clusters[, 2]))
names(df)[1] <- "Year"
names(df)[8] <- "Cluster"
```


### ����ͼ

������ֵ�ı�׼ֵͼ��

```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.1.1
```

```r
ggplot(df, aes(x = Year, y = sentzscore)) + stat_smooth(method = "lm", formula = y ~ 
    x, size = 1, color = "black", alpha = 0.1) + geom_point(aes(color = factor(Cluster), 
    size = 5)) + labs(title = "Expression of Emotion in 20 Century Chinese Books", 
    x = "Year", y = "Emotion(Zscore)")
```

![plot of chunk Emotion](figure/Emotion.png) 



������дʸ���Ƶ�Σ�

```r
ggplot(df, aes(x = Year, y = poszscore)) + stat_smooth(method = "lm", formula = y ~ 
    x, size = 1, color = "black", alpha = 0.1) + geom_point(aes(color = factor(Cluster), 
    size = 5)) + labs(title = "Positive Emotion in 20 Century Chinese Books", 
    x = "Year", y = "Positive Emotion(Zscore)")
```

![plot of chunk Pos](figure/Pos.png) 


������дʸ���Ƶ�Σ�

```r
ggplot(df, aes(x = Year, y = negzscore)) + stat_smooth(method = "lm", formula = y ~ 
    x, size = 1, color = "black", alpha = 0.1) + geom_point(aes(color = factor(Cluster), 
    size = 5)) + labs(title = "Negitive Emotion in 20 Century Chinese Books", 
    x = "Year", y = "Negitive Emotion(Zscore)")
```

![plot of chunk Neg](figure/Neg.png) 


���߷���һ��ͼ�ϣ�

```r
plot(1900:2000, seq(-0.008, 0.012, 0.02/100), type = "n", xlab = "year", ylab = "sentiment")
points(pos[, 1], sent, col = "black", pch = 19)
abline(sentlm$coefficients[1], sentlm$coefficients[2], col = "black", lty = 2, 
    lwd = 2)
points(neg[, 1], -freqneg, col = "blue", pch = 19)
abline(neglm$coefficients[1], neglm$coefficients[2], col = "blue", lty = 2, 
    lwd = 2)
points(pos[, 1], freqpos, col = "red", pch = 19)
abline(poslm$coefficients[1], poslm$coefficients[2], col = "red", lty = 2, lwd = 2)
```

![plot of chunk All](figure/All.png) 


## ����Լ���
### �����Ƶ����

```r
posdf <- read.csv("posdf.csv", stringsAsFactors = F, header = T)
negdf <- read.csv("negdf.csv", stringsAsFactors = F, header = T)
```


### ���������

```r
poscor <- rep(0, ncol(posdf) - 1)
negcor <- rep(0, ncol(negdf) - 1)
for (i in 2:ncol(posdf)) {
    freq <- posdf[, i]/total[, 3]
    poscor[i - 1] = cor(poszscore, (freq - mean(freq))/sd(freq))
}
for (i in 2:ncol(negdf)) {
    freq <- negdf[, i]/total[, 3]
    negcor[i - 1] = cor(negzscore, (freq - mean(freq))/sd(freq))
}
```


### �ҳ����ܴ�������������ƺ͸���������ƵĴ���

```r
top10poscorscore <- sort(poscor, T)[1:10]
top10posterm <- rep("", 10)
for (i in 1:10) {
    top10posterm[i] <- names(posdf)[which(poscor == top10poscorscore[i]) + 1]
}
top10posterm
```

```
##  [1] "��"   "Ҫ"   "��"   "��"   "��"   "����" "��"   "�϶�" "����" "����"
```

```r

top10negcorscore <- sort(negcor, T)[1:10]
top10negterm <- rep("", 10)
for (i in 1:10) {
    top10negterm[i] <- names(negdf)[which(negcor == top10negcorscore[i]) + 1]
}
top10negterm
```

```
##  [1] "˵"       "����"     "����"     "����"     "����"     "ŭ���ɶ�"
##  [7] "��ѵ"     "ҡ��"     "��"       "����"
```



```r
freq <- posdf$��/total[, 3]
zscore <- (freq - mean(freq))/sd(freq)
plot(pos[, 1], poszscore, pch = 19, col = "red")
points(pos[, 1], zscore, pch = 19, col = "green")
```

![plot of chunk hao](figure/hao.png) 

```r
cor(poszscore, zscore)
```

```
## [1] 0.8661
```



```r
freq <- negdf$����/total[, 3]
zscore <- (freq - mean(freq))/sd(freq)
plot(neg[, 1], negzscore, pch = 19, col = "red")
points(pos[, 1], zscore, pch = 19, col = "purple")
```

![plot of chunk chengzhong](figure/chengzhong.png) 

```r
cor(poszscore, zscore)
```

```
## [1] 0.7628
```


### Write out sentiment data

```r
write.csv(df, "sentiment.csv")
```



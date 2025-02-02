# Solutions ch. 2 - Dimensionality reduction {#solutions-dimensionality-reduction}

Solutions to exercises of chapter \@ref(dimensionality-reduction).

## Exercise 2.5. 

We can run tSNE using the following command:


```r
set.seed(12345)
D <- read.csv(file = "data/PGC_transcriptomics/PGC_transcriptomics.csv", header = TRUE, sep = ",", row.names=1)
genenames <- rownames(D)
genenames <- genenames[4:nrow(D)]
```

This reads in the corresponding spreadsheet into the R environment as a data frame variable. 



```r
library(Rtsne)
set.seed(1)
tsne_model_1 = Rtsne(as.matrix(t(D)), check_duplicates=FALSE, pca=TRUE, perplexity=100, theta=0.5, dims=2)
```


As we did previously, we can plot the results using:


```r
y1 <- tsne_model_1$Y[which(D[1,]==-1),1:2]
y2 <- tsne_model_1$Y[which(D[1,]==0),1:2]
y3 <- tsne_model_1$Y[which(D[1,]==1),1:2]
y4 <- tsne_model_1$Y[which(D[1,]==2),1:2]

plot(y1,type="p",col="red",xlim=c(-20, 20),ylim=c(-20, 20))
points(y2,type="p",col="black")
points(y3,type="p",col="blue")
points(y4,type="p",col="green")
legend(-20, 10, legend=c("ESC", "preimp", "PGC", "soma"), col=c("red", "black", "blue", "green"),pch="o", bty="n", cex=0.8)
```

<img src="15-solutions-dimensionality-reduction_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## Exercise 2.6.

We can plot the expression patterns for pre-implantation embryos:


```r
y2_0 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==0),1:2]
y2_1 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==1),1:2]
y2_2 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==2),1:2]
y2_3 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==3),1:2]
y2_4 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==4),1:2]
y2_5 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==5),1:2]
y2_6 <- tsne_model_1$Y[which(D[1,]==0 & D[3,]==6),1:2]

plot(y2_0,type="p",col="tomato",xlim=c(-10, 10),ylim=c(-10, 10))
points(y2_1,type="p",col="tomato")
points(y2_2,type="p",col="tomato1")
points(y2_3,type="p",col="tomato1")
points(y2_4,type="p",col="tomato2")
points(y2_5,type="p",col="tomato3")
points(y2_6,type="p",col="tomato4")
legend(-10, 10, legend=c("Ooc", "Zyg", "2C", "4C","8C","Mor","Blast"), col=c("tomato", "tomato", "tomato1", "tomato1", "tomato2","tomato3","tomato4"),pch="o", bty="n", cex=0.8)
```

<img src="15-solutions-dimensionality-reduction_files/figure-html/unnamed-chunk-4-1.png" width="672" />

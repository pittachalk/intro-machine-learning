# Support vector machines {#svm}

## Introduction
Support vector machines (SVMs) are models of supervised learning, applicable to both classification and regression problems. The SVM is an extension of the support vector classifier (SVC), which is turn is an extension of the maximum margin classifier. 

### Maximum margin classifier
Let's start by definining a hyperplane. In _p_-dimensional space a hyperplane is a flat affine subspace of _p_-1. Figure \@ref(fig:svmSeparatingHyperplanes2) shows three separating hyperplanes and objects of two different classes. A separating hyperplane forms a natural linear decision boundary, classifying new objects according to which side of the line they are located.

<div class="figure" style="text-align: center">
<img src="images/svm.9.2.png" alt="Left: two classes of observations (blue, purple) and three separating hyperplanes. Right: separating hyperplane shown as black line and grid indicates decision rule. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="90%" />
<p class="caption">Left: two classes of observations (blue, purple) and three separating hyperplanes. Right: separating hyperplane shown as black line and grid indicates decision rule. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>

If the classes of observations can be separated by a hyperplane, then there will in fact be an infinite number of hyperplanes. So which of the possible hyperplanes do we choose to be our decision boundary? 

The **maximal margin hyperplane** is the separating hyperplane that is farthest from the training observations. The perpendicular distance from a given hyperplane to the nearest training observation is known as the **margin**. The maximal margin hyperplane is the separating hyperplane for which the margin is largest.

<div class="figure" style="text-align: center">
<img src="images/svm.9.3.png" alt="Maximal margin hyperplane shown as solid line. Margin is the distance from the solid line to either of the dashed lines. The support vectors are the points on the dashed line. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="75%" />
<p class="caption">Maximal margin hyperplane shown as solid line. Margin is the distance from the solid line to either of the dashed lines. The support vectors are the points on the dashed line. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>

Figure \@ref(fig:svmMaximalMarginHyperplane) shows three training observations that are equidistant from the maximal margin hyperplane and lie on the dashed lines indicating the margin. These are the **support vectors**. If these points were moved slightly, the maximal margin hyperplane would also move, hence the term *support*. The maximal margin hyperplane is set by the **support vectors** alone; it is not influenced by any other observations.

The maximal margin hyperplane is a natural decision boundary, but only if a separating hyperplane exists. In practice there may be non separable cases which prevent the use of the maximal margin classifier.
<div class="figure" style="text-align: center">
<img src="images/svm.9.4.png" alt="The two classes cannot be separated by a hyperplane and so the maximal margin classifier cannot be used. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="75%" />
<p class="caption">The two classes cannot be separated by a hyperplane and so the maximal margin classifier cannot be used. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>

## Support vector classifier
Even if a separating hyperplane exists, it may not be the best decision boundary. The maximal margin classifier is extremely sensitive to individual observations, so may overfit the training data.

<div class="figure" style="text-align: center">
<img src="images/svm.9.5.png" alt="Left: two classes of observations and a maximum margin hyperplane (solid line). Right: Hyperplane (solid line) moves after the addition of a new observation (original hyperplane is dashed line). Source: http://www-bcf.usc.edu/~gareth/ISL/" width="90%" />
<p class="caption">Left: two classes of observations and a maximum margin hyperplane (solid line). Right: Hyperplane (solid line) moves after the addition of a new observation (original hyperplane is dashed line). Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>


It would be better to choose a classifier based on a hyperplane that:

* is more robust to individual observations
* provides better classification of most of the training variables

In other words, we might tolerate some misclassifications if the prediction of the remaining observations is more reliable. The **support vector classifier** does this by allowing some observations to be on the wrong side of the margin or even on the wrong side of the hyperplane. Observations on the wrong side of the hyperplane are misclassifications.

<div class="figure" style="text-align: center">
<img src="images/svm.9.6.png" alt="Left: observations on the wrong side of the margin. Right: observations on the wrong side of the margin and observations on the wrong side of the hyperplane. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="90%" />
<p class="caption">Left: observations on the wrong side of the margin. Right: observations on the wrong side of the margin and observations on the wrong side of the hyperplane. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>

The support vector classifier has a tuning parameter, _C_, that determines the number and severity of the violations to the margin. If _C_ = 0, then no violations to the margin will be tolerated, which is equivalent to the maximal margin classifier. As _C_ increases, the classifier becomes more tolerant of violations to the margin, and so the margin widens.

The optimal value of _C_ is chosen through cross-validation.  

_C_ is described as a tuning parameter, because it controls the bias-variance trade-off:

* a small _C_ results in narrow margins that are rarely violated; the model will have low bias, but high variance.
* as _C_ increases the margins widen allowing more violations; the bias of the model will increase, but its variance will decrease.

The **support vectors** are the observations that lie directly on the margin, or on the wrong side of the margin for their class. The only observations that affect the classifier are the support vectors. As _C_ increases, the margin widens and the number of support vectors increases. In other words, when _C_ increases more observations are involved in determining the decision boundary of the classifier.

<div class="figure" style="text-align: center">
<img src="images/svm.9.7.png" alt="Margin of a support vector classifier changing with tuning parameter C. Largest value of C was used in the top left panel, and smaller values in the top right, bottom left and bottom right panels. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="75%" />
<p class="caption">Margin of a support vector classifier changing with tuning parameter C. Largest value of C was used in the top left panel, and smaller values in the top right, bottom left and bottom right panels. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>

## Support Vector Machine
The support vector classifier performs well if we have linearly separable classes, however this isn't always the case.

<div class="figure" style="text-align: center">
<img src="images/svm.9.8.png" alt="Two classes of observations with a non-linear boundary between them." width="90%" />
<p class="caption">Two classes of observations with a non-linear boundary between them.</p>
</div>

The SVM uses the **kernel trick** to operate in a higher dimensional space, without ever computing the coordinates of the data in that space.

<div class="figure" style="text-align: center">
<img src="images/svm_kernel_machine.png" alt="Kernel machine. By Alisneaky - Own work, CC0, https://commons.wikimedia.org/w/index.php?curid=14941564" width="80%" />
<p class="caption">Kernel machine. By Alisneaky - Own work, CC0, https://commons.wikimedia.org/w/index.php?curid=14941564</p>
</div>


<div class="figure" style="text-align: center">
<img src="images/svm.9.9.png" alt="Left: SVM with polynomial kernel of degree 3. Right: SVM with radial kernel. Source: http://www-bcf.usc.edu/~gareth/ISL/" width="90%" />
<p class="caption">Left: SVM with polynomial kernel of degree 3. Right: SVM with radial kernel. Source: http://www-bcf.usc.edu/~gareth/ISL/</p>
</div>



## Example - training a classifier
Training of an SVM will be demonstrated on a 2-dimensional simulated data set, with a non-linear decision boundary.

### Setup environment
Load required libraries

```r
library(caret)
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(RColorBrewer)
library(ggplot2)
library(doMC)
```

```
## Loading required package: foreach
```

```
## Loading required package: iterators
```

```
## Loading required package: parallel
```

```r
library(pROC)
```

```
## Type 'citation("pROC")' for a citation.
```

```
## 
## Attaching package: 'pROC'
```

```
## The following objects are masked from 'package:stats':
## 
##     cov, smooth, var
```

```r
library(e1071)
```

Initialize parallel processing

```r
registerDoMC(detectCores())
getDoParWorkers()
```

```
## [1] 8
```

### Partition data
Load data

```r
moons <- read.csv("data/sim_data_svm/moons.csv", header=F)
str(moons)
```

```
## 'data.frame':	400 obs. of  3 variables:
##  $ V1: num  -0.496 1.827 1.322 -1.138 -0.21 ...
##  $ V2: num  0.985 -0.501 -0.397 0.192 -0.145 ...
##  $ V3: chr  "A" "B" "B" "A" ...
```

V1 and V2 are the predictors; V3 is the class. 

Partition data into training and test set

```r
set.seed(42)
trainIndex <- createDataPartition(y=moons$V3, times=1, p=0.7, list=F)
moonsTrain <- moons[trainIndex,]
moonsTest <- moons[-trainIndex,]

summary(moonsTrain$V3)
```

```
##    Length     Class      Mode 
##       280 character character
```

```r
summary(moonsTest$V3)
```

```
##    Length     Class      Mode 
##       120 character character
```

### Visualize training data

```r
point_shapes <- c(15,17)
bp <- brewer.pal(3,"Dark2")
point_colours <- ifelse(moonsTrain$V3=="A", bp[1], bp[2])
point_shapes <- ifelse(moonsTrain$V3=="A", 15, 17)

point_size = 2

ggplot(moonsTrain, aes(V1,V2)) + 
  geom_point(col=point_colours, shape=point_shapes, 
             size=point_size) + 
  theme_bw() +
  theme(plot.title = element_text(size=25, face="bold"), axis.text=element_text(size=15),
        axis.title=element_text(size=20,face="bold"))
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/svmMoonsTrainSet-1.png" alt="Scatterplot of the training data" width="50%" />
<p class="caption">Scatterplot of the training data</p>
</div>


### Define a custom model

Caret has over two hundred built in models, including several support vector machines:
[https://topepo.github.io/caret/available-models.html](https://topepo.github.io/caret/available-models.html)

However, despite this wide range of options, you may occasionally need to define your own model. Caret does not currently have a radial SVM implemented using the [e1071 library](https://cran.r-project.org/package=e1071), so we will define one here.


```r
svmRadialE1071 <- list(
  label = "Support Vector Machines with Radial Kernel - e1071",
  library = "e1071",
  type = c("Regression", "Classification"),
  parameters = data.frame(parameter="cost",
                          class="numeric",
                          label="Cost"),
  grid = function (x, y, len = NULL, search = "grid") 
    {
      if (search == "grid") {
        out <- expand.grid(cost = 2^((1:len) - 3))
      }
      else {
        out <- data.frame(cost = 2^runif(len, min = -5, max = 10))
      }
      out
    },
  loop=NULL,
  fit=function (x, y, wts, param, lev, last, classProbs, ...) 
    {
      if (any(names(list(...)) == "probability") | is.numeric(y)) {
        out <- e1071::svm(x = as.matrix(x), y = y, kernel = "radial", 
                          cost = param$cost, ...)
      }
      else {
        out <- e1071::svm(x = as.matrix(x), y = y, kernel = "radial", 
                          cost = param$cost, probability = classProbs, ...)
      }
      out
    },
  predict = function (modelFit, newdata, submodels = NULL) 
    {
      predict(modelFit, newdata)
    },
  prob = function (modelFit, newdata, submodels = NULL) 
    {
      out <- predict(modelFit, newdata, probability = TRUE)
      attr(out, "probabilities")
    },
  predictors = function (x, ...) 
    {
      out <- if (!is.null(x$terms)) 
        predictors.terms(x$terms)
      else x$xNames
      if (is.null(out)) 
        out <- names(attr(x, "scaling")$x.scale$`scaled:center`)
      if (is.null(out)) 
        out <- NA
      out
    },
  tags = c("Kernel Methods", "Support Vector Machines", "Regression", "Classifier", "Robust Methods"),
  levels = function(x) x$levels,
  sort = function(x)
  {
    x[order(x$cost), ]
  }
)
```
Note that the radial SVM model we have defined has only one tuning parameter, cost (_C_). If we do not define the kernel parameter _gamma_, e1071 will automatically calculate it as 1/(data dimension); _i.e._ if we have 58 predictors, _gamma_ will be 1/58 = 0.01724.

### Model cross-validation and tuning
Set seeds for reproducibility. We will be trying 9 values of the tuning parameter with 10 repeats of 10 fold cross-validation, so we need the following list of seeds.

```r
set.seed(42)
seeds <- vector(mode = "list", length = 26)
for(i in 1:25) seeds[[i]] <- sample.int(1000, 9)
seeds[[26]] <- sample.int(1000,1)
```

We will pass the twoClassSummary function into model training through trainControl. Additionally we would like the model to predict class probabilities so that we can calculate the ROC curve, so we use the classProbs option.

```r
cvCtrl <- trainControl(method = "repeatedcv", 
                       repeats = 5,
                       number = 5,
                       summaryFunction = twoClassSummary,
                       classProbs = TRUE,
                       seeds=seeds)
```

We set the **method** of the **train** function to **svmRadial** to specify a radial kernel SVM. In this implementation we only have to tune one parameter, **cost**. The default grid of cost parameters start at 0.25 and double at each iteration. Choosing tuneLength = 9 will give us cost parameters of 0.25, 0.5, 1, 2, 4, 8, 16, 32 and 64. 

```r
svmTune <- train(x = moonsTrain[,c(1:2)],
                 y = moonsTrain[,3],
                 method = svmRadialE1071,
                 tuneLength = 9,
                 preProc = c("center", "scale"),
                 metric = "ROC",
                 trControl = cvCtrl)
                 
svmTune
```

```
## Support Vector Machines with Radial Kernel - e1071 
## 
## 280 samples
##   2 predictor
##   2 classes: 'A', 'B' 
## 
## Pre-processing: centered (2), scaled (2) 
## Resampling: Cross-Validated (5 fold, repeated 5 times) 
## Summary of sample sizes: 224, 224, 224, 224, 224, 224, ... 
## Resampling results across tuning parameters:
## 
##   cost   ROC        Sens       Spec     
##    0.25  0.9202041  0.8385714  0.9014286
##    0.50  0.9272449  0.8571429  0.9071429
##    1.00  0.9355612  0.8671429  0.9028571
##    2.00  0.9404592  0.8785714  0.8985714
##    4.00  0.9423469  0.8728571  0.8971429
##    8.00  0.9466837  0.8671429  0.8885714
##   16.00  0.9477041  0.8728571  0.8800000
##   32.00  0.9475000  0.8757143  0.8742857
##   64.00  0.9464796  0.8757143  0.8685714
## 
## ROC was used to select the optimal model using the largest value.
## The final value used for the model was cost = 16.
```


```r
svmTune$finalModel
```

```
## 
## Call:
## svm.default(x = as.matrix(x), y = y, kernel = "radial", cost = param$cost, 
##     probability = classProbs)
## 
## 
## Parameters:
##    SVM-Type:  C-classification 
##  SVM-Kernel:  radial 
##        cost:  16 
## 
## Number of Support Vectors:  79
```



### Prediction performance measures
SVM accuracy profile

```r
plot(svmTune, metric = "ROC", scales = list(x = list(log =2)))
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/svmAccuracyProfileMoons-1.png" alt="SVM accuracy profile for moons data set." width="80%" />
<p class="caption">SVM accuracy profile for moons data set.</p>
</div>

Predictions on test set.

```r
svmPred <- predict(svmTune, moonsTest[,c(1:2)])
confusionMatrix(svmPred, as.factor(moonsTest[,3]))
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction  A  B
##          A 59  6
##          B  1 54
##                                           
##                Accuracy : 0.9417          
##                  95% CI : (0.8835, 0.9762)
##     No Information Rate : 0.5             
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.8833          
##                                           
##  Mcnemar's Test P-Value : 0.1306          
##                                           
##             Sensitivity : 0.9833          
##             Specificity : 0.9000          
##          Pos Pred Value : 0.9077          
##          Neg Pred Value : 0.9818          
##              Prevalence : 0.5000          
##          Detection Rate : 0.4917          
##    Detection Prevalence : 0.5417          
##       Balanced Accuracy : 0.9417          
##                                           
##        'Positive' Class : A               
## 
```

Get predicted class probabilities so we can build ROC curve.

```r
svmProbs <- predict(svmTune, moonsTest[,c(1:2)], type="prob")
head(svmProbs)
```

```
##             A          B
## 8  0.03696266 0.96303734
## 12 0.89220448 0.10779552
## 13 0.91413392 0.08586608
## 14 0.03175599 0.96824401
## 16 0.92280576 0.07719424
## 18 0.94228425 0.05771575
```

Build a ROC curve.

```r
svmROC <- roc(moonsTest[,3], svmProbs[,"A"])
```

```
## Setting levels: control = A, case = B
```

```
## Setting direction: controls > cases
```

```r
auc(svmROC)
```

```
## Area under the curve: 0.9817
```

Plot ROC curve.

```r
plot(svmROC, type = "S")
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/svmROCcurveMoons-1.png" alt="SVM accuracy profile." width="80%" />
<p class="caption">SVM accuracy profile.</p>
</div>
**Sensitivity (true positive rate)**

_TPR = TP/P = TP/(TP+FN)_

**Specificity (true negative rate)**

_SPC = TN/N = TN/(TN+FP)_

Calculate area under ROC curve. 

```r
auc(svmROC)
```

```
## Area under the curve: 0.9817
```

### Plot decision boundary
Create a grid so we can predict across the full range of our variables V1 and V2.

```r
gridSize <- 150 
v1limits <- c(min(moons$V1),max(moons$V1))
tmpV1 <- seq(v1limits[1],v1limits[2],len=gridSize)
v2limits <- c(min(moons$V2), max(moons$V2))
tmpV2 <- seq(v2limits[1],v2limits[2],len=gridSize)
xgrid <- expand.grid(tmpV1,tmpV2)
names(xgrid) <- names(moons)[1:2]
```

Predict values of all elements of grid.

```r
V3 <- as.numeric(predict(svmTune, xgrid))
xgrid <- cbind(xgrid, V3)
```

Plot

```r
point_shapes <- c(15,17)
point_colours <- brewer.pal(3,"Dark2")
point_size = 2

trainClassNumeric <- ifelse(moonsTrain$V3=="A", 1, 2)
testClassNumeric <- ifelse(moonsTest$V3=="A", 1, 2)

ggplot(xgrid, aes(V1,V2)) +
  geom_point(col=point_colours[V3], shape=16, size=0.3) +
  geom_point(data=moonsTrain, aes(V1,V2), col=point_colours[trainClassNumeric],
             shape=point_shapes[trainClassNumeric], size=point_size) +
  geom_contour(data=xgrid, aes(x=V1, y=V2, z=V3), breaks=1.5, col="grey30") +
  ggtitle("train") +
  theme_bw() +
  theme(plot.title = element_text(size=25, face="bold"), axis.text=element_text(size=15),
        axis.title=element_text(size=20,face="bold"))

ggplot(xgrid, aes(V1,V2)) +
  geom_point(col=point_colours[V3], shape=16, size=0.3) +
  geom_point(data=moonsTest, aes(V1,V2), col=point_colours[testClassNumeric],
             shape=point_shapes[testClassNumeric], size=point_size) +
  geom_contour(data=xgrid, aes(x=V1, y=V2, z=V3), breaks=1.5, col="grey30") +
  ggtitle("test") +
  theme_bw() +
  theme(plot.title = element_text(size=25, face="bold"), axis.text=element_text(size=15),
        axis.title=element_text(size=20,face="bold"))
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/simDataBinClassDecisionBoundarySVM-1.png" alt="Decision boundary created by radial kernel SVM." width="50%" /><img src="05-support-vector-machines_files/figure-html/simDataBinClassDecisionBoundarySVM-2.png" alt="Decision boundary created by radial kernel SVM." width="50%" />
<p class="caption">Decision boundary created by radial kernel SVM.</p>
</div>

**Iris example**

splitting the data into training set and test set

```r
library(e1071)
library(caTools)
my.split = sample.split(iris$Species, SplitRatio = .8)
training_set = subset(iris, my.split == TRUE)
test_set = subset(iris, my.split == FALSE)
nrow(training_set)
```

```
## [1] 120
```


```r
training_set[,1:4] = scale(training_set[,1:4])
test_set[,1:4] = scale(test_set[,1:4])
classifier1 = svm(formula = Species~., data = training_set, type = 'C-classification', kernel = 'radial')
classifier2 = svm(formula = Species~ Petal.Width + Petal.Length, data = training_set, type = 'C-classification', kernel = 'radial')
```



```r
test_pred1 = predict(classifier1, type = 'response', newdata = test_set[-5])
test_pred2 = predict(classifier2, type = 'response', newdata = test_set[-5])
# Making Confusion Matrix
cm1 = table(test_set[,5], test_pred1)
cm2 = table(test_set[,5], test_pred2)
cm1 # Confusion Matrix for all parameters
```

```
##             test_pred1
##              setosa versicolor virginica
##   setosa         10          0         0
##   versicolor      0         10         0
##   virginica       0          1         9
```

```r
cm2 # Confusion Matrix for parameters being Petal Length and Petal Width
```

```
##             test_pred2
##              setosa versicolor virginica
##   setosa         10          0         0
##   versicolor      0          9         1
##   virginica       0          1         9
```

The accuracy for both model looks solid. Also notice that as we had deduced, only Petal Length and Width is important to make this model accurate and our second classifier proves it!


```r
m2 <- svm(Species~., data = iris)
plot(m2, iris, Petal.Width ~ Petal.Length,
      slice = list(Sepal.Width = 3, Sepal.Length = 4))
```

<img src="05-support-vector-machines_files/figure-html/svm.plots-1.png" width="672" />


```r
iris.part = subset(iris, Species != 'setosa')
iris.part$Species = factor(iris.part$Species)
#iris.part = iris.part[, c(1,2,5)]
svm.fit = svm(formula=Species~., data=iris.part, type='C-classification', kernel='linear')
plot(svm.fit, iris.part, Petal.Width ~ Petal.Length, slice = list(Sepal.Width = 3, Sepal.Length = 4))
```

<img src="05-support-vector-machines_files/figure-html/svm.subsets-1.png" width="672" />

## Example - regression
This example serves to demonstrate the use of SVMs in regression, but perhaps more importantly, it highlights the power and flexibility of the [caret](http://cran.r-project.org/web/packages/caret/index.html) package. Earlier we used _k_-NN for a regression analysis of the **BloodBrain** dataset (see section \@ref(knn-regression)). We will repeat the regression analysis, but this time we will fit a radial kernel SVM. Remarkably, a re-run of this analysis using a completely different type of model, requires changes to only two lines of code.

The pre-processing steps and generation of seeds are identical, therefore if the data were still in memory, we could skip this next block of code:

```r
data(BloodBrain)

set.seed(42)
trainIndex <- createDataPartition(y=logBBB, times=1, p=0.8, list=F)
descrTrain <- bbbDescr[trainIndex,]
concRatioTrain <- logBBB[trainIndex]
descrTest <- bbbDescr[-trainIndex,]
concRatioTest <- logBBB[-trainIndex]

transformations <- preProcess(descrTrain,
                              method=c("center", "scale", "corr", "nzv"),
                              cutoff=0.75)
descrTrain <- predict(transformations, descrTrain)

set.seed(42)
seeds <- vector(mode = "list", length = 26)
for(i in 1:25) seeds[[i]] <- sample.int(1000, 50)
seeds[[26]] <- sample.int(1000,1)
```

In the arguments to the ```train``` function we change ```method``` from ```knn``` to ```svmRadialE1071```. The ```tunegrid``` parameter is replaced with ```tuneLength = 9```. Now we are ready to fit an SVM model.

```r
svmTune2 <- train(descrTrain,
                 concRatioTrain,
                 method=svmRadialE1071,
                 tuneLength = 9,
                 trControl = trainControl(method="repeatedcv",
                                          number = 5,
                                          repeats = 5,
                                          seeds=seeds,
                                          preProcOptions=list(cutoff=0.75)
                                          )
)

svmTune2
```

```
## Support Vector Machines with Radial Kernel - e1071 
## 
## 168 samples
## 127 predictors
## 
## No pre-processing
## Resampling: Cross-Validated (5 fold, repeated 5 times) 
## Summary of sample sizes: 136, 134, 134, 134, 134, 134, ... 
## Resampling results across tuning parameters:
## 
##   cost   RMSE       Rsquared   MAE      
##    0.25  0.5985893  0.5052429  0.4536373
##    0.50  0.5624023  0.5369833  0.4282522
##    1.00  0.5469292  0.5454531  0.4195863
##    2.00  0.5343754  0.5632350  0.4066230
##    4.00  0.5300758  0.5694109  0.3988008
##    8.00  0.5299476  0.5695642  0.3967481
##   16.00  0.5312291  0.5676824  0.3970615
##   32.00  0.5312291  0.5676824  0.3970615
##   64.00  0.5312291  0.5676824  0.3970615
## 
## RMSE was used to select the optimal model using the smallest value.
## The final value used for the model was cost = 8.
```


```r
plot(svmTune2)
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/rmseCorSVM-1.png" alt="Root Mean Squared Error as a function of cost." width="100%" />
<p class="caption">Root Mean Squared Error as a function of cost.</p>
</div>

Use model to predict outcomes, after first pre-processing the test set.

```r
descrTest <- predict(transformations, descrTest)
test_pred <- predict(svmTune2, descrTest)
```

Prediction performance can be visualized in a scatterplot.

```r
qplot(concRatioTest, test_pred) + 
  xlab("observed") +
  ylab("predicted") +
  theme_bw()
```

<div class="figure" style="text-align: center">
<img src="05-support-vector-machines_files/figure-html/obsPredConcRatiosSVM-1.png" alt="Concordance between observed concentration ratios and those predicted by radial kernel SVM." width="80%" />
<p class="caption">Concordance between observed concentration ratios and those predicted by radial kernel SVM.</p>
</div>

We can also measure correlation between observed and predicted values.

```r
cor(concRatioTest, test_pred)
```

```
## [1] 0.7664559
```


## Further reading
[An Introduction to Statistical Learning](http://www-bcf.usc.edu/~gareth/ISL/)

## Exercises

### Exercise 1

In this exercise we will return to the cell segmentation data set that we attempted to classify using _k_-nn in section \@ref(knn-cell-segmentation) of the nearest neighbours chapter.

```r
data(segmentationData)
```

The aim of the exercise is to build a binary classifier to predict the quality of segmentation (poorly segmented or well segmented) based on the various morphological features. 

* Do not worry about feature selection, but you may want to pre-process the data. 

* Use a radial SVM model and tune over the cost function C. 

* Produce a ROC curve to show the performance of the classifier on the test set. 


Solutions to exercises can be found in appendix \@ref(solutions-svm)

Writeup
================
Zhengqi Tian
9/10/2021

Reference:<https://www.investopedia.com/terms/m/montecarlosimulation.asp>

***Introduction***
<p>

 As data scientists, we use simulation to generate approximate answer,
providing prediction. Monte Carlo simulation is a classic model for the
task. It is a fact is that there is some degree of error in a quantity
estimated. However, we find that the degree of error gets smaller as the
number of simulation replicates increase. The blog is focusing on the
investigation of the relationship between the number of replicates and
simulation errors. We try to answer which error type can be
Representative

***Concepts***
<p>

 The basis of a Monte Carlo simulation is that the probability of
varying outcomes cannot be determined by random variable interference.
Thus, the model focuses on constantly repeating random samples to
achieve certain results.

<p>

 We assume that the more simulation replicates, the Himmler the degree
of error between estimated values and True values. For statistics, we
can find the absolute error and relative error, figuring out which one
can be a better error we look for.

***Key Vocabulary Terms and Step***
<p>
 To setup the simulation, The first step is to set several true
variables we trying to observe. For this case, we set five true
underlying probabilities, as P. Beloe is the detail:
</p>

``` r
probability<-c(0.01, 0.05, 0.10, 0.25, 0.50)
probability
```

    ## [1] 0.01 0.05 0.10 0.25 0.50

<p>
  Second, we need apply some benchmarks to sign the replicate number.
the replicate number allow as to repeated simulation process again and
again,providing the estimated value, which here is estimated probability
based on the true underlying portability. To avoide the basis number, we
need apple some rule, ensuring each random repudiated number can have a
relationship. Here we apply log 2, for exmaple 2, 4,8, 16…. With the
code， we can have 14 replicated numbers:
</p>

``` r
size<-NULL
  size[1]<-2
  for(i in 2:14)(
  size[i]=size[i-1]*2
 )
  size
```

    ##  [1]     2     4     8    16    32    64   128   256   512  1024  2048  4096
    ## [13]  8192 16384

<p>
  Third, to find the random estimated probability based on the 14 x 5
factorial experiment simulation, we use binomial distribution. Applying
the following code is helpful:
</p>

    rbinom(n, size, prob)

<p>
  In the code X is the number of observations, size is the replicate
number, and the prob is the true underlying probability. To find a more
accurate estimated probability, we want to have a large number of
observation before finding the average meaning of a set of estimate P.
We apply the follow code to find the random estimated p:
</p>

    rbinom(1000, size, probability)

<p>
  Fourth, we need apply the functions of absolute error and relative
error function to find two types of errors. Calculate error as:
</p>

    Absolute Error=|p̂−p|
    and
    Relative error=|p̂−p|/p.

<p>
  Here are simulation R code chunk for errors:
</p>

    Absolute Error=mean(abs((rbinom(1000, size, probability) / size) - probability)
    and
    Relative error=mean(abs((rbinom(1000, size, probability) / size) - probability) / probability)

<p>
  Now have got all random error now. Here is the code to show all the
errs based on our factorial experiment we have
</p>

``` r
data<-data.frame(size = rep(size, length(probability)), probability = rep(probability, length(size)), Abs_Error = rep(NA, length(probability) * length(size)),Rel_Error = rep(NA, length(probability) * length(size)))

for (i in 1:nrow(data)) {
  size =data$size[i] 
  probability = data$probability[i]
  Abs=mean(abs((rbinom(1000, size, probability) / size) - probability))
  Rel= mean(abs((rbinom(1000, size, probability) / size) - probability) / probability)
  data[i, "Abs_Error"] = Abs
  data[i, "Rel_Error"] = Rel
}
set.seed(30)
data
```

    ##     size probability    Abs_Error   Rel_Error
    ## 1      2        0.01 0.0200800000 2.008000000
    ## 2      4        0.05 0.0845500000 1.525000000
    ## 3      8        0.10 0.0842000000 0.854250000
    ## 4     16        0.25 0.0854375000 0.337500000
    ## 5     32        0.50 0.0678125000 0.134437500
    ## 6     64        0.01 0.0106350000 1.062625000
    ## 7    128        0.05 0.0150234375 0.306781250
    ## 8    256        0.10 0.0148000000 0.144382812
    ## 9    512        0.25 0.0158691406 0.060937500
    ## 10  1024        0.50 0.0119580078 0.025054687
    ## 11  2048        0.01 0.0017738867 0.177378906
    ## 12  4096        0.05 0.0026568359 0.053006836
    ## 13  8192        0.10 0.0026458984 0.025946289
    ## 14 16384        0.25 0.0027340088 0.010491211
    ## 15     2        0.50 0.2655000000 0.501000000
    ## 16     4        0.01 0.0183000000 1.989000000
    ## 17     8        0.05 0.0661250000 1.301500000
    ## 18    16        0.10 0.0620750000 0.611375000
    ## 19    32        0.25 0.0595937500 0.244000000
    ## 20    64        0.50 0.0507812500 0.099687500
    ## 21   128        0.01 0.0069956250 0.695218750
    ## 22   256        0.05 0.0107585937 0.213515625
    ## 23   512        0.10 0.0108218750 0.107488281
    ## 24  1024        0.25 0.0109716797 0.042429688
    ## 25  2048        0.50 0.0087631836 0.018133789
    ## 26  4096        0.01 0.0012042969 0.121369141
    ## 27  8192        0.05 0.0018558105 0.037192871
    ## 28 16384        0.10 0.0019339233 0.018618164
    ## 29     2        0.25 0.2860000000 1.084000000
    ## 30     4        0.50 0.1895000000 0.383000000
    ## 31     8        0.01 0.0190700000 1.682500000
    ## 32    16        0.05 0.0420375000 0.870750000
    ## 33    32        0.10 0.0447062500 0.432000000
    ## 34    64        0.25 0.0401875000 0.178437500
    ## 35   128        0.50 0.0352421875 0.071046875
    ## 36   256        0.01 0.0052210938 0.508562500
    ## 37   512        0.05 0.0079015625 0.161468750
    ## 38  1024        0.10 0.0076224609 0.072160156
    ## 39  2048        0.25 0.0076367188 0.031205078
    ## 40  4096        0.50 0.0064860840 0.012598145
    ## 41  8192        0.01 0.0008953662 0.085809082
    ## 42 16384        0.05 0.0013544678 0.026837891
    ## 43     2        0.10 0.1634000000 1.613000000
    ## 44     4        0.25 0.1592500000 0.626000000
    ## 45     8        0.50 0.1296250000 0.273000000
    ## 46    16        0.01 0.0175875000 1.695750000
    ## 47    32        0.05 0.0320625000 0.643375000
    ## 48    64        0.10 0.0302406250 0.305625000
    ## 49   128        0.25 0.0308046875 0.124375000
    ## 50   256        0.50 0.0255312500 0.048460938
    ## 51   512        0.01 0.0035668750 0.353726563
    ## 52  1024        0.05 0.0056228516 0.107253906
    ## 53  2048        0.10 0.0053826172 0.051786133
    ## 54  4096        0.25 0.0053269043 0.021854492
    ## 55  8192        0.50 0.0045219727 0.008629395
    ## 56 16384        0.01 0.0006302979 0.063387451
    ## 57     2        0.05 0.0897000000 1.856000000
    ## 58     4        0.10 0.1324000000 1.339500000
    ## 59     8        0.25 0.1132500000 0.467000000
    ## 60    16        0.50 0.0972500000 0.191625000
    ## 61    32        0.01 0.0144100000 1.457250000
    ## 62    64        0.05 0.0210125000 0.433000000
    ## 63   128        0.10 0.0202750000 0.221484375
    ## 64   256        0.25 0.0212109375 0.092328125
    ## 65   512        0.50 0.0163691406 0.037058594
    ## 66  1024        0.01 0.0025144141 0.241605469
    ## 67  2048        0.05 0.0038842773 0.076136719
    ## 68  4096        0.10 0.0037412109 0.037943848
    ## 69  8192        0.25 0.0037292480 0.015375000
    ## 70 16384        0.50 0.0030062256 0.006191040

***Visualization***

<p>
 Now, thank ggplot package for giving us a quick way to visualize the
solutions. We are about have the absolute error figure and the relative
error figure.
</p>
<p>
 Here is the Absolute Error:
</p>

``` r
require(ggplot2)
```

    ## Loading required package: ggplot2

``` r
ggplot(data, aes(size, Abs_Error, color = factor(probability))) + 
  geom_point() +
  geom_line(aes(group = probability)) + 
  scale_x_continuous(trans = "log2") +
  ylab("Absolute Error")
```

![](WriteUP_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
<p>
 From the graph, we could find that the each true p’s estimate p will
have a lower absolute error when the repudiated number goes higher,
especially when the size is over 256. The absoluate error has a sharp
decrease when the replicate error increase from 2 to 64.
</p>
<p>
 Here is the related error table:
</p>

``` r
require(ggplot2)
ggplot(data, aes(size, Rel_Error, color = factor(probability))) + 
  geom_point() +
  geom_line(aes(group = probability)) + 
  scale_x_continuous(trans = "log2") +
  ylab("Related Error")
```

![](WriteUP_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
<p>

 From the graph, we could find that the each true p’s estimate p will
have a lower related error when the repudiated number goes higher.
However, different from absolute error graph. when true probability is
small, the estimated error is more likely happen even the replicated
size number applied. For example, when the true probaility is 0.01, even
the replicated simulation number is over 4096, it still have a higher
related error than true factor, 0.5, has.&gt;

***Solution***
<p>
  Now, we have both figures for Monte Carlo simulation error. Generally,
both errors have same tracks. As the replicate simulation number rise
up, the absolute error and related error go down. we have more
confidence to say that the higher replicate number allow us to have a
estimated p, which close to the true p.
</p>
<p>
 However, absolute error goes sharp down when the replicate number is
16. We can find the related error have a more smooth downside line than
absolute error has. While when the simulate amount is close to infinite,
both error will close two zero, we can find that relate error is larger
than absolute error for each replicate number. When the true p is small,
it is more likely to have a large related error than absolute error. We
can conclude that related error is more meaningful than absolute error
does for prediction.
</p>
<p>
  Thus, when we want to find a estimated value of a certain issue with a
smaller replicated number, we prefer to use related error as a benchmark
of simulation error,since it can be more accurate to define the error.
It is a economic way to get a better prediction.
</p>

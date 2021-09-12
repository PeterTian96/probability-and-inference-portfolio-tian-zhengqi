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
    ## 1      2        0.01 0.0196000000 2.392000000
    ## 2      4        0.05 0.0833500000 1.577000000
    ## 3      8        0.10 0.0855500000 0.851250000
    ## 4     16        0.25 0.0871250000 0.346500000
    ## 5     32        0.50 0.0717500000 0.141500000
    ## 6     64        0.01 0.0103993750 1.048687500
    ## 7    128        0.05 0.0160859375 0.311406250
    ## 8    256        0.10 0.0150000000 0.154007813
    ## 9    512        0.25 0.0155039062 0.060273437
    ## 10  1024        0.50 0.0123144531 0.025089844
    ## 11  2048        0.01 0.0017610156 0.180353516
    ## 12  4096        0.05 0.0028286133 0.053712891
    ## 13  8192        0.10 0.0025761230 0.026449463
    ## 14 16384        0.25 0.0026788330 0.011095215
    ## 15     2        0.50 0.2585000000 0.505000000
    ## 16     4        0.01 0.0194300000 1.968000000
    ## 17     8        0.05 0.0653750000 1.346500000
    ## 18    16        0.10 0.0643875000 0.608125000
    ## 19    32        0.25 0.0580937500 0.238750000
    ## 20    64        0.50 0.0529062500 0.100687500
    ## 21   128        0.01 0.0070490625 0.735781250
    ## 22   256        0.05 0.0110679688 0.209875000
    ## 23   512        0.10 0.0104730469 0.110320313
    ## 24  1024        0.25 0.0107470703 0.041515625
    ## 25  2048        0.50 0.0090693359 0.017265625
    ## 26  4096        0.01 0.0012210449 0.121865234
    ## 27  8192        0.05 0.0019275635 0.038503906
    ## 28 16384        0.10 0.0018366333 0.018822388
    ## 29     2        0.25 0.2805000000 1.108000000
    ## 30     4        0.50 0.1877500000 0.380500000
    ## 31     8        0.01 0.0181250000 1.852500000
    ## 32    16        0.05 0.0449875000 0.896000000
    ## 33    32        0.10 0.0416312500 0.431750000
    ## 34    64        0.25 0.0432187500 0.173937500
    ## 35   128        0.50 0.0345468750 0.073203125
    ## 36   256        0.01 0.0050060938 0.495187500
    ## 37   512        0.05 0.0072953125 0.161187500
    ## 38  1024        0.10 0.0073111328 0.073292969
    ## 39  2048        0.25 0.0075278320 0.030130859
    ## 40  4096        0.50 0.0063454590 0.012134766
    ## 41  8192        0.01 0.0009161523 0.084674316
    ## 42 16384        0.05 0.0013614380 0.028217041
    ## 43     2        0.10 0.1622000000 1.659000000
    ## 44     4        0.25 0.1552500000 0.611000000
    ## 45     8        0.50 0.1363750000 0.266750000
    ## 46    16        0.01 0.0163475000 1.704000000
    ## 47    32        0.05 0.0303875000 0.650250000
    ## 48    64        0.10 0.0301437500 0.292906250
    ## 49   128        0.25 0.0310937500 0.122593750
    ## 50   256        0.50 0.0246406250 0.049468750
    ## 51   512        0.01 0.0033714063 0.333726562
    ## 52  1024        0.05 0.0055970703 0.109339844
    ## 53  2048        0.10 0.0054346680 0.052038086
    ## 54  4096        0.25 0.0054433594 0.022829102
    ## 55  8192        0.50 0.0044478760 0.008706055
    ## 56 16384        0.01 0.0006232251 0.061282959
    ## 57     2        0.05 0.0857000000 1.782000000
    ## 58     4        0.10 0.1318500000 1.290500000
    ## 59     8        0.25 0.1201250000 0.476500000
    ## 60    16        0.50 0.1008125000 0.203250000
    ## 61    32        0.01 0.0147100000 1.491125000
    ## 62    64        0.05 0.0214468750 0.429875000
    ## 63   128        0.10 0.0210968750 0.203531250
    ## 64   256        0.25 0.0217070313 0.087109375
    ## 65   512        0.50 0.0169628906 0.035187500
    ## 66  1024        0.01 0.0024717187 0.231785156
    ## 67  2048        0.05 0.0037720703 0.078583984
    ## 68  4096        0.10 0.0036620117 0.036385742
    ## 69  8192        0.25 0.0039500732 0.015722656
    ## 70 16384        0.50 0.0030996094 0.006103882

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

Writeup
================
Zhengqi Tian
9/10/2021

Reference:<https://www.investopedia.com/terms/m/montecarlosimulation.asp>

***Introduction***
<p>

 As data scientist, we use simulation to generate approximate answer,
providing prediction. Monte Carlo simulation is a classic model for the
task. It is a fact is that there is some degree of error in a quantity
estimated. However, we find the degree of error get smaller as the
number of simulation replicates increase. The blog is focusing on the
investigation of the relationship between the number of replicates and
simulation error.

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
 To setup the simulation, The first step is to set several true variable
we trying to observe. For this case, we set five true underlying
probability, as P. The is the detail:
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
based on the true underlying portability. with the code we can have 14
replicated numbers:
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
erro function to find two types of error. Calculate error as:
</p>

    Absolute Error=|p̂−p|
    and
    Relative error=|p̂−p|/p.

<p>
  Here are simulation R code chunk for errors:
</p>

    Aabsolute Error=mean(abs((rbinom(1000, size, probability) / size) - probability)
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
    ## 1      2        0.01 0.0186400000 1.480000000
    ## 2      4        0.05 0.0813500000 1.716000000
    ## 3      8        0.10 0.0871500000 0.878000000
    ## 4     16        0.25 0.0850000000 0.329250000
    ## 5     32        0.50 0.0703437500 0.138000000
    ## 6     64        0.01 0.0105968750 1.043875000
    ## 7    128        0.05 0.0157203125 0.328156250
    ## 8    256        0.10 0.0159781250 0.149890625
    ## 9    512        0.25 0.0154003906 0.059531250
    ## 10  1024        0.50 0.0123613281 0.025859375
    ## 11  2048        0.01 0.0017399414 0.176115234
    ## 12  4096        0.05 0.0026689453 0.054467773
    ## 13  8192        0.10 0.0026230957 0.027077881
    ## 14 16384        0.25 0.0026482544 0.010378174
    ## 15     2        0.50 0.2415000000 0.509000000
    ## 16     4        0.01 0.0187600000 1.759000000
    ## 17     8        0.05 0.0651250000 1.333500000
    ## 18    16        0.10 0.0641500000 0.594875000
    ## 19    32        0.25 0.0600312500 0.241625000
    ## 20    64        0.50 0.0504843750 0.098125000
    ## 21   128        0.01 0.0071781250 0.707250000
    ## 22   256        0.05 0.0112265625 0.215640625
    ## 23   512        0.10 0.0105714844 0.105703125
    ## 24  1024        0.25 0.0103837891 0.043500000
    ## 25  2048        0.50 0.0089335937 0.017293945
    ## 26  4096        0.01 0.0012544238 0.120967773
    ## 27  8192        0.05 0.0020312500 0.038123535
    ## 28 16384        0.10 0.0017856323 0.019378784
    ## 29     2        0.25 0.2745000000 1.140000000
    ## 30     4        0.50 0.1947500000 0.389500000
    ## 31     8        0.01 0.0184800000 1.877500000
    ## 32    16        0.05 0.0446250000 0.898000000
    ## 33    32        0.10 0.0412250000 0.409187500
    ## 34    64        0.25 0.0428750000 0.171187500
    ## 35   128        0.50 0.0352031250 0.067437500
    ## 36   256        0.01 0.0048306250 0.511171875
    ## 37   512        0.05 0.0079101562 0.156218750
    ## 38  1024        0.10 0.0073869141 0.074207031
    ## 39  2048        0.25 0.0076323242 0.030582031
    ## 40  4096        0.50 0.0061535645 0.012117676
    ## 41  8192        0.01 0.0009046289 0.085591797
    ## 42 16384        0.05 0.0014091431 0.027002686
    ## 43     2        0.10 0.1542000000 1.666000000
    ## 44     4        0.25 0.1632500000 0.650000000
    ## 45     8        0.50 0.1336250000 0.264250000
    ## 46    16        0.01 0.0178450000 1.782000000
    ## 47    32        0.05 0.0304500000 0.631500000
    ## 48    64        0.10 0.0281593750 0.296781250
    ## 49   128        0.25 0.0304296875 0.119343750
    ## 50   256        0.50 0.0238828125 0.049453125
    ## 51   512        0.01 0.0033686719 0.348218750
    ## 52  1024        0.05 0.0053869141 0.110511719
    ## 53  2048        0.10 0.0053779297 0.054909180
    ## 54  4096        0.25 0.0052055664 0.021435547
    ## 55  8192        0.50 0.0043420410 0.008672852
    ## 56 16384        0.01 0.0006022437 0.061874023
    ## 57     2        0.05 0.0952000000 1.708000000
    ## 58     4        0.10 0.1349500000 1.298500000
    ## 59     8        0.25 0.1135000000 0.465500000
    ## 60    16        0.50 0.1013125000 0.199500000
    ## 61    32        0.01 0.0143550000 1.429125000
    ## 62    64        0.05 0.0217875000 0.427937500
    ## 63   128        0.10 0.0217546875 0.207906250
    ## 64   256        0.25 0.0207304688 0.086890625
    ## 65   512        0.50 0.0174472656 0.035250000
    ## 66  1024        0.01 0.0025712500 0.251437500
    ## 67  2048        0.05 0.0037935547 0.077964844
    ## 68  4096        0.10 0.0037315430 0.037280762
    ## 69  8192        0.25 0.0037858887 0.014818848
    ## 70 16384        0.50 0.0031971436 0.006030884

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

***Solution***
<p>
  Now, we have both figures for Monte Carlo simulation error. Generally,
both errors have same tracks. As the replicate number rise up, the
absolute error and related error go down. we have more confidence to say
that the higher replicate number allow us to have a estimated p, which
close to the true p.
</p>
<p>
 However, absolute error goes sharp down when the replicate number is
16. We can find the related error have a more smooth downside line than
absolute error has. As the relative error compare the absolute error
with the real value, we can find it is more accurate than absolute
error, when the replicate number is smaller. But accuracy impact will be
mitigate as the replciate number goes large.
</p>
<p>
  Thus, when we want to find a estimated value of a certain issue, if we
do not have a large replicate size, we prefer to use related error as a
benchmark of simulation error. It is a economic way to get a better
prediction.
</p>

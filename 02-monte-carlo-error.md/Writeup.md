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
</p>
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
    ## 1      2        0.01 0.0191200000 2.104000000
    ## 2      4        0.05 0.0791500000 1.644000000
    ## 3      8        0.10 0.0867000000 0.892250000
    ## 4     16        0.25 0.0829375000 0.333500000
    ## 5     32        0.50 0.0698125000 0.135562500
    ## 6     64        0.01 0.0106200000 1.056750000
    ## 7    128        0.05 0.0154796875 0.312593750
    ## 8    256        0.10 0.0143007813 0.149890625
    ## 9    512        0.25 0.0153125000 0.062656250
    ## 10  1024        0.50 0.0124560547 0.024203125
    ## 11  2048        0.01 0.0017138477 0.180521484
    ## 12  4096        0.05 0.0028039062 0.054447266
    ## 13  8192        0.10 0.0026285645 0.026864258
    ## 14 16384        0.25 0.0026712646 0.010753174
    ## 15     2        0.50 0.2440000000 0.481000000
    ## 16     4        0.01 0.0198900000 1.966000000
    ## 17     8        0.05 0.0684500000 1.339500000
    ## 18    16        0.10 0.0608375000 0.640875000
    ## 19    32        0.25 0.0620625000 0.235375000
    ## 20    64        0.50 0.0513281250 0.096062500
    ## 21   128        0.01 0.0069881250 0.713000000
    ## 22   256        0.05 0.0108218750 0.218734375
    ## 23   512        0.10 0.0101968750 0.105375000
    ## 24  1024        0.25 0.0106953125 0.042132812
    ## 25  2048        0.50 0.0084677734 0.017569336
    ## 26  4096        0.01 0.0011643457 0.126766602
    ## 27  8192        0.05 0.0018771973 0.038778809
    ## 28 16384        0.10 0.0018477295 0.019148926
    ## 29     2        0.25 0.2780000000 1.132000000
    ## 30     4        0.50 0.1882500000 0.368500000
    ## 31     8        0.01 0.0198250000 1.892000000
    ## 32    16        0.05 0.0443125000 0.881000000
    ## 33    32        0.10 0.0420687500 0.421000000
    ## 34    64        0.25 0.0436875000 0.168312500
    ## 35   128        0.50 0.0357187500 0.069265625
    ## 36   256        0.01 0.0050525000 0.500312500
    ## 37   512        0.05 0.0077550781 0.149664063
    ## 38  1024        0.10 0.0074439453 0.079376953
    ## 39  2048        0.25 0.0077451172 0.028888672
    ## 40  4096        0.50 0.0061987305 0.012314453
    ## 41  8192        0.01 0.0009032422 0.087370117
    ## 42 16384        0.05 0.0013354370 0.025954346
    ## 43     2        0.10 0.1577000000 1.602000000
    ## 44     4        0.25 0.1577500000 0.568000000
    ## 45     8        0.50 0.1396250000 0.277000000
    ## 46    16        0.01 0.0164900000 1.638750000
    ## 47    32        0.05 0.0318437500 0.643000000
    ## 48    64        0.10 0.0308656250 0.286843750
    ## 49   128        0.25 0.0305234375 0.119968750
    ## 50   256        0.50 0.0250351562 0.050117188
    ## 51   512        0.01 0.0035102344 0.343515625
    ## 52  1024        0.05 0.0053222656 0.109550781
    ## 53  2048        0.10 0.0054556641 0.052811523
    ## 54  4096        0.25 0.0056032715 0.022013672
    ## 55  8192        0.50 0.0042885742 0.008547119
    ## 56 16384        0.01 0.0006214893 0.063874512
    ## 57     2        0.05 0.0878000000 1.788000000
    ## 58     4        0.10 0.1294000000 1.298500000
    ## 59     8        0.25 0.1118750000 0.498500000
    ## 60    16        0.50 0.1014375000 0.201500000
    ## 61    32        0.01 0.0150362500 1.433000000
    ## 62    64        0.05 0.0214062500 0.441312500
    ## 63   128        0.10 0.0198921875 0.218078125
    ## 64   256        0.25 0.0203164062 0.084140625
    ## 65   512        0.50 0.0172187500 0.036386719
    ## 66  1024        0.01 0.0024749609 0.250949219
    ## 67  2048        0.05 0.0039896484 0.078255859
    ## 68  4096        0.10 0.0036512695 0.037360352
    ## 69  8192        0.25 0.0038419189 0.015919434
    ## 70 16384        0.50 0.0031599121 0.006232666

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

![](Writeup_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

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

![](Writeup_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

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

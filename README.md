# Time Series Classification with SEQL

## Description

Mr-SEQL is a time series classification software which utilizes multiple symbolic representations of time series.



## SAX

<p align="center">
<img src="figs/sax_demo.png" width="680" height="350" />
</p>

SAX is a transformation method to convert numeric vector to a symbolic representation, i.e. a sequence of symbols from a predefined alphabet *a*. SAX first computes the Piecewise Aggregate Approximation (PAA) of a time series and then transforms this approximation to a symbolic representation.

PAA reduces a time series of length L to a vector of length *l* (*l* < *L* is also the length of the symbolic sequence) by dividing the time series into equal segments. Each segment is then replaced with its mean value.

PAA followed by a discretisation step which replaces each value of the PAA with a corresponding symbol. Symbol is selected from the alphabet based on the interval in which the value falls. There are *a* intervals, as many as the size of the alphabet. Each interval is associated with a symbol from the alphabet. Assuming that the time series is normal distributed, the intervals are divided under the normal distribution (i.e. *N*(0,1)) with equal probability.

## SFA

SFA is also transforms a time series to a symbolic representation. The core differences between SAX and SFA are the choices of approximation and discretisation techniques. SFA uses a Discreet Fourier Transform (DFT) method to approximate a time series.

More information on SFA can be found here: https://github.com/patrickzib/SFA


## SEQL

The original SEQL software and its description can be found here: https://github.com/heerme/seql-sequence-learner

## Combination of SEQL and symbolic representation of time series

## Single Representation

This is our first attempt in time series classification with SEQL. SEQL learn a linear classification model from the symbolic representation of time series (either SAX or SFA).

<p align="center">
<img src="figs/sgseql.jpg" width="800" height="180" />
</p>

## Multiple Representations

SEQL can be combined with symbolic representations of multiple resolutions and multiple domains.

<p align="center">
<img src="figs/mrseql.jpg" width="700" height="350" />
</p>

## Interpretation

As our classifier is linear, the model itself is interpretable. Furthermore, we can visualize the SAX features selected by SEQL in the raw time series domain.

<p align="center">
<img src="figs/coffee_interpret.png" width="700" height="350" />
</p>

## Installation

To compile execute following commands in the src directory:

```
mkdir -p build
cd build
mkdir -p Release
cd Release
cmake -DCMAKE_BUILD_TYPE=Release ../../
make
```


## How to Use


Convert time series to multiple SAX representations:

```
./sax_convert -i Coffee_TRAIN -o sax.train
./sax_convert -i Coffee_TEST -o sax.test
```

Classify with Ensemble SEQL (./saxdir/ will store the output of the program):

```
./mr_seql -t sax.train -T sax.test -o saxdir
```

SEQL can also be used for feature selections. The command above also writes to file a list of features selected by SEQL. Following example uses sklearn Logistic Regression for classification with the selected features:

```
python mf_logreg.py saxdir
```
The steps to use SFA representation are similar. We provide in the src folder the python script that can work with the [Python port of SFA](https://github.com/sharford5/SFA_Python). To combine SFA features and SAX features for classification, simply add both directories to the above command:

```
python mf_logreg.py saxdir sfadir
```

## References

Our paper:

[Time Series Classification by Sequence Learning in All-Subsequence Space](https://ieeexplore.ieee.org/document/7930038/)

[Interpretable Time Series Classification using All-Subsequence Learning and Symbolic Representations in Time and Frequency Domains](https://arxiv.org/abs/1808.04022)

Other SEQL-based projects:

https://github.com/heerme/seql-sequence-learner

https://github.com/lnthach/SAX-SEQL

https://github.com/svgsponer/SqLoss

Read more about SAX and other time series techniques [here](http://www.cs.ucr.edu/~eamonn/). The site also hosts the popular UCR Time Series Classification Archive.

[SFA implementation by the author](https://github.com/patrickzib/SFA).

[The UEA & UCR Time Series Classification Repository](http://timeseriesclassification.com). Datasets and implementations of most state-of-the-art time series classifiers can be found here.

## Acknowledgements

This work was funded by Science Foundation Ireland (SFI).

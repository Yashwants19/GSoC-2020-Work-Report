<p align="center">
  <img width="556" height="112" src="https://github.com/Yashwants19/GSoC-2020-Work-Report/blob/master/src/gsoc.png">
</p>

My proposal for **Automatically-Generated R Bindings** was selected as an official project under **Google Summer of Code 2020**. I worked with the organisation [mlpack](https://github.com/mlpack/), under the mentorship of **Dirk Eddelbuettel** ([@eddelbuettel](https://github.com/eddelbuettel)), **James Balamuta** ([@coatless](https://github.com/coatless)) and **Ryan Curtin**([@rcurtin](https://github.com/rcurtin)).

## Goal

`mlpack` is an intuitive, fast and flexible `C++` machine learning library with bindings to other languages. It is meant to be a machine learning analog to `LAPACK`, and aims to implement a wide array of machine learning methods and functions as a "swiss army knife" for machine learning researchers. In addition to its powerful `C++` interface, `mlpack` also provides command-line programs, `Python`, `Julia` and `Go` bindings.

`R` is a programming language and free software environment for statistical computing and graphics supported by the R Foundation for Statistical Computing. The `R` language is widely used among statisticians and data miners for developing statistical software and data analysis. In this summer we aimed to implement `R-bindings` for mlpack, as with `Python`, `Julia`, and `Go` bindings for mlpack, adding a `R` interface would further enhance the library.

## R-Binding Generator
For the past 3 months, I have been implementing `R` binding generator. The generator produces two files for every binding, and it also generate proper documentation for every binding in `man/` folder:

A `.R` file: This file is the `R` interface for `R` users. It uses `Rcpp` in order to sharing data and communicate with `C++`. When generated, the `.R` file is build in the `mlpack/binding/R/mlpack/src` directory.

A `.cpp` file: This file is used as a `C++` interface. This file includes the function which can be called by `R`.
Additionally, a method specific library is created the `method_main.cpp` in order for `R` to call the `mlpack_main()` function.

## Pull Request
[R-bindings](https://github.com/mlpack/mlpack/pull/2556)

## How To Use The Binding Generator
### USING THE BINDINGS
Here's an example usage:

```R
R version 4.0.2 (2020-06-22) -- "Taking Off Again"
Copyright (C) 2020 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> library(mlpack)
> x <- matrix(rexp(1000, rate=.1), nrow=10)
> pca(x, verbose=TRUE, new_dimensionality=5)
[INFO ] Performing PCA on dataset...
[INFO ] 72.0016% of variance retained (5 dimensions).
$output
            [,1]      [,2]        [,3]       [,4]       [,5]
 [1,] 111.667141 -11.65468  12.0705689  -8.472432  19.570236
 [2,] -20.948960  14.12169 -18.4961187  27.159226  -7.702679
 [3,] -61.017751 -81.38778 -17.9509279 -20.468096  16.520563
 [4,] -18.417396  29.84052  41.0875320  -8.433435  48.420332
 [5,]  18.359201 -28.26317  -0.3566754 -58.028579  -7.920467
 [6,] -39.205519  29.39570  75.0991434   1.771608  -7.283641
 [7,]  -5.009963  45.03162 -44.8698549  -1.245450  15.697076
 [8,]  -6.844800  32.32820 -62.9257916   7.394938   5.893395
 [9,]   4.144613  14.69571   4.6391430 -18.892109 -73.807397
[10,]  17.273434 -44.10780  11.7029811  79.214330  -9.387417
```
In order to get documentation about a method, you can use:
```R
> ?mlpack::pca
```
or
```R
> library(mlpack)
> ?pca
```
Here is an example for the auto-generated R file:https://gist.github.com/4967c5939ba054bc868415d1e3e5c956

### GENERATING THE BINDING
If you want to build/generate these bindings and run them, you can follow this procedure:

* Download R from https://cran.r-project.org and install all dependencies required by the bindings using:
```sh
Rscript -e "install.packages(c('Rcpp', 'RcppArmadillo', 'RcppEnsmallen', 'BH', 'roxygen2', 'testthat'))"
```
* Configure mlpack with `-DBUILD_R_BINDINGS=ON` (optionally `-DBUILD_MARKDOWN_BINDINGS=ON` if you want to see that output).
* Build with make R, and if you want make test will test the R bindings also.
* Once the R bindings are built, then you can run them by (from the build directory) `cd src/mlpack/bindings/R/mlpack/` && `R CMD INSTALL .` then you can do `library(mlpack)` and play with it.

### INSTALL THE BINDINGS
Currently for Direct Installation you can use:
```R
 devtools::install_github('Yashwants19/RcppMLPACK')
```

From mlpack's build system you can use:
```sh
make install
```
or, Like discussed earlier after building R bindings for mlpack, you can run them using (from the build directory):
```sh
cd src/mlpack/bindings/R/mlpack/
R CMD INSTALL
```

### TESTING THE BINDINGS
Finally, to test the binding, `testthat` is used. Simply use:

```sh
ctest -R r_binding_test --verbose
```

## Other Pull Request during GSoC
[Force CMake to show error when it didn't find python/modules.](https://github.com/mlpack/mlpack/pull/2568)

[(GSoC Week 11 -12) Refactor ProgramInfo() to separate out all the different information.](https://github.com/mlpack/mlpack/pull/2558)

[Fix ModelType Julia Documentation. ](https://github.com/mlpack/mlpack/pull/2530)

[Fix Bad Regex from CLI11. ](https://github.com/mlpack/mlpack/pull/2520)

[Fix pointer error in Go-bindings ](https://github.com/mlpack/mlpack/pull/2483)

[:rocket: Add Go bindings for some missed models.](https://github.com/mlpack/mlpack/pull/2460)

## Blog
You can follow my weekly progress on my medium blogs. [Medium Articles by yashwantsingh.sngh](https://medium.com/@yashwantsingh.sngh)

## Acknowledgements
I have no words to describe how grateful I am to my mentors -  [Dirk Eddelbuettel](https://github.com/eddelbuettel), [James Balamuta](https://github.com/coatless) and [Ryan Curtin](https://github.com/rcurtin) - for all their support and for being extremely responsive and helpful with every one of my problems. Without their vote of confidence, this project would've been a lot harder and a lot less fun to do.

I am also thankful to `mlpack` and `Google` for the opportunity to work on this project, which helped me learn a lot in such a short period of time.

---
layout: archive
title: "Code"
permalink: /code/
author_profile: true
---

{% include base_path %}

## Post-selection inference in Python
I wrote a Python port, [available on GitHub][hdmpy_github], of parts of the excellent [R package `hdm`][hdm_cran] by Victor Chernozhukov, Chris Hansen and Martin Spindler. None of the theory or the original code are mine; all credit goes to the authors. All I did was port parts of it into Python and enable parallel processing. This can make the Python port much faster than the original R package for anything which involves simulations, for example data dependent penalty terms for robust LASSO.

## Linear models in Python
I wrote a Python package, [available on GitHub][lmpy_github], to run linear models in Python. It is in many ways similar to (and will often miss functionality provided by) existing packages, such as [`statsmodels`][statsmodels] or [`scikit-learn`'s linear regression][sklearn_linear_regression], but contains some additional capabilites, such as some bootstrap algorithms (e.g. the wild bootstrap with the ability to impose the null, and the cluster robust bootstrap from [Cameron, Gelbach, and Miller, 2008][cgm2008]). It also contains the ability to drop variables based on their variance inflation factors, mostly to mirror functionality provided by STATA.

## Covariance matrices for `fastglm`
I modified the R package [fastglm][fastglm_github] by Jared Huling, adding a `vcov()` method to get the estimated covariance matrix of the coefficients. (The original package provides standard errors, but not the whole covariance matrix.) The code is [available on GitHub][fastglm_fork_github]. You can install my port of the package using
```r
library(devtools)
install_github("maxhuppertz/fastglm")
```
The `vcov()` method works just like its counterpart in `glm`. For an estimated `model <- fastglm(x, y, ...)`, just call `vcov(model)` to retrieve it. This works for all decomposition methods used in the package, though setting `method = 2` or `method = 3` (standard and robust Cholesky decomposition) is more efficient. The reason is that those methods already decompose the gram matrix `X^T * X` to estimate coefficients, whereas for other methods that has to be done as an extra step when calculating the covariance matrix. I recommend you only use my version of the package if you actually need the covariance matrix, since it's slightly slower and uses more memory than the original.

[cgm2008]: https://www.mitpressjournals.org/doi/10.1162/rest.90.3.414
[fastglm_github]: https://github.com/jaredhuling/fastglm
[fastglm_fork_github]: https://github.com/maxhuppertz/fastglm
[hdmpy_github]: https://github.com/maxhuppertz/hdmpy
[hdm_cran]: https://cran.r-project.org/web/packages/hdm/index.html
[lmpy_github]: https://github.com/maxhuppertz/lmpy
[sklearn_linear_regression]: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html
[statsmodels]: https://www.statsmodels.org/stable/index.html

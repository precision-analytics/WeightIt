WeightIt News and Updates
======

Version 0.7.1

* Fixed bug when using `weightit()` inside another function that passed a `by` argument explcitly. Also changed teh syntax for `by`; it must now either be a string which was always possible) or a one-sided formula with the stratifying variable on the right-hand side. To use a variable that is not in `data`, you must use the formula interface. 

Version 0.7.0

* Added new `sbps()` function for estimating subgroup balancing propensity score weights, including both the standard method and a new smooth version.

* Setting `method = "gbm"` and `method = "twang"` will now do two different things. `method = "gbm"` uses `gbm` and `cobalt` functions to estimate the weights and is much faster, while `method = "twang"` uses `twang` functions to estimate the weights. The results are similar between the two methods. Prior to this version, `method = "gbm"` and `method = "twang"` both did what `method = "twang"` does now. 

* Bug fixes when `stabilize = TRUE`, thanks to @ulriksartipy and Sven Rieger.

* Fixes for using `base.weight` argument with `method = "ebal"`. Now the supplied vector should have a length equal to the number of units in the dataset (in contrast to its use in `ebalance`, which requires a length equal to the number of control units).

* Restored dependency on `cobalt` for examples and vignette.

* When `method = "ps"` and the treatment is ordered (i.e., ordinal), `MASS::polr()` is used to fit an ordinal regression. Make the treatment un-ordered to to use multinomial regression instead.

* Added support for using bias-reduced fitting functions when `method = "ps"` as provided by the `brglm2` package. These can be accessed by changing the `link` to, for example, `"br.logit"` or `"br.probit"`. For multinomial treatments, setting `link = "br.logit"` fits a bias-reduced multinomial regression model using `brglm2::brmultinom()`. This can be helpful when regular maximum likelihood models fail to converge, though this may also be a sign of lack of overlap.

Version 0.6.0

* Bug fixes. Functions now work better when used inside other functions (e.g., `lapply`).

* Behavior of `weightit()` in the presence of non-`NULL` `focal` has changed. When `focal` is specified, `estimand` is assumed to be `ATT`. Previously, `focal` would be ignored unless `estimand = "ATT"`.

* Processing of `estimand` and `focal` is improved. Functions are smarter about guessing which group is the focal group when one isn't specified, especially with non-numeric treatments. `focal` can now be used with `estimand = "ATC"` to indicate which group is the control group, so `"ATC"` and `"ATT"` now function more similarly. 

* Added function `get_w_from_ps()` to transform propensity scores into weights (instead of having to go through `weightit()`).

* Added functions `as.weightit()` and `as.weightitMSM()` to convert weights and treatments and other components into `weightit` objects so that `summary.weightit()` can be used on them.

* Updated documentation to describe how missing data in the covariates is handled. Some bugs related to missing data have been fixed as well, thanks to Yong Hao Pua.

* `ps.cont()` had the "z-transformed correlation" options removed to simplify output. This function and its supporting functions will be deprecated as soon as the new version of `twang` is released.

* When using `method = "ps"` or `method = "super"` with continuous treatments, setting `use.kernel = TRUE` and `plot = TRUE`, the plot is now made with `ggplot2` rather than the base R plots.

* Added `plot.summary.weightit()` to plot the distribution of weights (a feature also in `optweight`).

* Removed dependency on `cobalt` temporarily, which means the examples and vignette won't run. 

* Added `ggplot2` to Imports.

Version 0.5.1

* Fixed a bug when using the `ps` argument in `weightit()`.

* Fixed a bug when setting `include.obj = TRUE` in `weightitMSM()`.

* Added warnings for using certain methods with longitudinal treatments as they are not validated and may lead to incorrect inferences.

Version 0.5.0

* Added `super` method to estimate propensity scores using the `SuperLearner` package.

* Added `optweight` method to estimate weights using optimization (but you should probably just use the `optweight` package).

* `weightit()` now uses the correct formula to estimate weights for the ATO with multinomial treatments as described by Li & Li (2018).

* Added `include.obj` option in `weightit()` and `weightitMSM()` to include the fitted object in the output object for inspection. For example, with `method = "ps"`, the `glm` object containing the propensity score model will be included in the output.

* Rearranged the help pages. Each method now has its own documentation page, linked from the `weightit` help page.

* Propensity scores are now included in the output for binary tretaments with `gbm` and `cbps` methods. Thanks to @Blanch-Font for the suggestion.

* Other bug fixes and minor changes.

Version 0.4.0

* Added `trim()` function to trim weights.

* Added `ps.cont()` function, which estimates generalized propensity score weights for continuous treatments using generalized boosted modeling, as in `twang`. This function uses the same syntax as `ps()` in `twang`, and can also be accessed using `weightit()` with `method = "gbm"`. Support functions were added to make it compatible with `twang` functions for assessing balance (e.g., `summary`, `bal.table`, `plot`). Thanks to Donna Coffman for enlightening me about this method and providing the code to implement it.

* The input formula is now much more forgiving, allowing objects in the environment to be included. The `data` argument to `weightit()` is now optional. To simplify things, the output object no longer contains a `data` field.

* Under-the-hood changes to facilitate adding new features and debugging. Some aspects of the output objects have been slightly changed, but it shouldn't affect use for most users.

* Fixed a bug where variables would be thrown out when `method = "ebal"`.

* Added support for sampling weights with stable balancing weighting and empirical balancing calibration weighting.

Version 0.3.2

* Added new `moments` and `int` options for some `weightit()` methods to easily specify moments and interactions of covariates.

* Fixed bug when using objects not in the data set in `weightit()`. Behavior has changed to include transformed covariates entered in formula in `weightit()` output.

* Fixed bug resulting from potentially colinearity when using `ebal` or `ebcw`.

* Added a vignette.

Version 0.3.1

* Edits to code and help files to protect against missing `CBPS` package.

* Corrected sampling weights functionality so they work correctly. Also expanded sampling weights to be able to be used with all methods, including those that do not natively allow for sampling weights (e.g., `sbw` and `ATE`)

* Minor bug fixes and spelling corrections.

Version 0.3.0

* Added `weightitMSM()` function (and supporting `print()` and `summary()` functions) to estimate weights for marginal structural models with time-varying treatments and covariates.

* Fixed some bugs, including when using CBPS with continuous treatments, and when using `focal` incorrectly.

Version 0.2.0

* Added `method = "sbw"` for stable balancing weights (now removed and replaced with `method = "optweight"`)

* Allowed for estimation of multinomial propensity scores using multiple binary regressions if `mlogit` is not installed

* Allowed for estimation of multinomial CBPS using multiple binary CBPS for more than 4 groups

* Added README and NEWS

Version 0.1.0

* First version!

--- 
 
# required metadata 
title: "rx_btrees: Fits stochastic gradient boosted decision trees (revoscalepy)" 
description: "Fit stochastic gradient boosted decision trees on an .xdf file or data frame for small or large data using parallel external memory algorithm." 
keywords: "learner, tree" 
author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# rx_btrees


 


## Usage



```
revoscalepy.rx_btrees(formula, data, output_file=None, write_model_vars=False,
    overwrite=False, pweights=None, fweights=None, cost=None, min_split=None,
    min_bucket=None, max_depth=1, cp=0, max_compete=0, max_surrogate=0,
    use_surrogate=2, surrogate_style=0, n_tree=10, m_try=None, replace=False,
    strata=None, sample_rate=None, importance=False, seed=None,
    loss_function='bernoulli', learning_rate=0.1, max_num_bins=None,
    max_unordered_levels=32, remove_missings=False, compute_obs_node_id=None,
    use_sparse_cube=False, find_splits_in_parallel=True, row_selection=None,
    transforms=None, transform_objects=None, transform_function=None,
    transform_variables=None, transform_packages=None, blocks_per_read=1,
    report_progress=2, verbose=0, compute_context=None, xdf_compression_level=1,
    **kwargs)
```





## Description

Fit stochastic gradient boosted decision trees on an .xdf file or data frame for
small or large data using parallel external memory algorithm.


## Arguments


### formula

Statistical model using symbolic formulas.


### data

Either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_file

Either an RxXdfData data source object or a character
string specifying the ‘.xdf’ file for storing the resulting node indices.
If None, then no node indices are stored to disk. If the input data is a
data frame, the node indices are returned automatically.


### write_model_vars

Bool value. If True, and the output file is
different from the input file, variables in the model will be written to
the output file in addition to the node numbers. If variables from the
input data set are transformed in the model, the transformed variables will
also be written out.


### overwrite

Bool value. If True, an existing output_file with an
existing column named outColName will be overwritten.


### pweights

Character string specifying the variable of numeric values
to use as probability weights for the observations.


### fweights

Character string specifying the variable of integer values
to use as frequency weights for the observations.


### method

Character string specifying the splitting method. Currently,
only “class” or “anova” are supported. The default is “class” if the
response is a factor, otherwise “anova”.


### parms

Optional list with components specifying additional
parameters for the “class” splitting method, as follows:

prior: A vector of prior probabilities. The priors must be positive and
    sum to 1. The default priors are proportional to the data counts.

loss: A loss matrix, which must have zeros on the diagonal and positive
    off-diagonal elements. By default, the off-diagonal elements are set to 1.

split: The splitting index, either gini (the default) or information.

If parms is specified, any of the components can be specified or
    omitted. The defaults will be used for missing components.


### cost

A vector of non-negative costs, containing one element for
each variable in the model. Defaults to one for all variables. When
deciding which split: to choose, the improvement on splitting on a variable
is divided by its cost.


### min_split

The minimum number of observations that must exist in a
node before a split is attempted. By default, this is sqrt(num of obs). For
non-XDF data sources, as (num of obs) is unknown in advance, it is wisest
to specify this argument directly.


### min_bucket

The minimum number of observations in a terminal node
(or leaf). By default, this is min_split/3.


### cp

Numeric scalar specifying the complexity parameter. Any split
that does not decrease overall lack-of-fit by at least cp is not attempted.


### max_compete

The maximum number of competitor splits retained in the
output. These are useful model diagnostics, as they allow you to compare
splits in the output with the alternatives.


### max_surrogate

The maximum number of surrogate splits retained in
the output. See the Details for a description of how surrogate splits are
used in the model fitting. Setting this to 0 can greatly improve the
performance of the algorithm; in some cases almost half the computation
time is spent in computing surrogate splits.


### use_surrogate

An integer specifying how surrogates are to be used
in the splitting process:
0: Display-only; observations with a missing value for the primary split variable are not sent further down the tree.

1: Use surrogates, in order, to split observations missing the primary split variable. If all surrogates are missing, the observation is not split.

2: Use surrogates, in order, to split observations missing the primary split variable. If all surrogates are missing or max_surrogate=0, send the observation in the majority direction.

The 0 value corresponds to the behavior of the tree function, and 2 (the default) corresponds to the recommendations of Breiman et al.


### surrogate_style

An integer controlling selection of a best
surrogate. The default, 0, instructs the program to use the total number of
correct classifications for a potential surrogate, while 1 instructs the
program to use the percentage of correct classification over the
non-missing values of the surrogate. Thus, 0 penalizes potential surrogates
with a large number of missing values.


### n_tree

A positive integer specifying the number of trees to grow.


### m_try

A positive integer specifying the number of variables to
sample as split candidates at each tree node. The default values are
sqrt(num of vars) for classification and (num of vars)/3 for regression.


### replace

A bool value specifying if the sampling of observations
should be done with or without replacement.


### cutoff

(Classification only) A vector of length equal to the number
of classes specifying the dividing factors for the class votes. The default
is 1/(num of classes).


### strata

A character string specifying the (factor) variable to use
for stratified sampling.


### sample_rate

A scalar or a vector of positive values specifying the
percentage(s) of observations to sample for each tree:
for unstratified sampling: A scalar of positive value specifying the percentage of observations to sample for each tree. The default is 1.0 for sampling with replacement (i.e., replace=True) and 0.632 for sampling without replacement (i.e., replace=False).

for stratified sampling: A vector of positive values of length equal to
    the number of strata specifying the percentages of observations to
    sample from the strata for each tree.


### importance

A bool value specifying if the importance of
predictors should be assessed.


### seed

An integer that will be used to initialize the random number
generator. The default is random. For reproducibility, you can specify the
random seed either using set.seed or by setting this seed argument as part
of your call.


### compute_oob_error

An integer specifying whether and how to compute
the prediction error for out-of-bag samples:
<0: never. This option may reduce the computation time.
=0: only once for the entire forest.
>0: once for each addition of a tree. This is the default.


### loss_function

Character string specifying the name of the loss function
to use. The following options are currently supported:
“gaussian”: Regression: for numeric responses.
“bernoulli”: Regression: for 0-1 responses.
“multinomial”: Classification: for categorical responses with two or more levels.


### learning_rate

Numeric scalar specifying the learning rate of the boosting procedure.


### max_num_bins

The maximum number of bins to use to cut numeric data.
The default is min(1001, max(101, sqrt(num of obs))). For non-XDF data
sources, as (num of obs) is unknown in advance, it is wisest to specify
this argument directly. If set to 0, unit binning will be used instead of
cutting. See the ‘Details’ section for more information.


### max_unordered_levels

The maximum number of levels allowed for an
unordered factor predictor for multiclass (>2) classification.


### remove_missings

Bool value. If True, rows with missing values
are removed and will not be included in the output data.


### use_sparse_cube

Bool value. If True, sparse cube is used.


### find_splits_in_parallel

bool value. If True, optimal splits for
each node are determined using parallelization methods; this will typically
speed up computation as the number of nodes on the same level is increased.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

None. Not currently supported, reserved for future use.


### transform_function

Variable transformation function. The variables used
in the transformation function must be specified in transform_variables if they
are not variables used in the model.


### transform_variables

List of strings of input data set variables needed
for the transformation function.


### transform_packages

None. Not currently supported, reserved for future use.


### blocks_per_read

Number of blocks to read for each chunk of data read from
the data source.


### report_progress

integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### compute_context

A RxComputeContext object for prediction.


### kwargs

Additional parameters


## Returns

A RxDForestResults object of dtree model.


## See also

[`rx_predict`](rx-predict.md),
[`rx_predict_rx_dforest`](rx-predict-rx-dforest.md).


## Example



```
import os
from revoscalepy import rx_btrees, rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)

# classification
form = "Kyphosis ~ Age + Number + Start"
seed = 0
n_trees = 50
interaction_depth = 4
n_min_obs_in_node = 1
shrinkage = 0.1
bag_fraction = 0.5
distribution = "bernoulli"

btree = rx_btrees(form, kyphosis, loss_function=distribution, n_tree=n_trees, learning_rate=shrinkage,
                sample_rate=bag_fraction, max_depth=interaction_depth, min_bucket=n_min_obs_in_node, seed=seed,
                replace=False, max_num_bins=200)

# regression
ds = RxXdfData(os.path.join(sample_data_path, "airquality.xdf"))
df = rx_import(input_data = ds)
df = df[df['Ozone'] != -2147483648]

form = "Ozone ~ Solar.R + Wind + Temp + Month + Day"
ntrees = 50
interaction_depth = 3
min_obs_in_node = 1
shrinkage = 0.1
bag_fraction = 0.5
distribution = "gaussian"

btree = rx_btrees(form, df, loss_function=distribution, n_tree=ntrees, learning_rate=shrinkage,
                  sample_rate=bag_fraction, max_depth=interaction_depth, min_bucket=min_obs_in_node, seed=0,
                  replace=False, max_num_bins=200, verbose=2)
```


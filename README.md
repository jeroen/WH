
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Whittaker-Henderson smoothing

## What is Whittaker-Henderson smoothing ?

<!-- badges: start -->

[![R-CMD-check](https://github.com/GuillaumeBiessy/WH/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/GuillaumeBiessy/WH/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

### Origin

The Whittaker-Henderson (WH) smoothing is a graduation method which
attenuates the impact of sample fluctuations. Initially introduced by
Whittaker (1922) for the construction of mortality tables and improved
by the work of Henderson (1924), it remains to date one of the most
popular smoothing method among actuaries working on the modelling of
person insurance risks such as death, disability, long-term care and
unemployement. Whittaker-Henderson smoothing generalizes to
two-dimension smoothing but requires in all cases that the observation
be equally spaced on each dimension.

### The one-dimension case

Let $y$ be an observation vector and $w \ge 0$ a vector of positive
weights, both of size $n$. The estimator associated with WH smoothing
is:

$$\hat{y} = \underset{\theta}{\text{argmin}}\{F(y,w,\theta) + R_{\lambda,q}(\theta)\}$$

where:

-   $F(y,w,\theta) = \underset{i = 1}{\overset{n}{\sum}} w_i(y_i - \theta_i)^2$
    is a fidelity criterion and

-   $R(\theta,\lambda,q) = \lambda \underset{i = 1}{\overset{n - q}{\sum}} (\Delta^q\theta)_i^2$
    a regularity criterion

$\Delta^q$ represents in the above expression the difference operator of
order $q$ such that for all $i\in[1,n - q]$:

$$(\Delta^q\theta)_i = \underset{k = 0}{\overset{q}{\sum}} \begin{pmatrix}q \\ k\end{pmatrix}(- 1)^{q - k} \theta_{i + k}.$$

Let us define $W = \text{Diag}(w)$ the diagonal matrix of weights and
$D_{n,q}$ the matrix of differences of order $q$, of dimensions
$(n - q,n)$, such that $(D_{n,q}\theta)_i = (\Delta^q\theta)_i$.
Actually, Only differences of order $q\in\{1,2\}$ ar of practical
interest. The associated difference matrices are represented below:

$$
D_1 = \begin{pmatrix}
1 & - 1 &  & 0 \\
& \ddots & \ddots & \\
0 & & 1 & - 1
\end{pmatrix}
\quad\quad
D_2 = \begin{pmatrix}
1 & - 2 & 1 & & 0 \\
& \ddots & \ddots & \ddots & \\
0 & & 1 & - 2 & 1
\end{pmatrix}.
$$

The fidelity and regularity criteria may be rewritten using matrix
notations:

$$\begin{aligned}
F(y,w,\theta) &= \underset{i = 1}{\overset{n}{\sum}} w_i(y_i - \theta_i)^2 = \|\sqrt{W}(y - \theta)\|^2 = (y - \theta)^TW(y - \theta) \\
R(\theta,\lambda,q) &= \lambda \underset{i = 1}{\overset{n - q}{\sum}} (\Delta^q\theta)_i^2 = \lambda\|D_{n,q}\theta\|^2 = \lambda\theta^TD_{n,q}^TD_{n,q}\theta
\end{aligned}$$

and the associated estimator becomes:

$$\hat{y} = \underset{\theta}{\text{argmin}} \left\lbrace(y - \theta)^TW(y - \theta) + \theta^TP_\lambda\theta\right\rbrace$$

by noting in the one-dimension case
$P_\lambda = \lambda D_{n,q}^TD_{n,q}$.

### The two-dimension case

In the two-dimension case, we start from an observation matrix $Y$ and
an associated matrix $\Omega$ of positive weights, both of dimensions
$n_x \times n_z$.

The estimator associated with Whittaker-Henderson smoothing in this case
reads:

$$\widehat{Y} = \underset{\Theta}{\text{argmin}}\{F(Y,\Omega, \Theta) + R_{\lambda,q}(\Theta)\}$$

where:

$$
\begin{aligned}
F(Y,\Omega, \Theta) &= \underset{i = 1}{\overset{n_x}{\sum}}\underset{j = 1}{\overset{n_z}{\sum}} \Omega_{i,j}(Y_{i,j} - \Theta_{i,j})^2\quad \text{is a fidelity criterion} \\
R(\Theta,\lambda,q) &= \lambda_x \underset{j = 1}{\overset{n_z}{\sum}}\underset{i = 1}{\overset{n_x - q_x}{\sum}} (\Delta^{q_x}\Theta_{\bullet,j})_i^2 + \lambda_z \underset{i = 1}{\overset{n_x}{\sum}}\underset{j = 1}{\overset{n_z - q_z}{\sum}} (\Delta^{q_z}\Theta_{i,\bullet})_j^2 \quad \text{is a regularity criterion.}
\end{aligned}
$$

The latter criterion may be decomposed as the sum of:

-   a one-dimension regularity criterion applied to all rows of $\Theta$
    and

-   a one-dimension regularity criterion applied to all columns of
    $\Theta$.

Once again it is more convenient to adopt matrix notations by defining
$y = \text{vec}(Y)$, $w = \text{vec}(\Omega)$,
$\theta = \text{vec}(\Theta)$ the vectors obtained by concatenating the
columns of $Y$, $\Omega$ and $\Theta$ respectively and by noting
$W = \text{Diag}(w)$ and $n = n_x \times n_z$.

The fidelity and regularity criteria may this be expressed as funuctions
of $y$, $w$ and $\theta$, using matrix notations:

$$\begin{aligned}
F(y,w, \theta) &= \underset{i = 1}{\overset{n}{\sum}}w_i(y_i - \theta_i)^2 = (y - \theta)^TW(y - \theta) \\
R(\theta,\lambda,q) &= \theta^{T}(\lambda_x I_{n_z} \otimes D_{n_x,q_x}^{T}D_{n_x,q_x} + \lambda_z D_{n_z,q_z}^{T}D_{n_z,q_z} \otimes I_{n_x}) \theta
\end{aligned}$$

which also leads to the equation:

$$\hat{y} = \underset{\theta}{\text{argmin}} \left\lbrace(y - \theta)^TW(y - \theta) + \theta^TP_\lambda\theta\right\rbrace$$

except in this case:

$$P_\lambda = \lambda_x I_{n_z} \otimes D_{n_x,q_x}^{T}D_{n_x,q_x} + \lambda_z D_{n_z,q_z}^{T}D_{n_z,q_z} \otimes I_{n_x}.$$

## Installation

You can install the development version of WH from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("GuillaumeBiessy/WH")
```

## How to use the package ?

The `WH` package features two main functions `WH_1d` and `WH_2d`
corresponding to the one-dimension and two-dimension cases respectively.
There are only two arguments for those functions:

-   The vector (or matrix in the two-dimension case) `d` corresponding
    to the number of observed events of interest by age (or by age and
    duration in the two-dimension case). `d` should have named elements
    (or rows and columns) for the model results to be extrapolated.

-   The vector (or matrix in the two-dimension case) `ec` corresponding
    to the portfolio central exposure by age (or by age and duration in
    the two-dimension case) whose dimensions should match those of `d`.
    The contribution of each individual to the portfolio central
    exposure corresponds to the time the individual was actually
    observed with corresponding age (and duration in the two-dimension
    cas). It always ranges from 0 to 1 and is affected by individuals
    leaving the portfolio, no matter the cause, as well as censoring and
    truncating phenomena.

Additional arguments are described in the documentations of those
functions.

The package also embed two fictive agregated datasets to illustrate how
to use it:

-   `portfolio_mortality` contains the agregated number of deaths and
    associated central exposure by age for an annuity portfolio.

-   `portfolio_LTC` contains the agregated number of deaths and
    associated central exposure by age and duration (in years) since the
    onset of LTC for the annuitant database of a long-term care
    portfolio.

``` r
# One-dimension case
keep <- which(portfolio_mort$ec > 0) # observations with no data
d <- portfolio_mort$d[keep]
ec <- portfolio_mort$ec[keep]

WH_1d_fit <- WH_1d(d, ec)
```

``` r
# Two_dimension case
keep_age <- which(rowSums(portfolio_LTC$ec) > 1e2)
keep_duration <- which(colSums(portfolio_LTC$ec) > 1e2)

d  <- portfolio_LTC$d[keep_age, keep_duration]
ec <- portfolio_LTC$ec[keep_age, keep_duration]

WH_2d_fit <- WH_2d(d, ec)
```

Functions `WH_1d` and `WH_2d` output objects of class `"WH_1d"` and
`"WH_2d"` to which additional functions (including generic S3 methods)
may be applied:

-   The `print` function provides a glimpse of the fitted results

``` r
WH_1d_fit

An object fitted using the WH_1D function
Initial data contains 74 data points
Optimal smoothing parameter selected: 22770 
Associated degrees of freedom: 7.659107 
WH_2d_fit

An object fitted using the WH_2D function
Initial data contains 176 data points
Optimal smoothing parameters selected: 126.18 
 Optimal smoothing parameters selected:   9.43 
Associated degrees of freedom: 12.22664 
```

-   The `plot` function generates rough plots of the model fit, the
    associated standard deviation, the model residuals or the associated
    degrees of freedom. See the `plot.WH_1d` and `plot.WH_2d` functions
    help for more details.

``` r
plot(WH_1d_fit)
```

<img src="man/figures/README-plot-1.png" width="100%" />

``` r
plot(WH_1d_fit, "res")
```

<img src="man/figures/README-plot-2.png" width="100%" />

``` r
plot(WH_1d_fit, "edf")
```

<img src="man/figures/README-plot-3.png" width="100%" />

``` r

plot(WH_2d_fit)
```

<img src="man/figures/README-plot-4.png" width="100%" />

``` r
plot(WH_2d_fit, "std_y_hat")
```

<img src="man/figures/README-plot-5.png" width="100%" />

-   The `predict` function generates an extrapolation of the model. It
    requires a `newdata` argument, a named list with one or two elements
    corresponding to the positions of the new observations. In the
    two-dimension case constraints are used so that the predicted values
    matches the fitted values for the initial observations (see
    Carballo, Durbán, and Lee 2021 to understand why this is required).

``` r
WH_1d_fit |> predict(newdata = 18:99) |> plot()
```

<img src="man/figures/README-predict-1.png" width="100%" />

``` r
WH_2d_fit |> predict(newdata = list(age = 50:99,
                                    duration = 0:19)) |> plot()
```

<img src="man/figures/README-predict-2.png" width="100%" />

-   Finally the `output_to_df` converts an `"WH_1d"` or `"WH_2d"` object
    into a `data.frame`. Information about the fit is lost in the
    process. This may be useful to produce better visualizations, for
    example using the ggplot2 package.

``` r
WH_1d_df <- WH_1d_fit |> output_to_df()
WH_2d_df <- WH_2d_fit |> output_to_df()
```

## Further WH smoothing theory

### Explicit solution

An explicit solution to the smoothing equation is obtained by computing
the derivate according to $\theta$ of the minimized quantity in the
equation. In the cas $w$ contains enough non-0 weights:

$$\hat{y} = (W + P_\lambda)^{- 1}Wy.$$

### Role of the penalization

In the smoothing equation, $(y - \theta)^{T}W(y - \theta)$ is a fidelity
criterion $\theta^{T}P_\lambda\theta$ a regularity criterion. The
relative importance of those criterions is controlled by the smoothing
parameter (or parameter vectors in the two-dimension case) $\lambda$.

In the one-dimension case, the penalization matrix may be rewritten:

$$
\theta^{T}P_\lambda\theta = \begin{cases}\lambda\underset{i = 1}{\overset{n - 1}{\sum}}(\theta_{i + 1} - \theta_i)^2 & \text{si }q = 1\\ \lambda\underset{i = 1}{\overset{n - 2}{\sum}}([\theta_{i + 2} - \theta_{i + 1}] - [\theta_{i + 1} - \theta_i])^2 & \text{si }q = 2
\end{cases}
$$

It is easily seen that:

-   In the case where $q_x = 1$ then this is equal to 0 only in the case
    where, for all $i\in[1,n_x - 1]$, $\theta_{i + 1} - \theta_i = 0$ or
    equivalently for all $i\in[1,n_x]$, $\theta_i = \theta_1$.

-   In the case where $q_x = 2$, this is equal to 0 only in the case
    where, for all $i\in[1,n_x - 2]$,
    $\theta_{i + 2} - \theta_{i + 1} = \theta_{i + 1} - \theta_i$, or
    equivalently for all $i\in[1,n_x]$,
    $\theta_i = \theta_1 + (i - 1)(\theta_2 - \theta_1)$.

In the sense of the penalization of order $q_x$, the space of
observation vectors that are totally smooth is therefore the space of
polynomial functions of degrees at most $q - 1$. Those results carry on
to two-dimension smoothing where a totally smooth observation matrix $Y$
corresponds to constant / aligned observations on the rows / columns of
the observation matrix, depending on the values of $q_x$ and $q_z$.

### Confidence / credibility intervals

Let $B$ a matrix such that $P_\lambda = B^TB$. The smoothing equation
may be rearranged:

$$(y - \theta)^TW(y - \theta) + \theta^TP_\lambda\theta = \left\Vert\begin{pmatrix}\sqrt{W}y \\ 0\end{pmatrix} - \begin{pmatrix}\sqrt{W} \\ B\end{pmatrix}\theta\right\Vert^2$$

which corresponds to an weighted mean square problem. In particular, if
$y \sim \mathcal{N}(\theta,\sigma^2W^{- 1})$ then
$\sqrt{W}y \sim \mathcal{N}(\sqrt{W}\theta,\sigma^2I_n)$ and the
framework of regression may be used. Note that $\theta = \mathbb{E}(y)$
is in this case both the parameter vector and the underlying law WH
smoothing is trying to estimate.

In the cas of 0 weights, the matrix $W^{- 1}$ is not properly defined.
By defining $w_*$ the subvector of stricly positive weights and $y_*$
the associated observation vector and noting $W_* = \text{Diag}(w_*)$,
the previous quantity may be rewritten:

$$\left\Vert\begin{pmatrix}\sqrt{W}y \\ 0\end{pmatrix} - \begin{pmatrix}\sqrt{W} \\ B\end{pmatrix}\theta\right\Vert^2 = \left\Vert\begin{pmatrix}\sqrt{W_{*}}y_{*} \\ 0\end{pmatrix} - \begin{pmatrix}\sqrt{W_{*}} \\ B\end{pmatrix}\theta\right\Vert^2$$

and the problem may now be expressed properly in terms of $y_{*}$ and
$w_{*}$. Nevertheless to keep things simple we stick with $y$ and $w$
and consider that $W^{- 1}$ contains infinite values associated with
initial 0 weights in $w$.

The previous explicit solution to the smoothing equation garanties the
normality of $\hat{y}$. In the framework of regression, the law of error
propagation yields:

$$\text{Var}(\hat{y}) = \text{Var}[(W + P_\lambda)^{- 1}Wy] = (W + P_\lambda)^{- 1}W\text{Var}(y) W(W + P_\lambda)^{- 1} = \sigma^2 (W + P_\lambda)^{- 1}W(W + P_\lambda)^{- 1}.$$
Unfortunately
$\mathbb{E}(\hat{y}) = (W + P_\lambda)^{- 1}W\mathbb{E}(y) \ne \mathbb{E}(y)$
if $\lambda \ne 0$. The penalization introduces a *smoothing bias* and
those results may not be used to compute a confidence interval for
$\theta$.

An alternative is to adopt a bayesian interpretation of the smoothing
equation and to interpretate the penalization as an *a priori* of the
form $\exp(- \theta^{T}P_\lambda\theta / 2\sigma^2)$ on $\theta$, which
boils down to assume that
$\theta \sim \mathcal{N}(0, \sigma^2P_\lambda^{-})$ (where $A^-$
corresponds to the pseudo-inverse of a matrix $A$). Bayes formula then
yields:

$$\begin{aligned}
f(\theta | y) &\propto f_y(y | \theta) f_\theta(\theta) \\
&\propto \exp\left(- \frac{1}{2\sigma^2}(y - \theta)^{T}W(y - \theta)\right)\exp\left(-\frac{1}{2\sigma^2}\theta^{T}P_\lambda\theta\right) \\
&\propto \exp\left(- \frac{1}{2\sigma^2}\left[(y - \theta)^{T}W(y - \theta) +\theta^{T}P_\lambda\theta\right]\right)
\end{aligned}$$

The mode of the posterior distribution
$\hat{\theta} = \text{argmax} [f(\theta | y)]$, also known as *maximum a
posteriori* (MAP) therefore matches the solution $\hat{y}$ of the
smoothing equation. Furthermore, a Taylor extension of
$\ln f(\theta | y)$ in $\theta = \hat{y}$ allows the posterior
distribution to be recognized as
$\mathcal{N}(\hat{y}, \sigma^2(W + P_\lambda)^{- 1})$.

An unbiased estimator of $\sigma^2$ is then given by:

$$\hat{\sigma}^2 = \frac{\|\sqrt{W}(y - \hat{y})\|^2}{n_* - \text{edf}}$$
where $n_*$ corresponds to the number of non 0 weights and
$\text{edf} = \text{tr}(H) = \text{tr}[(W + P_\lambda)^{- 1}W]$.

This result allows $100(1 -\alpha)\%$ credibility intervals of the form
$[\hat{y} \pm t_{n - \text{edf}}(\frac{1 - \alpha}{2})\sqrt{\text{diag}\left\lbrace(W + P_\lambda)^{- 1}\right\rbrace}]$
to be computed where $t_{n_* - \text{edf}}$ is the distribution function
for the student law of parameter $n_* - \text{edf}$. Let us note that
the use of student distribution instead of the normal distribution is
linked with the presence of an unknown $\sigma^2$ parameter.

## What should I use as observations and weights ?

The previous section show that applying WH smoothing to a couple $(y,w)$
such that $y \sim \mathcal{N}(\theta, \sigma^2W^{- 1})$ where $\theta$
is the underlying law we want to estimate, $\sigma^2$ is an
overdispersion parameter to be estimated, and $W = \text{Diag}(w)$,
then, using a bayesian interpration, credibility intervals were
available for the posterior distribution of $\theta | y$. In this
section, in the framework of survival analysis models, we exhibit a
candidate for $(y,w)$.

### Survival analysis framework - one-dimension case

Let us consider the observation of $m$ individuals in the cas of a
longitudinal study under left-truncating and right-censoring phenomena.
Let us assume a single law must be estimated (for example a mortality
law) and that it depends on a single covariate named $x$ (for example
the age of the insured). This law may be entirely characterized by
providing one of the 3 following quantities:

-   The cumulative distribution function $F(x)$ or the survival function
    $S(x) = 1 - F(x)$,

-   The associated density function
    $f(x) = - \frac{\text{d}}{\text{d}x}S(x)$,

-   The hazard rate
    $\mu(x) = - \frac{\text{d}}{\text{d}x}\text{ln} S(x)$

Let us assume that the underlying law depends on a parameter vector
$\beta$ that is to be estimated using maximum likelihood. The likelihood
associated with the observation of the individuals is:

$$
\mathcal{L}(\beta) = \underset{i = 1}{\overset{m}{\prod}} \left[\frac{f(x_i + t_i,\beta)}{S(x_i,\beta)}\right]^{\delta_i}\left[\frac{S(x_i + t_i,\beta)}{S(x_i,\beta)}\right]^{1 - \delta_i}
$$

where for each $x_i$ represents the age at the start of the observation,
$t_i$ is the observation duration and $\delta_i$ is 1 if the event of
interest (for example death) has been observed and 0 otherwise. Those 3
elements should be computed by taking into account the date of
subscribing and a possible termination date (for example beauce of
policy lapse) for each individual, possible presence of a waiting period
and exclusion of specific periods of time because of incomplete data or
medical underwriting effects. The observation period may therefore be
shorter than the actual period of presence of the individuals in the
portfolio.

Maximization of the previous likelihood may be rewritten by taking the
logarithm and using the relations:

$$\begin{aligned}
S(x) & = \exp\left(\underset{u = 0}{\overset{x}{\int}}\mu(u)\text{d}u\right) & f(x) & = \mu(x)S(x)
\end{aligned}$$

which yields, after a few simplifications:

$$
\ell(\beta) = \underset{i = 1}{\overset{m}{\sum}} \left[\delta_i \ln\mu(x_i + t_i,\beta) - \underset{u = 0}{\overset{t_i}{\int}}\mu(x_i + u,\beta)\text{d}u\right]
$$

Let us assume that the hazard rate is a piecewise constant function on
one-year interval between integer ages or more formally:
$\mu(x + \epsilon) = \mu(x)$ for all $x \in \mathbb{N}$ and
$\epsilon \in [0,1[$.

Let us further note that if $\mathbf{1}$ is the index function, then for
all $0 \le a < x_{\max}$,
$\underset{x = x_{\min}}{\overset{x_{\max}}{\sum}} \mathbf{1}(x \le a < x + 1) = 1$
by noting $x_{\min} = \min(x)$ and $x_{\max} = \max(x)$. The previous
likelihood therefore becomes:

$$\ell(\beta) = \underset{i = 1}{\overset{m}{\sum}} \left[\underset{x = x_{\min}}{\overset{x_{\max}}{\sum}} \delta_i\mathbf{1}(x \le x_i + t_i < x + 1)  \ln\mu(x_i + t_i,\beta) - \underset{u = 0}{\overset{t_i}{\int}}\underset{x = x_{\min}}{\overset{x_{\max}}{\sum}} \mathbf{1}(x \le x_i + u < x + 1)\mu(x_i + u,\beta)\text{d}u\right]$$

The piecewise constant assumption yields
$\mathbf{1}(x \le x_i + t_i < x + 1) \ln\mu(x_i + t_i,\beta) = \mathbf{1}(x \le x_i + t_i < x + 1) \ln\mu(x,\beta)$
and
$\mathbf{1}(x \le x_i + u < x + 1)\mu(x_i + u,\beta) = \mathbf{1}(x \le x_i + u < x + 1) \ln\mu(x,\beta)$.
The two sums may then be interverted, giving:

$$\begin{aligned}
\ell(\beta) &= \underset{x = x_{\min}}{\overset{x_{\max}}{\sum}} \left[\ln\mu(x,\beta) d(x) - \mu(x,\beta) e_c(x)\right] \quad \text{where} \\
d(x) & = \underset{i = 1}{\overset{m}{\sum}} \delta_i \mathbf{1}(x \le x_i + t_i < x + 1)  \quad \text{and} \\
e_c(x) & = \underset{i = 1}{\overset{m}{\sum}}\underset{u = 0}{\overset{t_i}{\int}}\mathbf{1}(x \le x_i + u < x + 1)\text{d}u = \underset{i = 1}{\overset{m}{\sum}} \left[\min(t_i, x - x_i + 1) - \max(0, x - x_i)\right]^+
\end{aligned}$$

where $d(x)$ and $e_c(x)$ corresponds respectively to the number of
observed deaths between age $x$ and $x + 1$ and to the sum of the
observation duration between those dates, by noting $a^+ = \max(a, 0)$.

### Extension to the two-dimension case

The extension of the previous approach to the two-dimension case only
requires minor adjustements to the previous proposition. Let us note
$z_{\min} = \min(z)$ and $z_\text{max} = \max(z)$. The piecewise
constant assumption needs to be extended to each of the two dimensions.
Formally we now assume that $\mu(x + \epsilon, z + \xi) = \mu(x, z)$ for
all $x, z \in \mathbb{N}$ and $\epsilon, \xi \in [0,1[$. The sums on $x$
are replaced by double sums on the values of both $x$ and $z$ and the
likelihood becomes:

$$\begin{aligned}
\ell(\beta) &= \underset{x = x_{\min}}{\overset{x_{\max}}{\sum}} \underset{z = z_{\min}}{\overset{z_{\max}}{\sum}}\left[\ln\mu(x,z,\beta) d(x,z) - \mu(x,z,\beta) e_c(x,z)\right] \quad \text{where} \\
d(x,z) & = \underset{i = 1}{\overset{m}{\sum}} \delta_i \mathbf{1}(x \le x_i + t_i < x + 1) \mathbf{1}(z \le z_i + t_i < z + 1)  \quad \text{and}\\
e_c(x,z) & = \underset{i = 1}{\overset{m}{\sum}}\underset{u = 0}{\overset{t_i}{\int}}\mathbf{1}(x \le x_i + u < x + 1)\mathbf{1}(z \le z_i + u < z + 1)\text{d}u \\
& = \underset{i = 1}{\overset{m}{\sum}} \left[\min(t_i, x + 1 - x_i, z + 1 - z_i) - \max(0, x - x_i, z - z_i)\right]^+.
\end{aligned}$$

### Likelihood equations

The log-likelihood in the one-dimension or two-dimension cases may be
expressed on the common vectorial form
$\ell(\beta) = \ln\mu(\beta)^{T}d - \mu(\beta)^{T}e_c$ where $d$ and
$e_c$ corresponds respectively to the vector of expected events and
associated central exposure.

In the particular case of log-linear models, the hazard rate may be
defined as $\ln\mu(\beta) = X\beta$ with $X$ a matrix of dimensions
$n \times p$ and full-rank $p \le n$. The use of the logarithmic link
ensures that $\mu = \exp(X\beta)$ is always positive. Derivatives of the
log-likelihood function are for this model:

$$\frac{\partial \ell}{\partial \beta} = X^{T}\left[d - \exp(X\beta) \odot e_c\right] \quad \text{and} \quad \frac{\partial^2 \ell}{\partial\beta^2} = - X^{T}W_{\beta}X \quad \text{where} \quad W_{\beta} = \text{Diag}(\exp(X\beta) \odot e_c).$$
Let us note that those likelihood equations are exactly those that would
have been obtained by treating the central exposure as a deterministic
quantity and by assuming that the number of deaths follows a Poisson GLM
of parameter $\mu(\beta)\times e_c$. The model above therefore behave as
a Poisson GLM (John Ashworth Nelder and Wedderburn 1972).

### Consequences for WH smoothing

Whittaker-Henderson focuses on the particular case where $X = I_n$ and
$\beta = \theta$. In that case the previous equations yield an explicit
solution $\beta = \ln(d) - \ln(e_c)$ and thus $\mu = d / e_c$.

Using the asymptotical properties of the maximum likelihood estimator,
we obtain $\hat{\beta} \sim \mathcal{N}(\beta, W_{\hat{\beta}}\,^{- 1})$
where diagonal elements of $W_{\hat{\beta}}$ are simply
$e_c \exp(\hat{\beta}) = e_c d / e_c = d$.

Thus it has been shown that in the survival analysis framework
introduced, asymptotically
$\ln(d / e_c) \sim \mathcal{N}(\ln(\mu), W^{- 1})$ where
$W = \text{Diag}(d)$. This justify applying WH smoothing to the
observation vector $y = \ln(d / e_c)$ with weights $w = d$. The
overdispersion parameter is in this cas simply $\sigma^2 = 1$. We obtain
credibility intervals for the posterior distribution $\theta | y$ of the
form:
$[\hat{y} \pm \Phi(\frac{1 - \alpha}{2})\sqrt{\text{diag}\left\lbrace(W + P_\lambda)^{- 1}\right\rbrace}]$.

### Generalization to penalized maximum likelihood

The previous approach relies on the asymptotic properties of the maximum
likelihood estimator. When few data is available, those properties may
not hold. An alternative approach is to apply the penalization from the
WH smoothing directly to the previous likelihood function and thus
maximize the penalizaed likelihood
$\ell_P(\beta) = \ell(\beta) - \beta^{T}P_\lambda\beta / 2$. This can be
seen as a generalization of WH smoothing to non-gaussian likelihood.
Still assuming a log-linear model is used, derivatives of the
log-likelihood functions become:

$$\frac{\partial \ell_P}{\partial \beta} = X^{T}\left[d - \exp(X\beta) \odot e_c\right] - P_\lambda\beta \quad \text{and} \quad \frac{\partial^2 \ell_P}{\partial\beta^2} = - (X^{T}W_{\beta}X + P_\lambda) \quad \text{where} \quad W_{\beta} = \text{Diag}(\exp(X\beta) \odot e_c).$$

Unlike the previously encountered likelihood, those equations does not
have an explicit solution, even when $X = I_n$, because both $X\beta$
and $\exp(X\beta)$ appear in the equations. Using Newton algorithm, a
series of estimators $(\beta_k)_{k \ge 0}$ may be built so that it
converges to the penalized maximum likelihood estimator
$\hat{\beta} = \underset{\beta}{\text{argmin}}\:l_P(\beta)$.

Those estimators are defined by:

$$
\beta_{k + 1} = \beta_k - \left(\left.\frac{\partial^2 \ell_P}{\partial\beta^2}\right|_{\beta = \beta_k}\right)^{- 1} \left.\frac{\partial \ell_P}{\partial\beta}\right|_{\beta = \beta_k} = \beta_k + (X^{T}W_kX + P_\lambda)^{- 1} \left[X^{T}\left(d - \exp(X\beta_k) \odot e_c\right) - P_\lambda \beta_k\right] = \Psi_k X^{T}W_k z_k
$$

by noting $\eta_k = X\beta_k$, $\Psi_k = (X^{T}W_kX + P_\lambda)^{- 1}$,
$W_k = \text{Diag}(\exp(\eta_k) \odot e_c)$ and
$z_k = \eta_k + W_k^{- 1}[d - \exp(\eta_k) \odot e_c]$. Setting
$\eta_0 = \ln d - \ln e_c$ yields an adequate starting point to the
algorithm (the starting $\beta_0$ does not have to be provided). This
implies that $W_0 = \text{Diag}(d)$ and $\mathbf{z}_0 = \ln d - \ln e_c$
which corresponds to the observations and weights used in the regression
framework. The update of $\beta_k$ in the Newton optimization step boils
down to applying WH smoothing to the couple ($z_k$, $W_k$). The first
estimator $\beta_1$ computed is therefore the solution of WH smoothing
in the regression framework. Further iterations relies on the *working
vector* $z_k$ and associated weights $W_k$ that are adjusted at each
step based on the previous step results.

The posterior distribution of $\beta | y$ may be asymptotically
approached by
$\mathcal{N}(\hat{\beta}, (X^TW_{\hat{\beta}}X + P_\lambda)^{- 1})$,
which allows $100(1 -\alpha)\%$ credibility intervales of the form
$\left[\hat{\beta} \pm \Phi(1 - \alpha / 2) \sqrt{\text{diag}\lbrace(X^TW_{\hat{\beta}}X + P_\lambda)^{- 1}\rbrace}\right]$
where $\Phi$ is the cumulative distribution function of the normal
distribution.

## How is the optimal smoothing parameter determined ?

### Short answer

The optimal smoothing parameter is determined according to a statistical
criterion. There are two main types of criteria that are adequate in
this case:

-   Prediction error criteria aim at minimizing the (asymptotic)
    prediction error. Such criteria include Akkake Information Criterion
    (AIC), Ordinary Cross-Validation (OCV) and Global Cross-Validation
    (GCV). The Bayesian Information Criterion (BIC) is very close to AIC
    and therefore may be added to this category although its
    interpretation is very different.

-   Likelihood-based criteria such as profile likelihood or restricted
    maximum likelihood (REML), a variant that accounts for the
    complexity of the model. Such criteria, while being less interesting
    in the asymptotic case, proved to perform better in most real-life
    situations with finite size samples.

At the time of writing the WH package allows AIC, BIC, GCV and REML, the
default, to be selected. Once this choice has been made, the package
uses a recursive algorithm to find the optimal smoothing parameter
according to the selected criterion:

-   In the case AIC, BIC or GCV is chosen, the `optimize` function is
    used in the one-dimension case (see Brent 1973) and the `optim`
    function with the Nelder-Mead algorithm (see John A. Nelder and
    Mead 1965) in the two-dimension case. Both functions come from the
    stats package and perform adequate optimization in the absence of
    derivatives. They may also be used with the REML criteria.

-   In the case REML (the default) is chosen, the generalized
    Fellner-Schall method (Wood and Fasiolo 2017) is used instead.
    Compared to the previous solution it allows for faster solving of
    the problem, especially in the two-dimension case where the number
    of observation points is high. The Fellner-Schall generalized update
    leads to an approximate solution when the maximum likelihood
    framework is used, yet this has little impact in practice.

### Long answer

See the upcoming paper !

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-brent1973optimize" class="csl-entry">

Brent, Richard P. 1973. “Algorithms for Minimization Without
Derivatives, Chap. 4.” Prentice-Hall, Englewood Cliffs, NJ.

</div>

<div id="ref-carballo2021prediction" class="csl-entry">

Carballo, Alba, Marı́a Durbán, and Dae-Jin Lee. 2021. “Out-of-Sample
Prediction in Multidimensional p-Spline Models.” *Mathematics* 9 (15):
1761.

</div>

<div id="ref-henderson1924new" class="csl-entry">

Henderson, Robert. 1924. “A New Method of Graduation.” *Transactions of
the Actuarial Society of America* 25: 29–40.

</div>

<div id="ref-nelder1965optim" class="csl-entry">

Nelder, John A, and Roger Mead. 1965. “A Simplex Method for Function
Minimization.” *The Computer Journal* 7 (4): 308–13.

</div>

<div id="ref-nelder1972glm" class="csl-entry">

Nelder, John Ashworth, and Robert WM Wedderburn. 1972. “Generalized
Linear Models.” *Journal of the Royal Statistical Society: Series A
(General)* 135 (3): 370–84.

</div>

<div id="ref-whittaker1922new" class="csl-entry">

Whittaker, Edmund T. 1922. “On a New Method of Graduation.” *Proceedings
of the Edinburgh Mathematical Society* 41: 63–75.

</div>

<div id="ref-wood2017fellner" class="csl-entry">

Wood, Simon N, and Matteo Fasiolo. 2017. “A Generalized Fellner-Schall
Method for Smoothing Parameter Optimization with Application to Tweedie
Location, Scale and Shape Models.” *Biometrics* 73 (4): 1071–81.

</div>

</div>

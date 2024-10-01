# Baseball Analytics

This project studies several decades worth of baseball data using statistical packages in R.

## Acknowledgements

- [MLB Winning Percentage Breakdown](https://www.samford.edu/sports-analytics/fans/2022/MLB-Winning-Percentage-Breakdown-Which-Statistics-Help-Teams-Win-More-Games)
- [Common MLB Statistics: Which Stats Determine a Team's Win Percentage - Sarah Sult](https://sarahesult.medium.com/common-mlb-statistics-which-stats-determine-a-teams-win-percentage-a6e0a83aa07c)
- [Pythagorean Theorem of Baseball](https://www.baseball-reference.com/bullpen/Pythagorean_Theorem_of_Baseball)
- [Sabermetrics](https://en.wikipedia.org/wiki/Sabermetrics)
- **Moneyball - Columbia Pictures**
- [Moneyball Dataset](https://www.kaggle.com/datasets/wduckett/moneyball-mlb-stats-19622012)

## Libraries

**Note: this analysis uses an R kernel in Jupyter Notebooks.**

- tidyverse
- caret
- corrplot
- reshape2
- gridExtra
- leaps
- ggpubr
- car
- DescTools

## Methodology

Following from the `Baseball_Analytics.ipynb` Jupyter Notebook:

1) Initial Findings
2) Theoretical Bases
3) Data Cleaning
4) Functions
5) Generalized Linear Models
6) Multiple Linear Regression Models
7) Pythagorean Theorem of Baseball - Simple Linear Regression on a Transformed Equation
8) Conclusions 

The models were assessed on how their resulting assumptions:

- linearity: observed versus predicted plot
- independence: successive residual plot
- constant variance (homoscedasticity): residuals vs. fittted plot
- normality: qq-plot
- outliers (influential points): residuals vs. leverage plot with Cook's distance thresholds

## Pythagorean Theorem of Baseball

Consider the following:

- $R_s$: runs scored
- $R_a$: runs allowed
- $W_p$: winning percentage

Using these values, two formulas are presented:

- common: $$W_p = \frac{R_a^2}{R_s^2 + R_a^2}$$
- accurate: $$W_p = \frac{R_a^{1.81}}{R_s^{1.81} + R_a^{1.81}}$$

Does the data support this theorem? In other words, setting the exponent to $f$,

$$W_p = \frac{R_a^f}{R_s^f + R_a^f}$$

---

For ease, let's assign 

- $R_s = s$
- $R_a = a$
- $W_p = p$

$W_p = \frac{R_s^f}{R_s^f + R_a^f}$

$\rightarrow p = \frac{s^f}{s^f + a^f}$

$(s^f + a^f) p = \frac{s^f}{s^f + a^f} (s^f + a^f)$

$(s^f + a^f)p = s^f$

$s^fp + a^fp = s^f$

$a^fp = s^f - s^fp$

$a^fp = s^f(1 - p)$

$\frac{p}{1 - p} = \frac{s^f}{a^f}$

$\log(\frac{p}{1 - p}) = \log(\frac{s^f}{a^f})$

$\log(\frac{p}{1 - p}) = f\log(\frac{s}{a})$

---

So what this means for our dataset and model is that we can transform our columns into:

- $y = \log(\frac{W_p}{1-W_p})$: response variable
- $x = \log(\frac{R_s}{R_a})$: predictor variable

By using simple linear regression, we can create a model where

$y = \beta_0 + \beta_1 x$,

If the **Pythagorean Formula of Baseball** holds weight, then we would roughly expect

$\beta_0$ to be close to $0$ and $\beta_1$ near the range $(1.81, 2)$, from:

The more *common*:

$W_p = \frac{R_s^2}{R_s^2 + R_a^2}$

The more *accurate*:

$W_p = \frac{R_s^{1.81}}{R_s^{1.81} + R_a^{1.81}}$


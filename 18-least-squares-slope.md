# Lesson 18 · From covariance to the least-squares slope

## Engineering question

Given input size before a build starts, what duration should a straight-line predictor return? The slope has physical units of seconds per MB. A correlation coefficient has no units. Two logs can each have perfect positive correlation even if one takes one extra second per MB and the other takes ten.

## Refresher from the correlation submission

All three logs select each of three rows with probability 1/3. The complete-log covariance is the average of the paired centered products. Pearson correlation divides this by the product of the two standard deviations.

| Data | Means | Centered products | Covariance | Standard deviations | Correlation |
|---|---|---|---|---|---|
| Requests (2,3), (4,6), (6,9), in kB and ms | 4 kB, 6 ms | 6, 0, 6 | 4 kB·ms | √(8/3) kB, √6 ms | 4/4 = 1 |
| Builds (1,5), (2,3), (3,4), in MB and s | 2 MB, 4 s | −1, 0, 0 | −1/3 MB·s | √(2/3) MB, √(2/3) s | (−1/3)/(2/3) = −1/2 |
| Runs (4,9), (4,6), (4,3), in workers and s | 4 workers, 6 s | 0, 0, 0 | 0 worker·s | 0 workers, √6 s | Undefined |

For the requests the payload deviations are −2, 0, 2 and the latency deviations −3, 0, 3. Each nonzero pair has matching signs. The data lie on the line latency = 1.5 × payload in the stated units.

Converting build duration to milliseconds multiplies each duration deviation by 1000. Covariance becomes −1000/3 MB·ms. Duration variance grows by a factor of 1,000,000 because its formula squares each deviation. Its square root, the duration standard deviation, grows by 1000. The spread product becomes 2000/3 MB·ms. Dividing the new covariance by this new denominator gives −1/2 again.

In the worker log every worker deviation equals zero. Duration deviations are 3, 0, −3 s, so their mean square is 18/3 = 6 s². The required division is 0/(0 × √6). A guard before division can return None for this undefined result. All observations were taken at the same worker count. The missing comparison between worker counts persists under exact arithmetic.

## Model and definitions

A complete log has a positive number $N$ of equally weighted rows. Row $i$, indexed from 1 through $N$, contains one observed input $x_i$ and one observed output $y_i$. Choose two real parameters: an intercept $a$ (output units) and a slope $b$ (output units per input unit). The prediction at input $x$ is $\hat y=a+bx$. The hat marks a prediction. The intercept is the line's output at zero input, which can lie outside the observed data range. Increasing the input by one unit increases the prediction by the signed amount $b$.

The residual on row $i$ is $y_i-(a+bx_i)$. **Ordinary least squares** chooses parameters minimizing the sum of squared residuals. Dividing by the positive fixed count $N$ yields mean squared error (MSE) and preserves the minimizing parameters.

## Derivation

Write $\bar x=(\sum_i x_i)/N$ and $\bar y=(\sum_i y_i)/N$ for the arithmetic means. Each sum visits all rows once.

1. Hold $b$ fixed. Define $t_i=y_i-bx_i$. The residual now reads $t_i-a$. By the **least-squares property of the mean**, the best constant prediction for the numbers $t_i$ is their average. Therefore $a=(\sum_i t_i)/N=\bar y-b\bar x$. The last equality distributes the sum across subtraction and moves the constant $b$ outside the sum.
2. Substitute this intercept into the line. For any slope under consideration, its best-positioned line predicts $\bar y+b(x-\bar x)$. At $x=\bar x$ the term in parentheses vanishes, so the line passes through $(\bar x,\bar y)$.
3. For row $i$ define $d_i=x_i-\bar x$ and $e_i=y_i-\bar y$. Subtract prediction from observation: $y_i-[\bar y+b(x_i-\bar x)]=e_i-bd_i$. Thus the score as a function of slope alone is $J(b)=(1/N)\sum_i(e_i-bd_i)^2$.
4. Expand one square: $(e_i-bd_i)^2=e_i^2-2be_id_i+b^2d_i^2$. Average each term. Call the averages of $e_i^2$, $d_ie_i$ and $d_i^2$ respectively $V_Y$ (output variance), $C$ (covariance) and $V_X$ (input variance). Then $J(b)=V_Y-2bC+b^2V_X$.
5. Differentiate with respect to $b$. The log is fixed, so $V_Y,C,V_X$ are constants. The constant term gives zero, the linear term gives $-2C$ and the quadratic term gives $2bV_X$. At a stationary point $0=-2C+2bV_X$. Add $2C$ to both sides. If $V_X>0$, divide by $2V_X$ to obtain $b=C/V_X$. Since $J''(b)=2V_X>0$ everywhere, this is the unique minimizing slope.
6. Units check: covariance has units input × output and input variance has units input². Their ratio has units output/input. If input variance is zero, all rows share one input. Many lines through that input and the mean output then earn the same score.

## Worked example · Build duration

For (size in MB, time in seconds) equal to (1,5), (2,3), (3,4), the means are 2 MB and 4 s. The input deviations −1,0,1 give $V_X=(1+0+1)/3=2/3$ MB². Output deviations 1,−1,0 give $C=(-1+0+0)/3=-1/3$ MB·s.

The fitted slope is $b=(-1/3)/(2/3)=-1/2$ s/MB. The intercept is $a=4-(-1/2)(2)=5$ s. Predictions at the three observed sizes are 4.5, 4, 3.5 s. Subtracting each from its observed duration gives residuals 0.5, −1, 0.5 s. The MSE is $(0.25+1+0.25)/3=0.5$ s². The constant mean predictor of 4 s scored $(1+1+0)/3=2/3$ s². The fitted line improves this log's score. It predicts half a second less per additional MB. This describes the fitted relationship across these three records.

## Hand practice

Each problem uses a complete, equally weighted log. Give your setup and interpretation in Python comments or code.

1. For (payload kB, latency ms) pairs (1,5), (2,7), (3,9), fit a line with an intercept. Give both means, covariance, input variance, slope with units, intercept, and all three residuals.
2. For (input MB, build time s) pairs (1,4), (2,7), (3,7), fit a line with an intercept. Predict the duration at 2.5 MB. Compare fitted MSE with the MSE from always predicting the observed mean.
3. For (workers, duration s) pairs (3,8), (3,10), (3,12), find the best prediction at three workers. Construct two lines with different slopes that make this prediction. Compute their MSEs and explain what these runs establish about the slope.

<details>
<summary>Worked practice solutions · Open after attempting</summary>

**1. Payload.** Means are $6/3=2$ kB and $21/3=7$ ms. Deviations are (−1,0,1) and (−2,0,2). Covariance is $(2+0+2)/3=4/3$ kB·ms. Input variance is $(1+0+1)/3=2/3$ kB². Divide to get $b=(4/3)/(2/3)=2$ ms/kB. Use the mean point to get $a=7-2(2)=3$ ms. Predictions are $3+2(1)=5$, $3+2(2)=7$, $3+2(3)=9$ ms. All residuals are zero. The line adds two milliseconds per additional kB.

**2. Build.** The means are $6/3=2$ MB and $18/3=6$ s. Deviations are (−1,0,1) and (−2,1,1), so covariance is $(2+0+1)/3=1$ MB·s and input variance is $2/3$ MB². The slope is $1/(2/3)=1.5$ s/MB and the intercept is $6-1.5(2)=3$ s. At 2.5 MB the prediction is $3+1.5(2.5)=6.75$ s. At 1,2,3 MB the predictions are 4.5, 6, 7.5 s. Residuals are −0.5,1,−0.5 s, so MSE is $(0.25+1+0.25)/3=0.5$ s². The constant mean predictor scores $(4+1+1)/3=2$ s². The improvement in this log is 1.5 s².

**3. Workers.** Every record uses the same input, so any proposed line gives a common prediction on all records. The best common value is the mean duration $(8+10+12)/3=10$ s. A line passing through (3,10) satisfies $a+3b=10$. Taking $b=0$ gives $a=10$. Taking $b=2$ s/worker gives $a=4$ s. Both lines predict 10 s at three workers. Their residuals are −2,0,2 s, and both MSEs equal $(4+0+4)/3=8/3$ s². Changing the slope while maintaining $a=10-3b$ leaves all fitted values identical. The slope is **unidentifiable** from this log.

</details>

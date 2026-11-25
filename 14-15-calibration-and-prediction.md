# Lessons 14–15 · Calibrating measurements and fitting predictions

In calibration you choose a correction and already know the reference. In prediction you choose an output before the target is revealed. In both problems, a squared-error score lets you compare the possible choices.

**Lesson topics**

- Lesson 14 · Mean squared error and constant calibration
- Lesson 15 · Conditional means as least-squares predictors

## Lesson 14 · Mean squared error and constant calibration

**Guiding question.** Which single correction should be applied to every reading, and how much error will remain?

Let $R$ be a reading, $r$ a fixed known reference, and $E=R-r$ the raw signed error. Subtract a constant $b$ from every reading. The corrected reading is $R-b$ and its signed error is $(R-b)-r=E-b$. A positive $b$ shifts every reading down. A negative $b$ shifts every reading up. The quantity to minimize is $J(b)=\mathbb E[(E-b)^2]$, the mean squared corrected error.

Write $\mu=\mathbb E[E]$ for the average raw error. Add and subtract $\mu$ inside the square:
$$
\begin{aligned}
E-b&=(E-\mu)+(\mu-b),\\
(E-b)^2&=(E-\mu)^2+2(E-\mu)(\mu-b)+(\mu-b)^2,\\
J(b)&=\mathbb E[(E-\mu)^2]+2(\mu-b)\mathbb E[E-\mu]+(\mu-b)^2,\\
&=\operatorname{Var}(E)+(\mu-b)^2.
\end{aligned}
$$
The first line changes nothing because the two copies of $\mu$ cancel. The second expands the square of a sum. The third averages term by term by **linearity of expectation**, pulling out constants. The middle term vanishes because $\mathbb E[E-\mu]=\mu-\mu=0$. The first term is variance by definition. This is the **mean squared error decomposition** for a fixed reference, also called the **bias–variance identity**. Bias here means the mean signed error after correction, $\mu-b$.

Variance stays fixed under a constant shift: the distance between every reading and its own shifted mean stays the same. Thus the best correction is $b=\mu$, making the square zero. The smallest *score* is $\operatorname{Var}(E)$. If the readings have positive spread then perfect correction of every reading with one constant is impossible. Solving $J(b)=0$ asks whether perfect correction exists. To minimize by calculus, differentiate $J(b)=\operatorname{Var}(E)+(b-\mu)^2$. The variance term is constant and the square gives $J'(b)=2(b-\mu)$. The stationary point is $b=\mu$, and $J''(b)=2>0$ confirms a minimum.

**Reading a score function.** The parentheses in $J(b)$ mean “evaluate the scoring procedure at the proposed adjustment $b$.” First fix $b$, then compute a corrected error for every row, square it, and average. The units of $b$ are those of the sensor. The units of $J(b)$ are those units squared. The derivative $J'(b)$ gives the rate of change of score per unit increase in $b$. At the bottom of this smooth quadratic its slope is zero even when its height is positive.

### Temperature adjustment and imaginary roots

**Problem.** The reference is 60 °C. Readings are 56, 58, 58 and 60 °C. A calibration subtracts the same real number $b$ from every reading. Find the best correction, corrected readings and minimum MSE.

**Worked solution**

1. Form the raw errors in record order: $56-60=-4$, $58-60=-2$, $58-60=-2$, $60-60=0$ °C. Their mean is $(-4-2-2+0)/4=-2$ °C. The sign tells us the instrument reads low on average.

2. After subtracting $b$, the errors become $-4-b,-2-b,-2-b,-b$. Directly from the definition, $J(b)=[(-4-b)^2+2(-2-b)^2+(-b)^2]/4$. The factor 2 retains the repeated 58 °C reading.

3. Expand the numerator. The first square gives $b^2+8b+16$, the pair gives $2b^2+8b+8$, and the last gives $b^2$. Add like terms and divide by four: $J(b)=(4b^2+16b+24)/4=b^2+4b+6$.

4. Complete the square: $(b+2)^2=b^2+4b+4$, so $J(b)=(b+2)^2+2$. The square's least possible value is zero when $b=-2$ °C. Therefore the score there is 2 °C². Setting the entire score equal to zero would instead require $(b+2)^2=-2$. Imaginary roots answer the perfect-fit equation. The real minimizing correction comes from making the square as small as possible.

5. Subtracting $-2$ adds 2 °C. The corrected readings are 58, 60, 60, 62. Their errors are $-2,0,0,2$. The corrected score is $(4+0+0+4)/4=2$ °C². Compare the uncorrected score $(16+4+4+0)/4=6$ °C².

### Flow correction with unequal group counts

**Problem.** Reference flow is 20 L/min. Eight readings are 21 and two are 26 L/min. Compare subtracting 1 L/min with the optimal constant correction.

**Worked solution**

1. Map readings to errors: eight errors of $21-20=1$ and two of $26-20=6$. On the ten equally weighted records, mean error is $[8(1)+2(6)]/10=20/10=2$ L/min.

2. The MSE decomposition says the best amount to subtract equals the mean error. To see why the proposed correction differs, subtract 1 first. Residual errors are $1-1=0$ and $6-1=5$ L/min.

3. Square, retain both counts, and divide by ten: $J(1)=[8(0^2)+2(5^2)]/10=(0+50)/10=5$ (L/min)².

4. Now subtract 2. Residual errors are $1-2=-1$ and $6-2=4$. Their mean is $[8(-1)+2(4)]/10=0$. Their MSE is $J(2)=[8(1)+2(16)]/10=40/10=4$ (L/min)².

5. The improvement is $5-4=1$ (L/min)². Eight readings acquire a small error under the second choice, but the two large errors shrink enough to reduce the squared-error total.

### Is a pressure specification achievable?

**Problem.** Raw pressure error has mean +3 kPa and variance 4 kPa². After subtracting a constant, the MSE is required to be at most 3 kPa². Can any constant satisfy it?

**Worked solution**

1. Write $b$ for the numerical amount subtracted in kPa. The remaining mean error is $3-b$ kPa. Subtracting the same number from every reading leaves the spread around their mean unchanged.

2. Insert the given variance and remaining mean error into the identity: $J(b)=4+(3-b)^2$, with the score measured in kPa².

3. The square is nonnegative for every real $b$. At $b=3$ it vanishes and $J(3)=4$. The requested ceiling of 3 is below the attainable minimum of 4.

4. As an inequality check, $4+(3-b)^2\le3$ implies $(3-b)^2\le-1$, which is impossible for a real pressure adjustment.

### Pressure calibration about 100 kPa

**Problem.** Four readings are 99, 101, 101 and 103 kPa for reference 100 kPa. Find the best constant to subtract and the resulting score.

**Worked solution**

1. Subtract the reference from the readings to obtain errors $-1,+1,+1,+3$ kPa. Their mean is $(-1+1+1+3)/4=1$ kPa.

2. Choose $b=1$ kPa by the MSE decomposition. The corrected errors are $-1-1=-2$, $1-1=0$, $1-1=0$ and $3-1=2$.

3. Square and average: $(4+0+0+4)/4=2$ kPa². For a check, the raw MSE was $(1+1+1+9)/4=3$ kPa². Removing the squared mean offset reduces it by $1^2=1$ kPa².

### Pressure calibration about 50 kPa

**Problem.** The reference is 50 kPa and the log reads 48, 50, 50 and 54 kPa. Find the raw MSE, the optimal constant correction and its MSE.

**Worked solution**

1. Raw errors are $-2,0,0,+4$ kPa. Their mean is $(-2+0+0+4)/4=0.5$ kPa. The average of their squares is $(4+0+0+16)/4=5$ kPa².

2. Subtract the mean error 0.5 kPa from every reading. Residual errors become $-2.5,-0.5,-0.5,+3.5$ kPa.

3. Square in record order and average: $(6.25+0.25+0.25+12.25)/4=19/4=4.75$ kPa². Check the decomposition: $4.75+(0.5)^2=4.75+0.25=5$ kPa², the original score.

## Lesson 15 · Conditional means as least-squares predictors

**Guiding question.** When a feature is available before a request runs, does using it lower the squared error of the predictions?

A **feature** is an input available when you make a prediction. The **target** is the value being predicted. A **residual** here is measured value minus predicted value. For $N$ equally weighted records, $y_i$ is the observed target on row $i$ and $\hat y_i$ its prediction. The hat denotes a predicted value. The empirical score is
$$
\mathrm{MSE}=\frac{1}{N}\sum_{i=1}^{N}(y_i-\hat y_i)^2.
$$
The index $i$ takes each integer from 1 through $N$. First match the prediction to its record, then subtract, square, sum over records and divide by their count. An MSE in ms² can be square-rooted to obtain root mean squared error in ms.

The **least-squares property of the mean** comes from the preceding identity with the outcome $Y$ in place of the error $E$ and the prediction $c$ in place of the correction $b$. The best constant for all records is their mean. Within a group the same argument applies to just that group's records. In probability language this is the **conditional expectation least-squares theorem**. For a finite second moment, the mean among records with the available feature value minimizes mean squared error among rules using that information. Group-specific constants can be chosen independently because each record belongs to exactly one group.

**Combining group scores.** If group $g$ has $n_g$ records and MSE $m_g$, multiplying $n_g m_g$ recovers its sum of squared errors. Add these sums across all groups and divide by $N=\sum_g n_g$:
$$
\mathrm{MSE}_{\mathrm{all}}=\frac{\sum_g n_gm_g}{\sum_g n_g}.
$$
The index $g$ visits each distinct group once. This weights records uniformly when group sizes differ.

**Why separate means minimize the complete score.** For each group $g$, call its prediction $c_g$, its mean target $\mu_g$ and its within-group variance $v_g$. Apply the Lesson 14 square expansion inside that group. The group's average squared error is $v_g+(\mu_g-c_g)^2$. Its contribution to the complete score is this quantity times its share $n_g/N$. For a represented group that share is positive. Only its square changes when we vary $c_g$, so $c_g=\mu_g$ minimizes that contribution. The group choices affect separate terms. Choosing every group mean therefore minimizes the overall score. With one group, the argument gives the overall mean as the best single prediction.

### One prediction for two different requests

**Problem.** Two requests took 4 and 8 ms. The prediction must be the same real number $c$ for both. Find the prediction that minimizes MSE and its score.

**Worked solution**

1. Write $J(c)=[(4-c)^2+(8-c)^2]/2$ since each record supplies one squared residual. Expand to get $[(c^2-8c+16)+(c^2-16c+64)]/2=c^2-12c+40$.

2. Since $(c-6)^2=c^2-12c+36$, the score is $(c-6)^2+4$. Choose $c=6$ ms to minimize the square. The attained score is $0+4=4$ ms².

3. Direct check: predicting 6 gives errors $4-6=-2$ and $8-6=+2$. Averaging $4$ and $4$ gives the same score. This example distinguishes a decision in ms from its evaluation in ms².

### Read a completed-square expression

**Problem.** The score is $J(c)=(c-7)^2+9$ with $c$ the prediction in ms. Find the prediction and attained score.

**Worked solution**

1. For a real prediction the squared term is at least zero. The remainder 9 is fixed for every possible prediction.

2. To attain the lower bound solve $c-7=0$, giving $c=7$ ms. Substitute into the original score: $J(7)=(7-7)^2+9=9$ ms².

3. Check by moving one millisecond away in either direction. Both $J(6)$ and $J(8)$ equal $1+9=10$ ms², above the value at 7.

### Choose a feature using four requests

**Problem.** In order, (size, server, latency in ms) are (small,A,2), (small,B,4), (large,A,8), (large,B,10). Both labels are known before prediction. Compare three rules on these four records.

**Worked solution**

1. Baseline: the pooled mean is $(2+4+8+10)/4=6$ ms. Subtract from observed values to get residuals $-4,-2,+2,+4$. Squaring and averaging gives $(16+4+4+16)/4=10$ ms².

2. Size rule: small predicts $(2+4)/2=3$ and large predicts $(8+10)/2=9$. Returning predictions to record order gives $3,3,9,9$ ms. The residuals are $-1,1,-1,1$ and MSE is $(1+1+1+1)/4=1$ ms².

3. Server rule: A predicts $(2+8)/2=5$ and B predicts $(4+10)/2=7$. In record order predictions are $5,7,5,7$. Residuals are $-3,-3,3,3$, so MSE is $36/4=9$ ms².

4. Among the three rules, size yields the lowest in-log score. To assess prediction on future traffic, fit using the initial log and evaluate those fixed predictions on separate records.

### Disk reads and writes

**Problem.** Six operations took 2, 2, 4, 4 ms for reads and 8, 10 ms for writes. Fit a single prediction and one prediction per operation. Calculate both scores on all six records.

**Worked solution**

1. Use the least-squares property of the mean. Pooled total is $2+2+4+4+8+10=30$ ms; dividing by six gives 5 ms. Pooled residuals are $-3,-3,-1,-1,3,5$ ms.

2. Square each and retain each individual operation: $(9+9+1+1+9+25)/6=54/6=9$ ms².

3. Fit separate means. Reads predict $(2+2+4+4)/4=3$ ms; writes predict $(8+10)/2=9$ ms. The residuals in original order are $-1,-1,1,1,-1,1$.

4. Four reads contribute $4(1)=4$ and two writes $2(1)=2$ to total squared error. Grouped MSE is $(4+2)/6=1$ ms². Improvement is $9-1=8$ ms² on this log.

### Routes with equal means

**Problem.** A's recorded times are 2, 6, 10 ms and B's are 5, 6, 7 ms. Does route improve the prediction? What else does route reveal?

**Worked solution**

1. Compute A's mean $(2+6+10)/3=18/3=6$ ms and B's mean $(5+6+7)/3=18/3=6$ ms. The complete mean is $(18+18)/6=6$ ms.

2. The constant and route rules return 6 on every row. The A squared residuals are $(2-6)^2=16$, 0 and 16. The B squared residuals are $1,0,1$.

3. Both rules have MSE $(16+0+16+1+0+1)/6=34/6=17/3$ ms². Separately A's score is $32/3$ ms² and B's is $2/3$ ms². The route identifies which group has larger spread in these data.

### Pool group scores with different counts

**Problem.** A fixed predictor gave MSE 1 ms² for 90 small requests and 25 ms² for ten large requests. A dashboard reports 13 ms². Find the score for all requests.

**Worked solution**

1. A group mean equals its sum divided by its count. Reverse that operation: small requests contribute $90(1)=90$ ms² and large requests contribute $10(25)=250$ ms² to the squared-error sum.

2. Combine the numerators and the record counts: $(90+250)/(90+10)=340/100=3.4$ ms².

3. The dashboard used $(1+25)/2=13$. That describes a procedure giving the small and large groups equal weight. The log gives them weights $90/100$ and $10/100$. Checking $0.90(1)+0.10(25)=0.9+2.5$ recovers 3.4.

### Alternative practice · Cache hits versus misses

**Problem.** Hits took 1, 1, 3, 3 ms and misses took 7, 9 ms. Fit and score a constant predictor and a cache-status predictor.

**Worked solution**

1. Pooled mean is $(1+1+3+3+7+9)/6=24/6=4$ ms. Residuals are $-3,-3,-1,-1,3,5$. MSE is $(9+9+1+1+9+25)/6=9$ ms².

2. Hits have mean $(1+1+3+3)/4=2$ and misses have mean $(7+9)/2=8$ ms. Record-by-record errors are $-1,-1,1,1,-1,1$ ms.

3. Squaring gives six ones. The score is $6/6=1$ ms². Cache status reduces the score on these six requests by 8 ms².

### Alternative practice · Two routes centered on four

**Problem.** A took 2, 4, 6 ms and B took 3, 4, 5 ms. Does a separate prediction for each route reduce the MSE?

**Worked solution**

1. A's mean is $12/3=4$ and B's is $12/3=4$ ms. The pooled mean is $(12+12)/(3+3)=4$ ms.

2. Using 4 on every row produces squared errors $4,0,4$ for A and $1,0,1$ for B. The combined score is $(8+2)/6=5/3$ ms².

3. Using the two group means predicts exactly the same value on each row, hence the same MSE. The spread differs: A's within-route MSE is $8/3$ ms² while B's is $2/3$ ms².

### Alternative practice · Two unequal groups with equal error totals

**Problem.** Ninety small requests have MSE 1 ms². Ten large requests have MSE 9 ms². Find the pooled MSE and compare the contributions.

**Worked solution**

1. Small requests contribute $90(1)=90$ ms² in all. Large requests contribute $10(9)=90$ ms² in all.

2. Add the two totals to obtain 180 ms² across $90+10=100$ requests. The score per request is $180/100=1.8$ ms².

3. Check with weights: $(90/100)(1)+(10/100)(9)=0.9+0.9=1.8$. The groups supply identical total squared error because the larger per-request score offsets the smaller group size.

### Alternative example · Request size

**Problem.** Small requests took 2, 2, 4, 4 ms. Large requests took 8 and 10 ms. Compare pooled and size-specific predictions.

**Worked solution**

1. The total of all latencies is $12+18=30$ ms across six requests. A constant prediction is therefore 5 ms. Small requests contribute $2(2-5)^2+2(4-5)^2=20$ ms² and large ones $(8-5)^2+(10-5)^2=34$ ms².

2. The pooled MSE is $(20+34)/6=9$ ms². Now fit the small mean $12/4=3$ and large mean $18/2=9$ ms.

3. The small squared-error total becomes $2(2-3)^2+2(4-3)^2=4$. The large total becomes $(8-9)^2+(10-9)^2=2$. Combined MSE is $(4+2)/6=1$ ms². On these data the size label reduces the squared-error total from 54 to 6.

## Plain-English glossary

| Term | Meaning in these lessons |
|---|---|
| Target and feature | The target is what will be predicted. A feature is information available when making that prediction. |
| Correction and residual | A correction shifts a reading. A residual here is the observed target minus the prediction. |
| Training error | Score calculated on the same records used to fit the predictions. |
| Held-out error | Score calculated on separate records by applying a previously fitted rule. |
| Conditional mean | Average target value among the observations sharing the given feature value. |

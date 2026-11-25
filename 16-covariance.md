# Lesson 16 · Covariance of paired measurements

A payload measurement and a latency measurement can each have a mean and a variance. To describe how they vary together, keep both measurements attached to their original request.

## Lesson 16 · Center, multiply within rows, then average

**Guiding question.** When the first quantity exceeds its mean, does the second tend to be above or below its own mean?

**Observation and units.** One observation is a pair of measurements on the same record. Choose a record uniformly from a complete log. $N$ is the positive number of records, and the index $i$ runs over the integers from 1 to $N$. $X,Y$ stand for the two quantities on the selected record. Lowercase $x_i,y_i$ give their measured values on row $i$. The probability of selecting any particular row is $1/N$.

**Step 1. Center each column.** Calculate the two means separately:
$$
\mu_X=\frac{x_1+\cdots+x_N}{N},\qquad
\mu_Y=\frac{y_1+\cdots+y_N}{N}.
$$
The dots mean include each intervening record. Subtract the appropriate mean to get $x_i-\mu_X$ and $y_i-\mu_Y$. A positive deviation means above that column's mean. A negative deviation means below. Averaging either centered column alone returns zero because its deviations sum to zero.

**Step 2. Preserve the pairing.** For each row multiply its two deviations. Same-sign deviations give a positive product. Opposite signs give a negative product. If either measurement sits exactly at its mean, the product is zero. Multiplication gives large joint departures greater influence through their magnitude. Counting only positive and negative rows would discard that magnitude.

**Step 3. Define covariance.** Average the products:
$$
\operatorname{Cov}(X,Y)
=\mathbb E[(X-\mu_X)(Y-\mu_Y)]
=\frac1N\sum_{i=1}^{N}(x_i-\mu_X)(y_i-\mu_Y).
$$
The expectation $\mathbb E$ averages over the specified selection process. Here it assigns every paired record weight $1/N$, giving the finite-sum formula. With unequal record probabilities each product would carry its corresponding probability. The sign tells which direction of joint deviation dominates in the weighted sum. The unit is the product of the two measurement units. The divisor $N$ describes this finite distribution.

**Derived identity for checking the arithmetic.** Distribute the two factors before taking expectations:
$$
(X-\mu_X)(Y-\mu_Y)=XY-X\mu_Y-\mu_XY+\mu_X\mu_Y.
$$
By **linearity of expectation** average each term. The means are constants, so $\mathbb E[X\mu_Y]=\mu_Y\mathbb E[X]=\mu_Y\mu_X$ and $\mathbb E[\mu_XY]=\mu_X\mu_Y$. The expectation of the final constant is itself. Collecting the three constant products leaves one negative copy:
$$
\operatorname{Cov}(X,Y)=\mathbb E[XY]-\mu_X\mu_Y.
$$
Here $\mathbb E[XY]$ means multiply the raw measurements **on each row** and then average. Multiplying the two column means is a separate operation. Their difference measures the contribution of the pairing.

**Connection to variance and regression.** If both measurements are the same quantity, their product of deviations on every row becomes $(x_i-\mu_X)^2$. Substituting in the definition gives $\operatorname{Cov}(X,X)=\operatorname{Var}(X)$. For two different measurements, covariance supplies the direction of a simple least-squares slope with an intercept when the input has positive variance. The units also reveal why a further calculation is needed for a slope: payload–latency covariance uses kB·ms, while latency change per unit payload uses ms/kB.

### Worked benchmark · Payload and latency

**Problem.** Select one of three requests uniformly. The (payload in kB, latency in ms) pairs are (1,4), (2,8) and (3,9). Calculate and interpret covariance.

**Worked solution**

1. Find the column means separately. Payload mean is $(1+2+3)/3=6/3=2$ kB. Latency mean is $(4+8+9)/3=21/3=7$ ms.

2. For row 1, deviations are $1-2=-1$ kB and $4-7=-3$ ms. Their product is $(-1)(-3)=+3$ kB·ms. This small, fast request contributes positively.

3. For row 2, deviations are $2-2=0$ and $8-7=1$. Their product is zero. For row 3 the deviations are $3-2=1$ and $9-7=2$, with product $+2$ kB·ms.

4. Average over all three rows: $(3+0+2)/3=5/3$ kB·ms. Payload and latency deviations in the same direction dominate the sum in this log.

5. Check independently: $\mathbb E[XY]=(1\cdot4+2\cdot8+3\cdot9)/3=(4+16+27)/3=47/3$. Subtract the product of means $2(7)=14=42/3$, leaving $5/3$. If decimal kB are converted to bytes, all payload deviations grow numerically by 1,000. The numerical covariance then grows by 1,000 with the new product unit.

### Practice 1 · Workers and duration

**Problem.** Choose uniformly from the (active workers, duration in seconds) records (1,9), (2,7) and (3,5). Compute covariance and interpret its sign.

**Worked solution**

1. Define $X$ as worker count and $Y$ as duration. Means are $\mu_X=(1+2+3)/3=2$ workers and $\mu_Y=(9+7+5)/3=7$ s.

2. Center row 1: $(1-2,9-7)=(-1,2)$, giving product $-2$. Row 2: $(2-2,7-7)=(0,0)$, giving zero. Row 3: $(3-2,5-7)=(1,-2)$, giving $-2$.

3. Add and divide by three: $\operatorname{Cov}(X,Y)=(-2+0-2)/3=-4/3$ worker·s. Both nonzero terms describe opposite-direction deviations. The higher-count run finishes sooner, and the lower-count run takes longer in these records.

4. Verify using raw products: $(1\cdot9+2\cdot7+3\cdot5)/3=38/3$. Subtract the mean product $2\cdot7=42/3$ to recover $-4/3$ worker·s.

### Practice 2 · The same measurements paired differently

**Problem.** Log A contains (payload kB, latency ms) pairs (1,2), (2,4), (3,6). Log B contains (1,6), (2,4), (3,2). Each row has equal weight. Calculate both covariances and explain the difference.

**Worked solution**

1. Both payload columns have mean $(1+2+3)/3=2$ kB. Both latency columns have mean $(2+4+6)/3=4$ ms. Reordering the latency values leaves each separate column's mean and variance unchanged.

2. Log A's payload deviations are $-1,0,1$ and its latency deviations are $-2,0,2$. Multiply in record order: $(-1)(-2)=2$, $0(0)=0$, $1(2)=2$. Average to get $(2+0+2)/3=4/3$ kB·ms.

3. Log B's payload deviations still read $-1,0,1$. Its latency deviations now read $2,0,-2$. Products are $(-1)(2)=-2$, 0 and $1(-2)=-2$. The average is $-4/3$ kB·ms.

4. The largest payload is paired with the longest time in A and the shortest time in B. A column summary gives the distribution of individual measurements. Covariance retains information about which values went together.

### Practice 3 · Zero covariance with a predictable outcome

**Problem.** Choose uniformly from controller records (signed position error in mm, correction energy in mJ) of (−2,4), (0,0), (2,4). Calculate covariance and predict the energy at zero and at error magnitude 2 mm.

**Worked solution**

1. Mean position error is $(-2+0+2)/3=0$ mm. Mean energy is $(4+0+4)/3=8/3$ mJ.

2. The energy deviations are $4-8/3=4/3$, $0-8/3=-8/3$, and $4-8/3=4/3$. Position deviations are already $-2,0,+2$ since the positional mean is zero.

3. Products are $(-2)(4/3)=-8/3$, $0(-8/3)=0$ and $(2)(4/3)=8/3$ mm·mJ. The sum is zero and division by three gives covariance zero.

4. Read the records themselves: zero position error always accompanies 0 mJ here. Either sign of 2 mm accompanies 4 mJ. Energy is exactly predictable from error magnitude on these records, despite the cancellation of signed products.

5. Check dependence with probabilities: across all rows $P(Y=4)=2/3$. Among rows with $X=2$, $P(Y=4\mid X=2)=1$. Those probabilities differ, establishing dependence directly. Zero covariance alone cannot establish independence.

## Plain-English glossary

| Term | Meaning in these lessons |
|---|---|
| Paired observation | Two values measured on the same record. |
| Centering | Subtract each quantity’s own mean from each of its observed values. |
| Covariance | Average product of the two centered measurements, with pairing preserved. |
| Uncorrelated | Covariance equals zero. Dependence can still be present, as in the position–energy exercise. |

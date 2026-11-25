# Lessons 9–13 · From individual records to distributions and spread

These five lessons ask different questions of one log. A random variable specifies the measurement. Its distribution describes how often values occur. A percentile sets a deadline, expectation gives an average, and variance measures spread around that average.

**Lesson topics**

- Lesson 9 · Random variables and probability mass functions
- Lesson 10 · Cumulative distribution functions and deadlines
- Lesson 11 · Nearest-rank percentiles and sharp bounds
- Lesson 12 · Expected value and signed measurement error
- Lesson 13 · Variance and standard deviation

## Lesson 9 · Random variables and probability mass functions

**Guiding question.** Which records give the numerical output you are counting?

A **random variable** $X$ is a function. Its input $\omega$ is a possible outcome, and its output $X(\omega)$ is a number with a specified meaning. For example, if $\omega$ is a build record, $X(\omega)$ can count compiler calls. The function can be deterministic even when the selected input is random.

The notation $P(X=x)$ asks for the probability of the *event* containing all records that the function maps to the number $x$. In a uniform log of $N$ records, $n_x$ of which yield $x$, the **probability mass function** is $p_X(x)=P(X=x)=n_x/N$. Grouping outcomes adds their weights. The list of values with positive probability is the support.

### Compilation attempts per build

**Problem.** Among 80 builds, 20 skipped compilation, 36 succeeded on the first attempt, 12 stopped after one failure, and 12 succeeded on a second attempt. Give the distribution of compiler calls.

**Worked solution**

1. Define $X(\omega)$ as the number of calls made by build record $\omega$. The unit is compiler calls. A uniformly chosen build has weight $1/80$.

2. Group by output of the function: zero calls for 20 cached builds, one call for $36+12=48$ builds, and two for 12 builds. Final success does not determine call count by itself.

3. For each output divide the qualifying count by all 80: $P(X=0)=20/80=0.25$, $P(X=1)=48/80=0.60$, and $P(X=2)=12/80=0.15$.

4. Check both levels: $20+48+12=80$ builds and $0.25+0.60+0.15=1$. All records and all probability are accounted for.

### Call count and the success indicator

**Problem.** Select one of 100 requests uniformly. Fifty succeeded from cache, 30 succeeded with one call, 15 failed with one call and five succeeded with two. Find the probability of one call, of success, and of both.

**Worked solution**

1. Define $X$ as database calls and $Y$ as a success indicator. In $X=1$ the numeral counts one call. In $Y=1$ it encodes a successful outcome.

2. Select by call count alone: $30+15=45$ qualify, so $P(X=1)=45/100=0.45$.

3. Select by success alone: $50+30+5=85$ qualify, so $P(Y=1)=85/100=0.85$.

4. Apply both conditions to the same request. Only the 30 one-call successes qualify, so $P(X=1,Y=1)=30/100=0.30$. The joint probability is at most each individual probability, as a subset check requires.

### Including a threshold observation

**Problem.** Of 40 jobs, 10 waited 0 s, 18 waited 2 s, 8 waited 5 s, and 4 waited 9 s. What fraction waited at least 5 s? What fraction waited more than 5 s?

**Worked solution**

1. Let $W$ output the selected job's wait in seconds. For $W\ge5$, the possible qualifying values are $\{5,9\}$. Add their counts: $8+4=12$.

2. Divide by the original 40 jobs: $P(W\ge5)=12/40=0.30$. For the strict condition $W>5$, only $\{9\}$ remains, so $P(W>5)=4/40=0.10$.

3. Subtract the results: $0.30-0.10=0.20$. Multiplying by 40 recovers the eight jobs exactly at the endpoint.

## Lesson 10 · Cumulative distribution functions and deadlines

**Guiding question.** What fraction finishes by a particular time, even if nobody finishes at exactly that time?

The **cumulative distribution function** (CDF) $F_X(t)=P(X\le t)$ gives the fraction at or below a proposed threshold $t$. The input uses the units of $X$ and the output is a probability. For a discrete variable, add the mass of each distinct value $x$ satisfying $x\le t$. If the threshold moves across a gap in the observed values, the qualifying records stay the same and the CDF stays flat.

For $a<b$, the event $X\le b$ consists of two disjoint pieces, $X\le a$ and $a<X\le b$. Finite additivity and subtraction give $P(a<X\le b)=F_X(b)-F_X(a)$. Removing everything up to and including $a$ accounts for the strict inequality at the lower end.

### API deadlines at 30 and 40 ms

**Problem.** A log records 8 requests at 10 ms, 22 at 20 ms, 15 at 40 ms and 5 at 80 ms. Select one uniformly. Find the point probability at 30 ms and the CDF at 30 and 40 ms.

**Worked solution**

1. Let $L$ be the recorded latency in ms. Total count is $8+22+15+5=50$. A point question uses equality, so $P(L=30)=0/50=0$.

2. For $F_L(30)=P(L\le30)$ the qualifying values are 10 and 20. Add the counts and divide: $(8+22)/50=30/50=0.60$.

3. For $F_L(40)$, include the 15 records at exactly 40 as well: $(30+15)/50=45/50=0.90$. A 30 ms deadline succeeds whenever a request finishes earlier or exactly then, hence its 60% success rate.

### Under five versus at most five

**Problem.** In 60 cycles, 12 took 1 ms, 18 took 3 ms, 24 took 5 ms and 6 took 8 ms. Compare the two deadline specifications and identify the CDF.

**Worked solution**

1. “Under” selects times strictly below five, so count the 1 and 3 ms groups. $P(T<5)=(12+18)/60=30/60=0.50$.

2. “At most” includes equality. Add the 24 boundary cycles: $P(T\le5)=(30+24)/60=54/60=0.90$. This inclusive probability is $F_T(5)$.

3. Subtract counts, $54-30=24$ cycles. Subtract fractions, $0.90-0.50=0.40$. These quantities have different units, and $60(0.40)=24$ connects them.

### What cumulative counts reveal

**Problem.** Among 80 jobs, 50% waited at most 2 s and 75% at most 6 s. How many waited more than 2 and at most 6? Can we count those waiting exactly 6?

**Worked solution**

1. Let $W$ be seconds waited. The jobs at or below two seconds are all included in the group at or below six. Subtracting the smaller group isolates $(2,6]$.

2. $P(2<W\le6)=F_W(6)-F_W(2)=0.75-0.50=0.25$. Convert the fraction to a count: $80(0.25)=20$. Direct count check: $80(0.75)-80(0.50)=60-40=20$.

3. To see why a point count is undetermined, place the 20 jobs all at 5 s, then imagine placing them all at 6 s. Both assignments give the two reported CDF values. The exact-6 count can range from 0 to 20.

## Lesson 11 · Nearest-rank percentiles and sharp bounds

**Guiding question.** What is the smallest deadline that covers a required fraction of the records?

The **nearest-rank empirical quantile** at fraction $p$, where $0<p\le1$, is the smallest deadline at which at least fraction $p$ has finished. In a complete log of $N$ equally weighted records, a count $k$ reaches the target exactly when $k/N\ge p$. Multiply by positive $N$ to obtain $k\ge Np$. Counts are integers, so the first eligible rank is $k=\lceil Np\rceil$, where the ceiling means round upward to an integer, leaving an integer unchanged. Sort from smallest to largest using ranks starting at 1. The value at rank $k$ is the requested quantile. This specifies the convention explicitly.

### Controller targets of 70% and 71%

**Problem.** Forty cycles have durations 2, 4, 7 and 12 ms with counts 10, 18, 8 and 4. Find the smallest deadline for each target.

**Worked solution**

1. Sort by time and accumulate counts. At the four deadlines the counts finished are $10$, $10+18=28$, $28+8=36$ and $36+4=40$.

2. For 70%, the first qualifying rank is $\lceil40(0.70)\rceil=28$. Rank 28 lies among the 4 ms records. Before 4 ms at most ten have finished, proving minimality.

3. For 71%, the calculation gives $40(0.71)=28.4$. At least 29 whole records are needed. Only 28 finish by 4 ms, and 36 finish by 7 ms, so the deadline rises to 7 ms.

### Disk reads at the 85th percentile

**Problem.** Twenty-four reads have times 1, 3, 8 and 20 ms with counts 6, 10, 5 and 3. Find the 85th percentile and the actual fraction finished by then.

**Worked solution**

1. Multiply target by count: $24(0.85)=20.4$. Round up because 20 finished would fall short. The desired rank is 21.

2. Cumulative counts are $6,6+10=16,16+5=21,21+3=24$. Thus rank 21 has time 8 ms.

3. At the preceding time only $16/24\approx66.7\%$ had finished. At 8 ms the fraction is $21/24=7/8=87.5\%$. The discrete jump can exceed the target.

### Bound a percentile from two dashboard readings

**Problem.** In 100 requests, 89 finished by 20 ms and 96 by 50 ms, including the endpoints. Give the sharpest possible interval for the 95th percentile.

**Worked solution**

1. The required rank is $\lceil100(0.95)\rceil=95$. Since 89 is below 95, the 95th completion must occur strictly after 20 ms.

2. The count by 50 ms is 96, already at least 95, so the 95th completion can occur at or before 50 ms. Together these imply $20<q_{0.95}\le50$ ms, where $q_{0.95}$ denotes the deadline.

3. Sharpness check: place ranks 90–96 at any chosen time strictly above 20 and at most 50. The two counts remain valid and rank 95 takes that time. Exactly 20 ms is excluded.

### Queue follow-up

**Problem.** Fifty jobs were logged. Exactly 44 finished by 8 s and 47 by 12 s. Bound the nearest-rank 90th percentile.

**Worked solution**

1. The target is $\lceil50(0.90)\rceil=45$. Up to and including the lower deadline, only 44 records qualify.

2. Ranks 45, 46 and 47 therefore fall after 8 s and by 12 s. In particular $8<q_{0.90}\le12$ s.

3. If all three finish at 12 s, the upper bound is attained. If they finish just after 8 s the percentile can approach the lower endpoint. The dashboard gives no finer timing.

### Forty-request follow-up

**Problem.** For 40 requests, 35 finished within 10 ms and 38 within 16 ms, inclusive. Bound the 90th percentile.

**Worked solution**

1. Compute $\lceil40(0.90)\rceil=36$. Rank 36 is the first record beyond the 35 already counted by 10 ms.

2. Since $35<36\le38$, rank 36 falls in the additional group counted by 16 ms. Thus $10<q_{0.90}\le16$ ms.

3. The strict lower bound describes a time later than 10 ms. The upper endpoint is attainable by placing all three additional completions at 16 ms.

## Lesson 12 · Expected value and signed measurement error

**Guiding question.** What average would you obtain if every individual observation received its correct weight?

Let $x$ range over possible numerical outputs and $p_X(x)$ give each output's probability. The **expected value** or mean is defined by $\mu=\mathbb E[X]=\sum_x x\,p_X(x)$. Multiply each possible numerical value by its share of the observations and add. Probabilities have no units, so the mean has the same units as $X$. It can lie between possible individual values.

For a uniform log, substitute $p_X(x)=n_x/N$. Distributing division over addition gives
$$
\mathbb E[X]=\sum_x x\frac{n_x}{N}=\frac{\sum_x n_x x}{N}.
$$
Multiplication by $n_x$ reconstructs repeated records. Zero-valued measurements still count in $N$.

For a fixed reference $r$, **linearity of expectation** gives $\mathbb E[R-r]=\mathbb E[R]-r$. To see the subtraction explicitly, sum $(x-r)p_X(x)$ over readings. Distribute the product to obtain $\sum_x xp_X(x)-r\sum_x p_X(x)$. The final sum of probabilities is one, leaving the mean reading minus $r$.

### Database calls per request

**Problem.** From 100 requests, 50 have zero database calls, 45 have one and five have two. Find the expected number of calls for a uniformly chosen request.

**Worked solution**

1. Convert counts to probabilities of the three numerical values: $50/100=0.50$, $45/100=0.45$, $5/100=0.05$.

2. Multiply each value by its probability: $0(0.50)=0$, $1(0.45)=0.45$, $2(0.05)=0.10$. Add to obtain $\mathbb E[X]=0.55$ calls per request.

3. Check using totals: 45 one-call requests produce 45 calls, five two-call requests produce ten, for 55 calls across 100 requests. A single request can have an integer count while the average is fractional.

### Temperature sensor bias

**Problem.** The reference is 100 °C. Readings are 98 °C twice, 100 °C five times and 103 °C three times. Find the mean signed error using reading minus reference.

**Worked solution**

1. Subtract the reference first: $98-100=-2$, $100-100=0$, $103-100=3$ degrees. Negative means low under this sign convention.

2. Reconstruct the total by count: $2(-2)+5(0)+3(3)=-4+0+9=5$ degrees across ten readings.

3. Divide by ten: $5/10=+0.5$ °C. Check the raw average $[2(98)+5(100)+3(103)]/10=(196+500+309)/10=100.5$ °C. It is half a degree above the reference.

### Pressure calibration

**Problem.** Reference 50 kPa. The log contains four readings of 49 kPa, ten of 50 and six of 52. Give the error distribution, calculate its mean and verify against the raw mean.

**Worked solution**

1. One uniform record is the observation. The errors in kPa are $49-50=-1$, $50-50=0$ and $52-50=+2$.

2. The corresponding probabilities are $4/20=0.20$, $10/20=0.50$ and $6/20=0.30$. Thus $\mathbb E[E]=(-1)(0.20)+0(0.50)+2(0.30)=-0.20+0.60=+0.40$ kPa.

3. Raw-pressure check: $[4(49)+10(50)+6(52)]/20=(196+500+312)/20=1008/20=50.4$ kPa. Subtracting 50 gives the same 0.4 kPa high on average.

### Early and late cycles

**Problem.** A system targets 20 ms. Three cycles finished 4 ms early, eight on time and one 4 ms late. Explain the distinct-value average and calculate the expected signed error.

**Worked solution**

1. With actual minus target, early is $-4$ ms and late is $+4$ ms. The computation $(-4+0+4)/3$ gives one third of the weight to each distinct error value.

2. Select a cycle uniformly. Its error values have weights $3/12,8/12,1/12$. Multiply and sum: $(-4)(3/12)+0(8/12)+4(1/12)=(-12+0+4)/12=-8/12$.

3. Reduce the fraction to $-2/3$ ms. Early and late magnitudes match, but early occurs three times as often. The net result is two thirds of a millisecond early on average.

### Solve for a missing reading

**Problem.** The reference is 80 °C. Eight readings have mean signed error +0.5 °C. Three readings were 79 °C and four were 81 °C. Find the eighth.

**Worked solution**

1. Let $x$ be the missing temperature, so its error is $x-80$. The known errors sum to $3(79-80)+4(81-80)=-3+4=1$.

2. Set up the mean equation $[1+(x-80)]/8=0.5$. Multiply both sides by 8: $1+(x-80)=4$. Subtract one: $x-80=3$. Add 80: $x=83$ °C.

3. Check with raw data: $[3(79)+4(81)+83]/8=(237+324+83)/8=644/8=80.5$ °C. Its difference from the reference is the reported +0.5 °C.

## Lesson 13 · Variance and standard deviation

**Guiding question.** How widely do measurements vary about their own mean, and what units describe that spread?

How far do readings spread around their own mean? Signed deviations always average to zero because $\mathbb E[X-\mu]=\mathbb E[X]-\mu=0$. Square each deviation before averaging so deviations on either side contribute positively. **Variance** is defined by
$$
\operatorname{Var}(X)=\mathbb E[(X-\mu)^2]
=\frac{\sum_x n_x(x-\mu)^2}{N}.
$$
The last equality uses uniform record selection in a complete log. For each distinct value, subtract the mean, square, multiply by the number of records at that value, sum, and divide by the total record count. The **standard deviation** $\sigma=\sqrt{\operatorname{Var}(X)}$ takes the nonnegative square root to restore the original units. The divisor $N$ describes the distribution specified here. Estimating a larger population's variance from a sample is a separate question.

### Controller cycle spread

**Problem.** The four durations are 8, 8, 12 and 12 ms. Choose one cycle uniformly. Calculate mean, deviations, variance and standard deviation.

**Worked solution**

1. Find the common center: $(8+8+12+12)/4=40/4=10$ ms. Subtract 10 in record order, giving $-2,-2,2,2$ ms.

2. Square deviations separately: $(-2)^2=4$ and $(+2)^2=4$. Each of the four terms has units ms². Average them: $(4+4+4+4)/4=4$ ms².

3. Take the nonnegative square root: $\sqrt{4\ \mathrm{ms}^2}=2$ ms. Here each observed duration is exactly one standard deviation from the mean. This exact distance is a feature of this particular log.

### Repeatability versus calibration offset

**Problem.** Reference pressure is 100 kPa. A reports 103 four times. B reports 98 twice and 102 twice. Compare mean signed errors and spread.

**Worked solution**

1. For A, each error relative to the reference is $103-100=3$ kPa. Their average is +3 kPa. A's own mean is 103, so its four deviations around that mean are all zero. Its variance is $0/4=0$ kPa².

2. For B the errors from the reference are $-2,-2,2,2$ kPa. Their mean is zero. The raw-reading mean is $(98+98+102+102)/4=100$ kPa.

3. B's deviations around its own mean therefore also happen to be $-2,-2,2,2$ kPa. Its variance is $(4+4+4+4)/4=4$ kPa² and its standard deviation is 2 kPa. A repeats exactly in these records. B has zero mean offset.

### Grouped latency with one slow request

**Problem.** Six requests took 2 ms, three took 4 ms and one took 8 ms. Find mean, variance and standard deviation, and identify the largest contribution.

**Worked solution**

1. The log has $6+3+1=10$ requests. Total latency is $6(2)+3(4)+1(8)=32$ ms. Hence the mean is $32/10=3.2$ ms.

2. Subtract 3.2 from each group value: $-1.2,0.8,4.8$ ms. Square to get $1.44,0.64,23.04$ ms² per request in the respective groups.

3. Multiply these quantities by group counts: $6(1.44)=8.64$, $3(0.64)=1.92$, $1(23.04)=23.04$. Add the resulting sums to get $33.60$ ms².

4. Divide by ten requests: variance $=33.60/10=3.36$ ms². Standard deviation $=\sqrt{3.36}\approx1.83$ ms. The 8 ms group's contribution to the variance is $23.04/10=2.304$ ms², larger than either other group's contribution.

### Weighting follow-up

**Problem.** Six recorded durations equal 2 and four equal 7. Find the mean, variance and standard deviation for one uniformly chosen record.

**Worked solution**

1. Compute the mean $(6\cdot2+4\cdot7)/10=(12+28)/10=4$ duration units.

2. Deviations are $2-4=-2$ and $7-4=3$. Squared deviations are 4 and 9. Six records contribute 4 each and four contribute 9 each.

3. The variance is $[6(4)+4(9)]/10=(24+36)/10=6$ squared units. Its square root is $\sqrt6\approx2.45$ duration units. The fractions $6/10$ and $4/10$ weight both the mean and the variance.

## Plain-English glossary

| Term | Meaning in these lessons |
|---|---|
| PMF and CDF | A probability mass function gives the share at one value. A cumulative distribution function adds all shares up to and including a threshold. |
| Quantile and rank | The rank is a position in sorted records. The quantile is the measured value at the first rank satisfying the target under the nearest-rank convention. |
| Expectation | Average numerical value under the specified selection probabilities. |
| Bias and spread | Bias here is the mean signed error around a known reference. Spread is calculated around the measurements’ own mean. |
| Variance and standard deviation | Mean squared deviation from the mean, and its nonnegative square root. |

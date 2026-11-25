# Lesson 17 · Comparing sensors with Pearson correlation

## The engineering question

An instrument records pressure and a sensor's output voltage on each run. A colleague converts volts to millivolts. The covariance becomes a thousand times larger. Did the instrument become a thousand times more responsive, or did only the numerical scale change? Pearson correlation lets us describe the direction and degree of linear association on a common, dimensionless scale. A calibration slope in V/kPa answers a different question about physical responsiveness.

## Refresher · Keep the pairs together

For each row subtract the pressure mean from the pressure and the voltage mean from the voltage. Two negative deviations multiply to a positive value. An above-average pair also contributes positively. Opposite-sign deviations contribute negatively. A column of centered values sums to zero, yet its products with a second centered column can sum to a positive, negative or zero value.

**Fresh sign check.** Two equally weighted (pressure in kPa, voltage in V) records are (8, 1) and (12, 3). In a Python comment give the sign of each centered product before adding them. This is a warm-up, and either practice set in this file can serve as your correlation submission.

## Formula, assumptions and terminology

Treat the log as the entire distribution. One of its $N$ rows is selected uniformly, so every row has probability $1/N$. For row $i$ (an integer from 1 through $N$), write $x_i$ and $y_i$ for its two measurements. $X$ and $Y$ are the values on the selected row. Define the means $\mu_X=(\sum_i x_i)/N$ and $\mu_Y=(\sum_i y_i)/N$. The sum symbol means add the indicated quantity once per row.

Let $d_i=x_i-\mu_X$ and $e_i=y_i-\mu_Y$ denote the signed deviations on row $i$. In this complete-log model,

$$
C=\frac{\sum_i d_i e_i}{N},\qquad
\sigma_X=\sqrt{\frac{\sum_i d_i^2}{N}},\qquad
\sigma_Y=\sqrt{\frac{\sum_i e_i^2}{N}}.
$$

$C$ is covariance. Multiply deviations on the same row, then average. The two $\sigma$ symbols denote standard deviations. Square each column's deviations, average the squares, then take the nonnegative square root. This gives a measure of spread in the original units of each column.

A **standardized value** or **z-score** tells how many standard deviations a value lies above or below its mean. Divide $d_i$ by $\sigma_X$ and $e_i$ by $\sigma_Y$. Both spreads must be positive. On a single row, units cancel in each division. Define **Pearson correlation** $\rho$ by averaging the products of those standardized values:

$$
\begin{aligned}
\rho
 &=\frac1N\sum_i \left(\frac{d_i}{\sigma_X}\right)
                         \left(\frac{e_i}{\sigma_Y}\right)\\
 &=\frac{1}{\sigma_X\sigma_Y}\left(\frac1N\sum_i d_i e_i\right)\\
 &=\frac{C}{\sigma_X\sigma_Y}.
\end{aligned}
$$

In the second line we combine the two fractions and pull the denominator outside the sum because every row uses the same two spreads. The parenthesized average is covariance by definition. A positive rescaling of one column multiplies both covariance and that column's spread by the same factor, which cancels in the ratio.

**Range check.** The Cauchy–Schwarz inequality for the two columns of deviations says $|\sum_i d_i e_i|\le\sqrt{\sum_i d_i^2}\sqrt{\sum_i e_i^2}$. The vertical bars denote absolute value. Divide by $N$ on both sides to obtain $|C|\le\sigma_X\sigma_Y$. Dividing by the positive product of spreads gives $|\rho|\le1$. At either extreme the centered columns are proportional, so the data lie on a straight line. Zero covariance, hence zero correlation when both spreads are positive, can occur with a perfectly predictable curved relationship.

## Worked example · A pressure bench

| Run | Pressure (kPa) | Output (V) | Pressure deviation (kPa) | Voltage deviation (V) | Product (kPa·V) |
|---|---:|---:|---:|---:|---:|
| 1 | 9 | 3 | −1 | −7 | 7 |
| 2 | 9 | 11 | −1 | 1 | −1 |
| 3 | 11 | 9 | 1 | −1 | −1 |
| 4 | 11 | 17 | 1 | 7 | 7 |

1. Compute the means. Pressure totals $9+9+11+11=40$ kPa and voltage totals $3+11+9+17=40$ V. Each mean is numerically 10, in its respective unit.
2. Subtract the corresponding mean and multiply within each row. For example, run 1 contributes $(9-10)(3-10)=(-1)(-7)=7$ kPa·V. The product column averages to $(7-1-1+7)/4=3$ kPa·V.
3. Pressure variance is $(1+1+1+1)/4=1$ kPa², so its standard deviation is 1 kPa. Voltage variance is $(49+1+1+49)/4=25$ V², so its standard deviation is 5 V.
4. Divide covariance by the product of spreads: $\rho=3/(1\cdot5)=0.60$. Positive products outweigh the negative ones. At 9 kPa the output takes two distinct values, so a single straight line cannot pass through all four points.
5. In millivolts, the four voltage deviations become −7000, 1000, −1000 and 7000. The covariance becomes 3000 kPa·mV and the voltage standard deviation becomes 5000 mV. The recomputed correlation is $3000/(1\cdot5000)=0.60$. The value 0.60 is a unitless coefficient, not a 60% chance of an event.

## Hand practice

Treat each log as complete and each row as equally likely. Show means, paired products, both spreads and the correlation expression in Python comments. Interpret the result in a sentence. The new set below is an alternative to the earlier Lesson 17 exercises.

1. **Cooling fan.** A bench records (fan speed in thousands of rpm, outlet temperature in °C) as (1, 32), (2, 30), (3, 28). Predict the sign of the centered products, then compute covariance and correlation. Explain the size and sign of the coefficient from the records.
2. **A change of units.** A log records (payload in kB, processing time in ms) as (2, 4), (2, 8), (4, 6), (4, 10). Compute covariance, the two spreads and correlation. Then convert payload to bytes using 1000 bytes per kB. Identify which numerator and denominator factors change and recompute the ratio.
3. **A stuck sensor.** Three (reference temperature in °C, sensor voltage in V) records are (10, 2), (20, 2), (30, 2). Compute covariance and both spreads. Write the division required for correlation and explain what the observed constant voltage allows you to conclude.

<details>
<summary>Worked solutions · Open after your attempt</summary>

### Sign check

Mean pressure is $(8+12)/2=10$ kPa. Mean voltage is $(1+3)/2=2$ V. The products are $(-2)(-1)=+2$ and $(2)(1)=+2$ kPa·V. Both are positive; covariance is their average, 2 kPa·V.

### 1. Cooling fan

1. Means are $(1+2+3)/3=2$ thousand rpm and $(32+30+28)/3=30$ °C.
2. Center the columns to get $(-1,0,1)$ and $(2,0,-2)$. Multiply in order to obtain $(-2,0,-2)$. Covariance is $-4/3$ in (thousand rpm)·°C.
3. Squared speed deviations sum to 2 and squared temperature deviations sum to 8. Divide each by three, then take roots: $\sigma_X=\sqrt{2/3}$ thousand rpm and $\sigma_Y=\sqrt{8/3}$ °C.
4. Their product is $\sqrt{16/9}=4/3$ in product units. Correlation is $(-4/3)/(4/3)=-1$. Each increase of one thousand rpm accompanies a decrease of exactly two degrees in these records. Every point lies on a descending line.

### 2. Payload and processing time

1. Mean payload is $12/4=3$ kB and mean time is $28/4=7$ ms. Deviations are $(-1,-1,1,1)$ and $(-3,1,-1,3)$.
2. Multiply corresponding entries to get $3,-1,-1,3$. Their sum is 4 and their mean is 1 kB·ms.
3. Payload squared deviations sum to 4, giving spread $\sqrt{4/4}=1$ kB. Time squared deviations sum to $9+1+1+9=20$, giving spread $\sqrt{20/4}=\sqrt5$ ms. Thus $\rho=1/(1\sqrt5)\approx0.447$.
4. In bytes the payload deviations are $(-1000,-1000,1000,1000)$. Every product gains a factor of 1000. The covariance is now 1000 byte·ms and the payload spread is 1000 bytes. The time spread stays $\sqrt5$ ms, so $\rho=1000/(1000\sqrt5)=1/\sqrt5$. The association is positive and imperfect in both representations.

### 3. Constant voltage

1. The means are $60/3=20$ °C and $6/3=2$ V. Temperature deviations are $(-10,0,10)$, while all voltage deviations are zero.
2. Every centered product equals zero. The covariance is $0/3=0$ °C·V.
3. Temperature spread is $\sqrt{(100+0+100)/3}=\sqrt{200/3}$ °C. Voltage spread is $\sqrt{0/3}=0$ V.
4. Pearson's ratio is $0/(\sqrt{200/3}\cdot0)=0/0$, which is undefined. Voltage stayed constant across the three tested temperatures. The data provide no voltage variation to standardize.

### Earlier Lesson 17 practice · Answer key

1. **Requests (2,3), (4,6), (6,9), in kB and ms.** The means are $12/3=4$ kB and $18/3=6$ ms. Deviations are $(-2,0,2)$ and $(-3,0,3)$. Products are $(6,0,6)$, so covariance is $12/3=4$ kB·ms. Spreads are $\sqrt{(4+0+4)/3}=\sqrt{8/3}$ kB and $\sqrt{(9+0+9)/3}=\sqrt6$ ms. Their product is $\sqrt{16}=4$ kB·ms, giving $\rho=4/4=1$. Each payload increase of 2 kB accompanies a latency increase of 3 ms.
2. **Builds (1,5), (2,3), (3,4), in MB and seconds.** Means are $6/3=2$ MB and $12/3=4$ s. Deviations $(-1,0,1)$ and $(1,-1,0)$ give products $(-1,0,0)$. Covariance is $-1/3$ MB·s. Each column has variance $2/3$ in its own squared units, so the product of spreads is $2/3$ MB·s and $\rho=(-1/3)/(2/3)=-1/2$. Converting to ms increases covariance to $-1000/3$ MB·ms and the product of spreads to $2000/3$ MB·ms. Dividing again gives $-1/2$.
3. **Worker counts and durations (4,9), (4,6), (4,3), in workers and seconds.** Mean worker count is 4 and mean duration is 6 s. Worker deviations are all zero and duration deviations are $(3,0,-3)$. All products vanish, so covariance is zero worker·s. Worker spread is zero and duration spread is $\sqrt{(9+0+9)/3}=\sqrt6$ s. Correlation asks for $0/(0\cdot\sqrt6)$, which is undefined. Every run used four workers, so the log supplies no comparison between worker counts.

</details>

## Progress

Covariance setup and the zero-covariance dependence example were demonstrated. A same-sign product check addresses the earlier Log A discrepancy. Correlation has been introduced and awaits a first submission. The next refresher can be selected from the submitted reasoning, particularly unit cancellation and the distinction between a zero numerator and a zero denominator.


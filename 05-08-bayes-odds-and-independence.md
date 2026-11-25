# Lessons 5–8 · Source rates, Bayes’ theorem, odds, and independence

Start with a source and predict a defect. Reverse the question to infer the source of a defect. Then test whether learning a second fact changes a probability.

**Lesson topics**

- Lesson 5 · Multiplication rule and law of total probability
- Lesson 6 · Bayes’ theorem in probability form
- Lesson 7 · Bayes’ theorem in odds form
- Lesson 8 · Independence, conditioning, and pooled rates

## Lesson 5 · Multiplication rule and law of total probability

**Guiding question.** How do percentages reported within individual machines translate to percentages of the entire batch?

Let $D$ mean defective and $M$ mean made by machine M. Starting from $P(D\mid M)=P(D\cap M)/P(M)$, multiply both sides by $P(M)>0$. This gives the **multiplication rule**:
$$
P(D\cap M)=P(M)P(D\mid M).
$$
Read the factors in order. Take the share of the whole batch from M, then retain the share of those components that are defective. The resulting share again refers to the whole batch. This identity requires no independence assumption.

A *partition* divides all outcomes into mutually exclusive, exhaustive groups. If $A$ and $B$ partition the sources, every defective component belongs to exactly one of $D\cap A$ and $D\cap B$. Finite additivity allows the two weights to be added. Substituting the multiplication rule produces the **law of total probability**:
$$
P(D)=P(D\cap A)+P(D\cap B)=P(A)P(D\mid A)+P(B)P(D\mid B).
$$
Each rate is weighted by its group's share. With more sources, include one term per source.

### Convert a machine-specific rate into a batch contribution

**Problem.** M makes 30% of components and 8% of M's components are defective. Find the chance a uniformly selected component is both defective and from M. Can the overall defect rate be found?

**Worked solution**

1. Translate the words. The 30% describes the whole batch, so $P(M)=0.30$. “Of M's components” restricts the denominator, so $P(D\mid M)=0.08$.

2. Use the multiplication rule: $P(M\cap D)=P(M)P(D\mid M)=0.30(0.08)=0.024$. In a basis of 1,000 parts, 300 are from M and 24 of those are defective.

3. The total rate includes defects outside M. Their source share is $1-0.30=0.70$. With outside defect rate $r$, $P(D)=0.024+0.70r$. The missing number is $r$ or an equivalent count of outside defects.

### Recover a within-machine percentage

**Problem.** Q supplied 20% of a batch. Of the entire batch, 3% are defective components made by Q. Find the defect rate within Q.

**Worked solution**

1. Translate the second percentage as the joint probability $P(D\cap Q)=0.03$. Its denominator is all components.

2. Restrict to Q by dividing the joint weight by $P(Q)=0.20$: $P(D\mid Q)=0.03/0.20=3/20=0.15$.

3. On a 100-component basis, all three qualifying defects sit among 20 Q components. This explains the 15% within-Q rate.

### One numerator, two denominators

**Problem.** Of 100 components, 25 came from M and five of those were defective. One component is selected uniformly. Find $P(M)$, $P(D\cap M)$ and $P(D\mid M)$.

**Worked solution**

1. Choose the reference group for each quantity. Selection starts from all 100, so $P(M)=25/100=0.25$.

2. Both defective and from M means five qualifying components in the original 100: $P(D\cap M)=5/100=0.05$.

3. Given M, those same five are judged against the 25 from M: $P(D\mid M)=5/25=0.20$. Check $0.25(0.20)=0.05$.

4. There are $100-25=75$ outside M. If $k$ of these were defective then $P(D)=(5+k)/100$. Nothing in the data specifies $k$.

### Assemble the contributions of two lines

**Problem.** A supplies 70% of a batch with 2% defective. B supplies 30% with 6% defective. Each component has exactly one source. Find both joint probabilities and the overall defect rate.

**Worked solution**

1. Make $A,B$ source events and $D$ the defect event. The four inputs are $P(A)=0.70$, $P(B)=0.30$, $P(D\mid A)=0.02$, $P(D\mid B)=0.06$.

2. For A, multiply the source share by the within-A rate: $P(D\cap A)=0.70(0.02)=0.014$. For B the corresponding weight is $P(D\cap B)=0.30(0.06)=0.018$.

3. Add the two disjoint joint probabilities: $P(D)=0.014+0.018=0.032$. With 1,000 components these are 14 A defects and 18 B defects, or 32 in all.

### Change the source mixture

**Problem.** X has a 1% defect rate and Y a 9% rate. Batch 1 is 50% X and 50% Y. Batch 2 is 90% X and 10% Y. Calculate and explain each overall rate.

**Worked solution**

1. For either batch use $P(D)=P(X)P(D\mid X)+P(Y)P(D\mid Y)$. The conditional rates remain 0.01 and 0.09. The batch determines their weights.

2. Batch 1 gives $0.50(0.01)+0.50(0.09)=0.005+0.045=0.050$, or 5%. Equal weights permit the ordinary average of 1% and 9%.

3. Batch 2 gives $0.90(0.01)+0.10(0.09)=0.009+0.009=0.018$, or 1.8%. Both answers lie between 1% and 9%. The second lies closer to X's rate because nine out of ten components are from X.

## Lesson 6 · Bayes’ theorem in probability form

**Guiding question.** A defect was observed. Among all defects, how many came from the proposed source?

A defect has already been observed. What share of all observed defects came from B? By conditional probability, $P(B\mid D)=P(B\cap D)/P(D)$. The intersection is unchanged if its names switch places. Apply the multiplication rule to that numerator:
$$
P(B\mid D)=\frac{P(D\mid B)P(B)}{P(D)},\qquad P(D)>0.
$$
This is **Bayes’ theorem**. The *prior* $P(B)$ is the source probability before learning defect status. The *posterior* $P(B\mid D)$ is the updated source probability. The *likelihood* of source B for this observation is $P(D\mid B)$ viewed as a function of the source, with the observation fixed. Likelihoods across proposed sources need not sum to one. The denominator $P(D)$ sums their prior-weighted contributions.

### Identify the source of a defect

**Problem.** A supplies 80% and B 20% of parts. Defect rates within A and B are 1% and 6%. A randomly selected part is defective. Find the chance it came from B.

**Worked solution**

1. Write the target $P(B\mid D)$. Its numerator must describe B and defective together. By the multiplication rule that share is $P(B)P(D\mid B)=0.20(0.06)=0.012$.

2. Build the denominator by the law of total probability. A's contribution is $0.80(0.01)=0.008$. All defects therefore weigh $0.012+0.008=0.020$.

3. Apply Bayes’ theorem: $P(B\mid D)=0.012/0.020=12/20=0.60$. B made 12 of 20 defects on a basis of 1,000 parts. The original 6% used B parts as its denominator.

### Equal defect rates

**Problem.** M supplies 30% and N supplies 70%. Each has a 4% defect rate. What is $P(M\mid D)$?

**Worked solution**

1. Using the two sources separately gives $P(D\cap M)=0.30(0.04)=0.012$ and $P(D\cap N)=0.70(0.04)=0.028$.

2. Their sum is $P(D)=0.040$. Normalize the M contribution: $0.012/0.040=0.30$.

3. Algebra check: $0.30(0.04)/[(0.30+0.70)(0.04)]=0.30$. The common 0.04 cancels, so the defect group preserves the original 30:70 split.

### Diagnose the missing denominator contribution

**Problem.** L supplies 25% at 12% defective. K supplies 75% at 4% defective. A proposed answer for L given defective divides L's defective contribution by K's.

**Worked solution**

1. Use 1,000 parts to make the groups visible. L has 250 parts and $250(0.12)=30$ defects. K has 750 and $750(0.04)=30$ defects.

2. The proposed ratio is $30/30=1$. Its denominator counts only K defects. The question selects from all $30+30=60$ defects.

3. Compute $P(L\mid D)=30/60=1/2$. Equivalently $0.25(0.12)/[0.25(0.12)+0.75(0.04)]=0.03/0.06$. A result can lie in the legal range and still use the wrong reference group.

## Lesson 7 · Bayes’ theorem in odds form

**Guiding question.** How strongly does one observation shift the balance between two competing explanations?

With two mutually exclusive, exhaustive alternatives $L,K$, the **odds** of L versus K are $O=P(L)/P(K)$. The denominator counts the competing alternative. For three A requests and nine B requests, the probability of A is $3/12=1/4$ and the odds of A versus B are $3/9=1/3$, or one A per three B.

For observed evidence $F$, divide Bayes’ expression for L by its expression for K. The evidence probability $P(F)$ occurs in both denominators and cancels:
$$
\frac{P(L\mid F)}{P(K\mid F)}
=\frac{P(F\mid L)P(L)/P(F)}{P(F\mid K)P(K)/P(F)}
=\underbrace{\frac{P(L)}{P(K)}}_{\text{prior odds}}
 \underbrace{\frac{P(F\mid L)}{P(F\mid K)}}_{\text{likelihood ratio}}.
$$
The left side is the **posterior odds**. This ratio form assumes nonzero denominators. To convert odds $O$ to probability $p$, use the two exhaustive alternatives: $O=p/(1-p)$. Multiply by $1-p$, collect terms to get $O=p+Op=p(1+O)$, then divide to get $p=O/(1+O)$.

### Timeout odds versus probability

**Problem.** Of 12 timed-out requests, three came from service A and nine from B. Calculate the probability and odds of A.

**Worked solution**

1. Work entirely inside the 12 timeouts. The probability denominator counts all 12, so $p=3/12=1/4$.

2. The odds denominator counts the competing B cases, giving $O=3/9=1/3$. This is one A for every three B.

3. Convert back using counts: one A in $1+3=4$ total gives $1/4$. This check keeps the direction A versus B fixed.

### Update after a flag

**Problem.** L made 200 and K made 800 of 1,000 parts. L's flag rate is 30% and K's is 5%. A selected part was flagged. Find prior odds, likelihood ratio, posterior odds and probability of L.

**Worked solution**

1. Prior odds compare source counts before observing a flag: $O_{\rm prior}=200/800=1/4$.

2. For the observed evidence, the likelihood ratio in the same direction is $P(F\mid L)/P(F\mid K)=0.30/0.05=6$. The flag is six times as probable on an L part.

3. Multiply: $O_{\rm post}=(1/4)6=3/2$. That is three L flags per two K flags. The probability is $3/(3+2)=3/5=0.60$.

4. Verify from the raw counts: $200(0.30)=60$ and $800(0.05)=40$. Of $60+40=100$ flags, 60 are from L.

### Use silence as evidence

**Problem.** Of 1,000 logged runs, 100 were faulty and 900 healthy. An alarm sounded on 70% of faulty runs and 20% of healthy runs. You learn the selected run had no alarm. Find the fault probability.

**Worked solution**

1. Write $S$ for silence. Within each group take the complement: $P(S\mid F)=1-0.70=0.30$ and $P(S\mid H)=1-0.20=0.80$. These are probabilities of silence given health status.

2. The likelihood ratio for faulty versus healthy is $0.30/0.80=3/8$. Prior odds are $100/900=1/9$. Posterior odds are $(1/9)(3/8)=1/24$.

3. Convert to probability: one faulty for 24 healthy means $1/(1+24)=0.04$. Compare with the prior $100/1000=0.10$. Silence lowers the fault probability.

4. Check directly: $100(0.30)=30$ faulty runs were silent and $900(0.80)=720$ healthy runs were silent. $30/(30+720)=30/750=0.04$.

### Why multiplying a probability by a likelihood ratio fails

**Problem.** X serves 20% of requests, Y serves 80%. Timeout rates are 12% for X and 4% for Y. Test the proposed posterior $0.20(0.12/0.04)=0.60$.

**Worked solution**

1. Identify the operands. The first is a prior probability. The second is a likelihood ratio with direction X versus Y. The odds form takes prior odds as its first factor.

2. Convert to prior odds $0.20/0.80=1/4$. Multiply by $0.12/0.04=3$ to obtain posterior odds $3/4$.

3. In probability units the answer is $3/(3+4)=3/7$. Of 1,000 requests, X accounts for $200(0.12)=24$ timeouts and Y for $800(0.04)=32$. The count result $24/(24+32)=3/7$ verifies the setup.

## Lesson 8 · Independence, conditioning, and pooled rates

**Guiding question.** Does knowing the first label change the probability of the second? Does that answer change if load or file size is fixed?

**Independence** of events $A,B$ is defined by $P(A\cap B)=P(A)P(B)$. For $P(A)>0$, dividing both sides by $P(A)$ gives $P(B\mid A)=P(B)$. Learning A leaves the probability of B unchanged. Disjoint events of positive probability fail this test: on a fair die $\{3\}$ and $\{4\}$ have intersection probability zero while the product is $1/36$.

**Counting form for the station problem.** In $N$ uniformly weighted records let $a$ be the number flagged by station A, $b$ the number flagged by B, and $h$ the number flagged by both. Independence requires $h/N=(a/N)(b/N)$. Multiply through by $N$ to get $h=ab/N$. If the required count is fractional, exact independence is impossible for those margins in that finite log.

**Conditional independence** asks whether learning a second label changes a probability *within a specified group*. For failure $F$, library label $L$, and file size $S$, compare $P(F\mid S=s,L=\ell)$ with $P(F\mid S=s)$ at every observed size $s$ and library $\ell$ having positive probability. For a binary failure outcome, equality for failure gives equality for success by the complement rule. These claims describe the supplied records under uniform selection.

### Two faces of one die

**Problem.** On a fair die, compare “3 occurred” with “4 occurred.” Are the events equally likely, disjoint, and independent?

**Worked solution**

1. Each face has probability $1/6$, so their marginal probabilities match.

2. No single roll satisfies both. Their intersection is empty and has probability zero.

3. Independence would require $(1/6)(1/6)=1/36$ for the intersection. Zero differs from $1/36$. Equivalently, among rolls showing 3, the chance of 4 is zero.

### Two inspection stations

**Problem.** Of 120 parts, A flagged 30, B flagged 48 and both flagged 18. What would the overlap have to be for A's list to have the same B-flag rate as the whole batch?

**Worked solution**

1. Across the batch B's rate is $48/120=0.40$. Inside A's group the observed rate is $18/30=0.60$. The same B flag is being counted in two different reference groups.

2. Call the required overlap $h$. To keep the 40% rate among A's 30 parts, set $h/30=0.40$. Multiply by 30 to get $h=12$ parts.

3. Compare 18 observed with 12 required. A's list contains six more B flags than exact independence would produce with these margins.

### Camera and gauge

**Problem.** A camera flags 45 of 150 parts and a gauge flags 50. Find the overlap required for exact independence.

**Worked solution**

1. The overall gauge rate is $50/150=1/3$. The camera-flagged group contains 45 parts.

2. To preserve the gauge rate within that group, $h/45=1/3$. Multiply by 45: $h=15$.

3. Check $15/150=0.10=(45/150)(50/150)$. The two margins determine a required overlap. The actual overlap would have to be observed to test it.

### CPU and latency alerts

**Problem.** Of 200 jobs, 50 have CPU alerts, 80 have latency alerts and ten have both. Compare the observed overlap with independence.

**Worked solution**

1. Latency alerts occur on $80/200=0.40$ of all jobs. Among CPU alerts the rate is $10/50=0.20$.

2. Under independence, multiply group size by overall rate: $50(0.40)=20$ jobs with both.

3. The actual ten are below 20. In this log the presence of a CPU alert reduces the conditional proportion with a latency alert.

### Low and high load

**Problem.** Low load has 80 cycles, 20 temperature alerts, 16 vibration alerts and four with both. High load has 80, 60, 48 and 36 respectively. Test independence within each load and after pooling.

**Worked solution**

1. Low load: vibration rate $16/80=0.20$. Expected overlap under independence $20(0.20)=4$, equal to the observed four.

2. High load: vibration rate $48/80=0.60$. Required overlap $60(0.60)=36$, equal to the observed 36.

3. Pool the counts: 160 cycles, $20+60=80$ temperature alerts, $16+48=64$ vibration alerts and $4+36=40$ with both. Required pooled overlap is $80(64/160)=32$.

4. The observed 40 differs from 32. High load has more of both alert types. Changing the mixture by selecting temperature-alert records changes the vibration rate.

### File compression

**Problem.** Short files had 3 failures among 60 A files and 2 among 40 B files. Long files had 12 among 40 A files and 9 among 30 B files. Does knowing library change the failure probability once length is given?

**Worked solution**

1. For short files the reference groups are the 60 A and the 40 B cases. The rates are $3/60=0.05$ and $2/40=0.05$. Their pooled rate is $(3+2)/(60+40)=0.05$.

2. For long files the rates are $12/40=0.30$ and $9/30=0.30$. Their pooled rate is $21/70=0.30$.

3. Both comparisons match at fixed length. The data exhibit conditional independence of failure and library given length.

### Timeout rate within load

**Problem.** Normal load has 4 timeouts in 80 A requests and 2 in 40 B requests. Peak load has 10 in 50 A and 18 in 60 B. Is the rate the same for both servers within every load?

**Worked solution**

1. Compute the normal comparison: $4/80=2/40=0.05$. Both servers agree for this group.

2. Compute the peak comparison: $10/50=0.20$ while $18/60=0.30$. The pooled peak rate is $28/110\approx0.255$, which conceals the difference.

3. The original statement says every load level. Peak load supplies a counterexample, so the statement fails.

### Why build teams have different overall failure rates

**Problem.** A has 2 failures in 40 short and 12 in 60 long builds. B has 4 in 80 short and 4 in 20 long. Compare the teams overall and at fixed length.

**Worked solution**

1. Within teams, first add failures and totals separately. A's rate is $(2+12)/(40+60)=0.14$. B's rate is $(4+4)/(80+20)=0.08$.

2. Hold length fixed. Short rates are $2/40=4/80=0.05$. Long rates are $12/60=4/20=0.20$.

3. Weight these rates by each team's length mix. A: $0.40(0.05)+0.60(0.20)=0.02+0.12=0.14$. B: $0.80(0.05)+0.20(0.20)=0.04+0.04=0.08$. A ran a larger fraction of the higher-failure long builds.

### Version and request size

**Problem.** Small requests had 6 timeouts in 60 V1 and 2 in 20 V2. Large requests had 8 in 20 V1 and 24 in 60 V2. Compare within-size and overall rates.

**Worked solution**

1. Calculate within each size: $6/60=2/20=0.10$ for small, and $8/20=24/60=0.40$ for large.

2. Combine by version: V1 had $6+8=14$ timeouts in $60+20=80$ requests, or 17.5%. V2 had $2+24=26$ in $20+60=80$, or 32.5%.

3. Large requests make up $20/80=25\%$ of V1 and $60/80=75\%$ of V2. This explains the pooled difference despite identical within-size rates.

## Plain-English glossary

| Term | Meaning in these lessons |
|---|---|
| Joint, conditional, marginal | Joint means both conditions on one record. Conditional specifies a reference group. Marginal means the individual probability after counting across the other categories. |
| Prior and posterior | Probability of a proposed source before and after incorporating the stated observation. |
| Likelihood ratio | Probability of the same observation under the first proposed source divided by its probability under the second. The order fixes the direction. |
| Independence | The joint probability equals the product of the individual probabilities. Conditional independence applies this test inside each specified group. |

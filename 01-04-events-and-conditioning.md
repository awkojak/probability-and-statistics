# Lessons 1–4 · Building a finite probability model

Begin with what one observation records. Then identify which records satisfy a statement, combine statements, and finally change the reference group when information arrives.

**Lesson topics**

- Lesson 1 · Outcomes, events, and probability weights
- Lesson 2 · Complements
- Lesson 3 · Intersections, inclusive unions, and exactly one
- Lesson 4 · Conditional probability and the reference group

## Lesson 1 · Outcomes, events, and probability weights

**Guiding question.** If the same procedure can produce several results, what would a probability of one of those results mean?

An *experiment* is a specified procedure for producing an observation, such as rolling once and recording the upward face. An *outcome* $\omega$ is one possible result. The *sample space* $\Omega$ is the set of all possible results at the chosen level of detail. An *event* $A$ is a subset of $\Omega$. It occurs when the recorded outcome belongs to that subset. Thus $5$ is a die face, $\{5\}$ is the event of rolling that face, and $P(\{5\})$ is a unitless number.

**Finite probability assignment.** Give each distinct outcome $\omega$ a weight $p_\omega=P(\{\omega\})$. Every weight is nonnegative and all weights sum to one. By **finite additivity**, the probability of an event is the sum of the weights of its members:
$$
P(A)=\sum_{\omega\in A}p_\omega.
$$
The summation means to visit each distinct outcome in $A$ exactly once. If there are $N$ equally likely outcomes, their common weight $p$ satisfies $Np=1$, so $p=1/N$. An event with $k$ members therefore has probability $k/N$. Equal weighting is the assumption behind the counting shortcut. Disjointness tells us when we can add weights.

**Reading the notation.** Braces collect the members of a set. The symbol $\in$ means “is a member of.” The index beneath $\sum$ says which members to visit. A weight is a share of the whole, so the unit is one and multiplying by 100 gives the percentage. A valid assignment can include a zero weight. In a finite model these rules completely specify the probability of every event.

### A fair die exceeds four

**Problem.** Roll a fair six-sided die once. Find the chance of obtaining a value greater than four.

**Worked solution**

1. Define the recorded quantity as the top face. Exactly one of $\Omega=\{1,2,3,4,5,6\}$ occurs.

2. Translate the strict inequality into membership. Four fails the test; five and six pass. Thus $A=\{5,6\}$.

3. Fairness assigns weight $1/6$ to each face. Finite additivity gives $P(A)=P(\{5\})+P(\{6\})=1/6+1/6=2/6=1/3$.

4. Check the denominator. There are six equally likely faces in the whole experiment and two qualifying faces. A third of the original probability is in $A$.

### Checking a machine-state table

**Problem.** One cycle ends in exactly one state. Normal, Warning, Shutdown and Sensor fault have probabilities 0.68, 0.17, 0.10 and 0.05. Is the assignment valid? What is the probability of Warning or Sensor fault?

**Worked solution**

1. Check each entry is between zero and one. Add them sequentially: $0.68+0.17=0.85$, then $0.85+0.10=0.95$, then $0.95+0.05=1$. These exhaustive states account for the whole cycle.

2. Define $B=\{\text{Warning},\text{Sensor fault}\}$. A cycle cannot finish in both recorded states, so add their weights: $P(B)=0.17+0.05=0.22$.

3. Interpret 0.22 as 22% of the probability of a single cycle. A count of $2/4$ assumes each state weighs 0.25, contradicting the supplied table.

4. Input check: using 0.668 for Normal would reduce the total by $0.680-0.668=0.012$, producing 0.988. That calculation uses a different input.

### Recording component identity or status

**Problem.** Select one of eight equally likely numbered components. Numbers 1–6 pass inspection and 7–8 fail. Build both sample spaces and give their probabilities.

**Worked solution**

1. Record identity: $\Omega_I=\{1,2,3,4,5,6,7,8\}$. Each individual selection carries $1/8$.

2. Record status instead: $\Omega_S=\{\text{pass},\text{fail}\}$. Six identity outcomes map to pass and two map to fail.

3. Sum each group's original weights: $P(\text{pass})=6(1/8)=0.75$ and $P(\text{fail})=2(1/8)=0.25$.

4. Check $0.75+0.25=1$. The two ways to record a selection describe the same physical procedure at different levels of detail.

### Three red balls and one blue ball

**Problem.** When is the chance of red $3/4$? Construct a valid selection rule that instead gives red probability $1/2$.

**Worked solution**

1. Distinguish physical objects by labels $R_1,R_2,R_3,B_1$. If each has probability $1/4$, then the red event has probability $1/4+1/4+1/4=3/4$.

2. For the requested alternative, allocate half the total weight to the single blue ball. Allocate the other half equally among the three red balls. Each gets $(1/2)/3=1/6$.

3. Check the assignment: $3(1/6)+1/2=1$. Summing only the red weights gives $3(1/6)=1/2$. Color counts alone never specified the selection weights.

### Disjoint outcomes and an incomplete assignment

**Problem.** A procedure returns one of $a,b,c,d$ with weights 0.10, 0.20, 0.30, 0.40. Calculate the probability of $a$ or $c$ and test a replacement of the last weight by 0.39.

**Worked solution**

1. Write the event $E=\{a,c\}$. Its pieces $\{a\}$ and $\{c\}$ are disjoint because one observation takes only one value.

2. Use additivity: $P(E)=P(\{a\})+P(\{c\})=0.10+0.30=0.40$. Disjointness describes the event sets. Their probabilities are the numbers 0.10 and 0.30.

3. For the proposed table, add $0.10+0.20+0.30+0.39=0.99$. With the same four exhaustive outcomes this leaves 0.01 unassigned, so the new assignment is invalid.

## Lesson 2 · Complements

**Guiding question.** How do you find the probability that a specified condition fails?

The *complement* $A^c$ contains every outcome of $\Omega$ outside $A$. Exactly one of $A$ and $A^c$ occurs. By finite additivity, $P(A)+P(A^c)=P(\Omega)=1$. Subtract $P(A)$ from both sides to get the **complement rule** $P(A^c)=1-P(A)$. This identity holds for unequal weights as well.

### Die roll greater than two

**Problem.** Roll a fair six-sided die. Find the probability of greater than two directly and through its complement.

**Worked solution**

1. Start with $\Omega=\{1,2,3,4,5,6\}$. The event is $A=\{3,4,5,6\}$. Remove those four faces from $\Omega$ to get $A^c=\{1,2\}$.

2. Uniform weights give $P(A)=4(1/6)=2/3$ and $P(A^c)=2(1/6)=1/3$.

3. The complement rule gives $P(A)=1-P(A^c)=1-1/3=2/3$, agreeing with the direct count.

### A device is available

**Problem.** A device is Ready, Busy, Offline or Fault with probabilities 0.72, 0.18, 0.07 and 0.03. Let $U$ mean Offline or Fault. Which states remain, and how probable are they?

**Worked solution**

1. Removing Offline and Fault from the complete state list leaves $U^c=\{\text{Ready},\text{Busy}\}$.

2. Sum the two remaining weights: $P(U^c)=0.72+0.18=0.90$.

3. Check via the states removed: $P(U)=0.07+0.03=0.10$, so $1-P(U)=1-0.10=0.90$. This probability includes a busy device.

### One remaining integer

**Problem.** A procedure records an integer from 1 through 10. Integers 1–9 together have probability 0.84. Find the complement and its probability.

**Worked solution**

1. List what is outside $B=\{1,\ldots,9\}$ but still inside $\Omega=\{1,\ldots,10\}$: only 10. Thus $B^c=\{10\}$.

2. Use the known weight of the whole set, $P(B^c)=1-P(B)=1-0.84=0.16$.

3. Check: uniformity would give $P(B)=9/10=0.90$. The supplied weight already rules out that assumption.

### Even and odd integers

**Problem.** For $\Omega=\{1,\ldots,8\}$ the event $C=\{2,4,6,8\}$ weighs 0.65. Find the remaining probability.

**Worked solution**

1. Remove the four listed even integers. The remaining event is $C^c=\{1,3,5,7\}$.

2. Since those two sets cover all eight outcomes without overlap, $P(C^c)=1-0.65=0.35$.

3. Counting $4/8=0.50$ would require equal weights. The stated even-event probability would then also be 0.50, contradicting 0.65.

### Three remaining letters

**Problem.** In $\Omega=\{a,b,c,d,e\}$, event $F=\{b,d\}$ weighs 0.62. Find its complement.

**Worked solution**

1. Cross out $b,d$. The remaining outcomes form $F^c=\{a,c,e\}$.

2. Compute $P(F^c)=1-P(F)=1-0.62=0.38$.

3. Check $0.62+0.38=1$. A count of $3/5$ would ignore the specified weights.

## Lesson 3 · Intersections, inclusive unions, and exactly one

**Guiding question.** How do you combine two conditions when one observation can satisfy both?

The *intersection* $A\cap B$ contains outcomes in both events. The *union* $A\cup B$ contains outcomes in at least one. Adding $P(A)$ and $P(B)$ counts every outcome in the intersection twice. Subtract its probability once to leave one copy. This is the two-event **inclusion–exclusion principle**:
$$
P(A\cup B)=P(A)+P(B)-P(A\cap B).
$$

### Pressure and Flow alarm records

**Problem.** One record shows neither alarm (0.40), Pressure only (0.25), Flow only (0.15) or both (0.20). Find the events for each alarm, both, at least one and exactly one. Calculate the last two probabilities.

**Worked solution**

1. Use $n,p,f,b$ for neither, Pressure only, Flow only and both. Pressure active is $A=\{p,b\}$ and Flow active is $B=\{f,b\}$. Membership in both gives $A\cap B=\{b\}$.

2. Membership in at least one gives $A\cup B=\{p,f,b\}$. By direct addition, $P(A\cup B)=0.25+0.15+0.20=0.60$. As a check, the excluded record is neither, with weight $1-0.60=0.40$.

3. Apply inclusion–exclusion separately. $P(A)=0.25+0.20=0.45$ and $P(B)=0.15+0.20=0.35$. Their sum is 0.80, which includes two copies of the 0.20 both-alarm record. Subtract one copy: $0.80-0.20=0.60$.

4. Exactly one means the record is $p$ or $f$. Its weight is $0.25+0.15=0.40$. Starting from the union, remove its single remaining copy of $b$: $0.60-0.20=0.40$. The definitions of $A$ and $B$ stay fixed throughout.

## Lesson 4 · Conditional probability and the reference group

**Guiding question.** What share satisfies a target among the records compatible with new information?

**Definition of conditional probability.** Suppose you know event $H$ occurred and want the chance of event $E$. Restrict attention to $H$. The original probability weight of this reference group is $P(H)$. Its part satisfying $E$ weighs $P(E\cap H)$. The fraction of the surviving weight that qualifies is
$$
P(E\mid H)=\frac{P(E\cap H)}{P(H)},\qquad P(H)>0.
$$
Read the vertical bar as “among records satisfying.” Division rescales all of $H$ to 100%. The numerator can never exceed the denominator because the intersection lies inside $H$.

### A die known to exceed three

**Problem.** For one fair roll let $E=\{2,4,6\}$ and $H=\{4,5,6\}$. Given $H$, calculate $P(E\mid H)$.

**Worked solution**

1. The right side of the bar names the new reference group $H$. Three of the original six faces remain, giving $P(H)=3/6$.

2. Keep the faces that also satisfy $E$: $E\cap H=\{4,6\}$. In the original six-face space their weight is $2/6$.

3. Divide original qualifying weight by original reference weight: $(2/6)/(3/6)=(2/6)(6/3)=2/3$. Equivalently two of the three remaining equally likely faces are even.

### Which alarm is given?

**Problem.** One record shows neither alarm (0.40), Pressure only (0.25), Flow only (0.15), or both (0.20). Calculate Flow given Pressure and Pressure given Flow. Diagnose the proposed ratio $0.45/0.35$.

**Worked solution**

1. Let $A$ mean Pressure active and $B$ mean Flow active. The intersection contains only the both record, weighing 0.20.

2. Given Pressure, the denominator includes Pressure only and both. Thus $P(B\mid A)=P(A\cap B)/P(A)=0.20/(0.25+0.20)=20/45=4/9$.

3. Given Flow, $P(A\mid B)=0.20/(0.15+0.20)=20/35=4/7$. Both answers count the same qualifying records relative to different groups.

4. The numerator 0.45 includes Pressure-only records, which cannot qualify once Flow is given. It exceeds the proposed denominator. A subset's share of its own reference group cannot exceed one.

### Reversing a condition with unequal weights

**Problem.** Outcomes $a,b,c,d$ weigh 0.10, 0.20, 0.30 and 0.40. For $R=\{a,b,c\}$ and $S=\{c,d\}$, calculate both directions.

**Worked solution**

1. List the common outcomes first. Only $c$ appears in both lists, so $P(R\cap S)=0.30$ in either direction.

2. For $P(R\mid S)$, the reference weight is $0.30+0.40=0.70$. The share of $S$ belonging to $R$ is $0.30/0.70=3/7$.

3. For $P(S\mid R)$, the reference weight is $0.10+0.20+0.30=0.60$. The share of $R$ belonging to $S$ is $0.30/0.60=1/2$. Counting shared letters divided by the number of letters would discard the unequal weights.

## Plain-English glossary

| Term | Meaning in these lessons |
|---|---|
| Outcome and event | An outcome is one possible recorded result. An event groups all results satisfying a question. |
| Equal probability and disjointness | Equal probability compares weights. Disjointness says the sets have no outcome in common. |
| Complement, union, intersection | Outside the event within the sample space, in at least one event, and in both events, respectively. |
| Conditioning | Restrict the reference group to outcomes compatible with the given information and rescale its weight to one. |

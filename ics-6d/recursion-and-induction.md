# Recursion and Induction

## Sequences

* **sequence:** function in which the domain is a set of consecutive integers.
  * `g_k` is the **term**
  * `k` is the **index**
  * can be **infinite** or **finite** (in the positive direction)
  * can be specified by an explicit formula
* here is some more vocab:
  * increasing, non-decreasing
  * decreasing, non-increasing&#x20;

### Geometric Sequences

sequence where you get one term by _multiplying_ the previous term by a **common ratio**. can be infinite or finite.&#x20;

```
s_k = a * r^k, k>=0
s_0 = a * r^0

a_0 = a   (initial value)
a_n = r * a_n-1  for n ≥ 1   (recurrence relation)
```

### Arithmetic Sequence

sequence where you get a term by taking the previous term and _adding_ a **common difference**.

```
t_n = t_0 + dn, with n>=0

a_0 = a   (initial value)
a_n = d + a_n-1   for n ≥ 1   (recurrence relation)
```

## Recurrence relations

it is a rule that defines a term `a_n` as a function of the previous term in a sequence. useful to model the running time of recursive algorithms

#### Fibonacci Sequence

```
f_0 = 0
f_1 = 1
f_n = f_n-1 + f_n-2
```

* more vocab:
  * **dynamic system:** system that changes over time
  * **discrete time dynamical system:** state of the system stays fixed within one time interval that you set
  * therefore the history of the system is defined by a sequence of states, indexed by the positive integers

### Summations

express the sum of terms in a numerical sequence.

![](<../.gitbook/assets/image (17).png>)

sometimes it is usful to pull out the final term in the summation, why idk, book said so

you can also change the variables in a summation, consider `i = k + 2`

``![](<../.gitbook/assets/image (13).png>)``

## Induction

is a proof technique. two components:&#x20;

* **base case:** establishes that the theorem is true for the first value in the sequence.
  * usually n = 0 or n = 1
* **inductive hypothesis:** assume the theorem is true for `k`
* **inductive step:** if the theorem is true for `k`, then show that the theorem also holds for `k + 1`

## Strong Induction

Similar to regular induction

* base case: S(0) and S(1) are true
  * finding the base cases is actually the hardeset part!
    * trial and error dw
* inductive hypothesis: S(0) ∧ S(1) ∧ ... ∧ S(k)
  * from first base case to k
* inductive step: For every k ≥ 1, (S(0) ∧ S(1) ∧ ...... ∧ S(k)) implies S(k + 1)
  * basically you assume that the statement is true for all values `>= k`, which will then be used to show that it holds for `k+1`

## Well-Ordering Principle

math induction is related to this.&#x20;

The well-ordering principle ays that any non-empty subset of the non-negative integers has a smallest element




# Binomial Option Pricing Model (Python)

This project implements the **Binomial Option Pricing Model** using the  
**Cox Ross Rubinstein (CRR)** framework in Python.

Two implementations are provided:
- A **naive loop based** version for clarity
- A **NumPy vectorized** version for performance

---

## ✨ Features

- Cox Ross Rubinstein (CRR) binomial tree
- Arbitrage free, risk neutral pricing
- Backward induction on a recombining tree
- European **Call** option pricing
- American **Put** option pricing with early exercise
- Performance benchmarking (loops vs NumPy vectorization)

---

## 📌 European Call Option

### Overview

A **European call option** can only be exercised at maturity.

The stock price follows a recombining binomial tree:

S<sub>i,j</sub> = S<sub>0</sub> · u<sup>j</sup> · d<sup>i−j</sup>

At maturity:

C<sub>N,j</sub> = max(S<sub>N,j</sub> − K, 0)

Option prices are computed backward using **risk neutral valuation**:

C<sub>i,j</sub> = e<sup>−rΔt</sup> . (q · C<sub>i+1,j+1</sub> + (1−q) · C<sub>i+1,j</sub>)

where the risk-neutral probability is:

q = (e<sup>rΔt</sup> − d) / (u − d)

---

### Implementations

#### 1️⃣ `european_tree_slow`
- Uses nested Python `for` loops
- Direct and intuitive
- Computationally expensive for large `N`

#### 2️⃣ `european_tree_fast`
- Uses NumPy vectorized array operations
- Same time complexity O(N²)
- Much faster due to execution in optimized C code

Both implementations return the option price at time `t = 0`.

---

## 📌 American Put Option

### Overview

An **American put option** can be exercised **at any time** up to maturity.

At maturity:

P<sub>N,j</sub> = max(K − S<sub>N,j</sub>, 0)

At each earlier node, the option value is the maximum of:
- **Continuation value** (holding the option)
- **Immediate exercise value** (early exercise)

P<sub>i,j</sub> = max(  (K − S<sub>i,j</sub>),  (e<sup>−rΔt</sup> · (q · P<sub>i+1,j+1</sub> +(1−q) · P<sub>i+1,j</sub>))  )

This early-exercise condition is what differentiates American options from European options.

---

### Implementations

#### 1️⃣ `american_slow_tree`
- Naive nested loop implementation
- Explicit early exercise check at each node
- Clear and educational
- Suitable for small `N`

#### 2️⃣ `american_fast_tree`
- NumPy vectorized backward induction
- Uses rolling arrays for memory efficiency
- Applies early exercise condition via `np.maximum`
- Much faster for large trees

Both implementations return the American put price at time `t = 0`.

---

## ⏱ Performance Benchmarking

A custom `@timing` decorator is used to measure execution time:

```python
@timing
def function(...):
    ...

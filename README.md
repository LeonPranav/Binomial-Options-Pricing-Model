# Binomial Option Pricing Model (Python)

I implemented the **Binomial Option Pricing Model** in Python, comparing a **naive (loop based)** implementation with a **vectorized NumPy implementation** to highlight performance differences.

The project demonstrates:
- Implements the **Cox–Ross–Rubinstein (CRR) binomial option pricing model**
- Arbitrage free / No arbitrage option pricing
- Backward induction on a binomial tree
- The impact of NumPy vectorization vs Python loops
- Practical performance benchmarking

---

## 📌 Overview

We price a **European Call Option** using a recombining binomial tree.

The stock price evolves as:
S<sub>i,j</sub> = S<sub>0</sub> · u<sup>j</sup> · d<sup>i−j</sup>

At maturity:
C<sub>N,j</sub> = max(S<sub>N,j</sub> − K, 0)

Option prices are computed backward in time using **risk neutral valuation**.

---

## 🧠 Implementations

### 1️⃣ `binomial_tree_slow`
- Uses **nested Python for loops**
- Direct and intuitive
- Computationally expensive for large `N`

### 2️⃣ `binomial_tree_fast`
- Uses **NumPy vectorized array operations**
- Same algorithmic complexity \(O(N^2)\)
- Much faster due to execution in optimized C code

Both implementations return the **option price at time 0**.

---

## ⏱ Timing Decorator

A custom `@timing` decorator is used to measure execution time:

```python
@timing
def function(...):
    ...

# crypto-assignment2
this repository is having a zip file containing all 3 python3 codes of trail division, Fermat and miller-rabin with the output screenshots😃.
# Primality Testing — Trial Division, Fermat & Miller-Rabin

**Name:** Abhiram Mamidi
**Roll No:** AM.SC.U4CYS25046
**Course:** B.Tech Cybersecurity, Amrita Vishwa Vidyapeetham
**Language:** Python 3

This repository implements three primality tests in Python 3:

1. **Trial Division**: a deterministic test that gives a definite answer.
2. **Fermat Primality Test**: a probabilistic test.
3. **Miller-Rabin Primality Test**: a probabilistic test that is stronger than Fermat.

The testing logic is written from scratch. No library primality functions are used. Only `pow(a, e, n)` (modular exponentiation) and `math.gcd` are used.

---

## Repository Structure

```
├── trial_division.py      # Trial division
├── fermat.py              # Fermat primality test
├── miller_rabin.py        # Miller-Rabin primality test
├── screenshots/           # Output screenshots of each program
└── README.md
```

A compressed `.zip` of all source files and screenshots is also included.

---

## How to Run

Requires **Python 3.8+** (`math.isqrt` is used). No external packages are needed.

```bash
python3 trial_division.py
python3 fermat.py
python3 miller_rabin.py
```

Each program automatically:
- prints a **worked example** with every intermediate step,
- runs the **required test inputs** `1, 2, 3, 4, 9, 17, 21, 29`,
- then asks for an optional integer to test. Type a number, or press **Enter** to exit.

---

## 1. Trial Division

The program checks every divisor `d` from 2 up to `⌊√n⌋`. If any `d` divides `n`, then `n` is composite. If none does, `n` is prime.

**Why stop at √n?** If `n = a × b` and both `a` and `b` were greater than `√n`, then `a × b > n`, which is impossible. So every composite number has at least one divisor `≤ √n`.

**Worked example: n = 29**
```
√29 ≈ 5.385 → test divisors 2 to 5
29 mod 2 = 1
29 mod 3 = 2
29 mod 4 = 1
29 mod 5 = 4
No divisor found → 29 is PRIME
```

---

## 2. Fermat Primality Test

Fermat's Little Theorem says that if `n` is prime and `gcd(a, n) = 1`, then `aⁿ⁻¹ ≡ 1 (mod n)`.

- If `aⁿ⁻¹ mod n ≠ 1`, then `n` is **definitely composite**.
- If `aⁿ⁻¹ mod n = 1`, then `n` is **probably prime for base a**.

**Worked example: n = 13, a = 2**
```
2^1 mod 13 = 2
2^2 mod 13 = 4
2^4 mod 13 = 3
2^8 mod 13 = 9
2^12 = 2^8 × 2^4 = 9 × 3 = 27 ≡ 1 (mod 13)
→ Probably prime (base 2)
```

**Limitation:** Some composite numbers still pass this test. For example, `341 = 11 × 31` gives `2³⁴⁰ ≡ 1 (mod 341)`. Composites like this are called *Fermat pseudoprimes*. *Carmichael numbers*, such as 561, pass for every base that is coprime to them.

---

## 3. Miller-Rabin Primality Test

Steps for an odd `n > 3`:

1. Write `n − 1 = 2ˢ × d`, where `d` is odd.
2. Compute `x₀ = aᵈ mod n`. If `x₀ = 1` or `x₀ = n − 1`, the number **passes**.
3. Otherwise square `x` repeatedly, up to `s − 1` times. If some square equals `n − 1`, the number **passes**.
4. If `n − 1` never appears, `n` is **definitely composite**.

**Worked example: n = 17, a = 3**
```
n − 1 = 16 = 2^4 × 1  →  s = 4, d = 1
x0 = 3^1 mod 17 = 3
x1 = 3^2 mod 17 = 9
x2 = 9^2 mod 17 = 13
x3 = 13^2 mod 17 = 16 = n − 1
→ Probably prime (base 3)
```

Miller-Rabin also catches `341`, the composite that fools Fermat:
```
340 = 2^2 × 85 → x0 = 2^85 mod 341 = 32, x1 = 32^2 mod 341 = 1
340 never appeared → COMPOSITE
```

---

## Results (base a = 2 for Fermat and Miller-Rabin)

| Input | Trial Division | Fermat | Miller-Rabin |
|:-----:|:--------------:|:------:|:------------:|
| 1  | Neither   | Neither | Neither |
| 2  | Prime     | Prime | Prime |
| 3  | Prime     | Prime | Prime |
| 4  | Composite | Composite | Composite |
| 9  | Composite | Composite | Composite |
| 17 | Prime     | Probably prime (base 2) | Probably prime (base 2) |
| 21 | Composite | Composite | Composite |
| 29 | Prime     | Probably prime (base 2) | Probably prime (base 2) |

These inputs are handled before any base is used:
- `n < 2` is neither prime nor composite.
- `2` and `3` are prime. No base fits the range `2 ≤ a ≤ n − 2`, so they are handled directly.
- Even numbers greater than 2 are composite.

---

## Output Screenshots

### Trial Division
![Trial Division Output](screenshots/trial_division_output.png)

### Fermat Test
![Fermat Output](screenshots/fermat_output.png)

### Miller-Rabin Test
![Miller-Rabin Output](screenshots/miller_rabin_output.png)

---

## Comparison

| | Trial Division | Fermat | Miller-Rabin |
|---|---|---|---|
| Result type | Definite | Probable prime | Probable prime |
| Can a composite pass? | No | Yes (pseudoprimes, Carmichael numbers) | Rarely (≤ 1/4 chance per base) |
| Time complexity | O(√n) | O(log n) multiplications | O(s · log n) multiplications |
| Suitable for large n | No | Yes, but unreliable | Yes, used in practice (e.g. RSA key generation) |

Trial division always gives a definite answer, but its running time grows with √n, so it becomes impractical for large numbers. Fermat and Miller-Rabin run in time proportional to the number of digits. Their prime results are only "probably prime" for the tested base.

---

## References

- Cormen, Leiserson, Rivest & Stein, *Introduction to Algorithms*, Chapter 31 (Number-Theoretic Algorithms).
- William Stallings, *Cryptography and Network Security*, chapter on primality testing.
- Python documentation for the built-in `pow()` and `math.gcd` / `math.isqrt`.

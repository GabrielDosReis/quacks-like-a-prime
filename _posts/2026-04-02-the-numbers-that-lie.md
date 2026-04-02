---
title: "The Numbers That Lie"
description: >-
  What pseudoprimes are, why they exist, and why your credit card
  depends on catching them.
date: 2026-04-07 09:00:00 -0700
categories: [Mathematics, Cryptography]
tags: [pseudoprimes, primality-testing, fermat, carmichael]
math: true
toc: true
pin: false
image:
  path: /assets/img/post1-numberline.svg
  alt: "Primes, composites, and pseudoprimes on the number line"
---

# The Numbers That Lie

**Subtitle:** What pseudoprimes are, why they exist, and why your credit card depends on catching them  

---

### The Number That Passed the Test

Here's a number: **341**.

Is it prime? If you try dividing by small primes, you'll eventually find that 341 = 11 × 31. Not prime. But division is tedious, and for very large numbers — the kind your browser uses for encryption — it's computationally hopeless. So mathematicians use a cleverer approach: a *test*.

There is a famous test, due to Pierre de Fermat, that has reliably identified primes since the 1640s. Apply it to 341, and 341 passes. The test says: probably prime.

The test is not wrong, exactly — it answered the question it was asked. But the answer is misleading. 341 is composite, and yet it passed. 341 is a **pseudoprime** — a composite number that passes a test designed to catch primes. It is, mathematically speaking, an impostor. It has the *appearance* of primality without the *substance* of it. One might say it interviews well.

This isn't a curiosity. Pseudoprimes are the reason your bank's website works, the reason encrypted messages stay secret, and the reason cryptographers lose sleep. Here's why.

### What the Test Is (and Why It Usually Works)

In 1640, Fermat wrote a letter to a friend containing a beautiful observation. If $p$ is a prime number, and you pick any number $a$ that isn't divisible by $p$, then:

> Raise $a$ to the power $p − 1$, divide by $p$, and the remainder is always $1$.

Let's try it. Take the prime $p = 7$ and $a = 2$:

- $2^6$ = 64
- 64 ÷ 7 = 9 remainder **1** ✓

It works for every prime you try. So you can use it as a test: given a big number $n$, pick a random $a$, compute $a^{(n−1)} \bmod n$, and check if you get $1$. If you don't get $1$, $n$ is definitely not prime. If you do get $1$, $n$ is *probably* prime.

This is **Fermat's primality test**, and it works shockingly well in practice. Most composite numbers fail it immediately.

But 341 doesn't fail. Pick $a = 2$:

- $2^340 \bmod 341$ = **1** ✓

The test says "probably prime."  But 341 is composite. The test passed a number it shouldn't have. Mathematicians say 341 is a *pseudoprime to base 2*.

![Primes, composites, and pseudoprimes on the number line](/assets/img/post1-numberline-vertical.svg)
_Green: genuine primes. Red: composites caught immediately. Yellow: impostors that slip through._


### The Perfect Liars: Carmichael Numbers

341 only fools the test for *some* choices of *a*. Pick *a* = 3 and it fails: $3^{340} \bmod 341$ ≠ 1. So you could catch 341 by testing a few different bases.

But in 1910, Robert Carmichael discovered something worse: the number **561** passes the Fermat test for *every* possible base. Not just base 2 or base 3 — every single one. No amount of repeating the Fermat test will ever catch 561. It is a **perfect liar**.

561 = 3 × 11 × 17. It's obviously composite if you factor it. But if you can't factor it — and for large numbers, factoring is essentially impossible — then 561 looks indistinguishable from a prime by Fermat's test.

These perfect liars are called **Carmichael numbers**. Their existence is not accidental — it follows from algebra. The Fermat test checks exactly one thing: does a certain quantity related to *n*'s internal arithmetic divide *n* − 1? For primes, the answer is always yes. For most composites, it's no. But Carmichael numbers are composites where the answer happens to be yes anyway, by structural coincidence. They pass the interview without being qualified for the job.

The first seven were actually found by a Czech mathematician named Václav Šimerka in **1885** — a quarter-century before Carmichael. His paper, published in Czech, went unnoticed. Mathematics is not always fair to those who arrive first but publish in the wrong language.

Fun fact: the third Carmichael number, **1729**, is also the Hardy–Ramanujan number — the smallest number expressible as the sum of two cubes in two different ways. Some numbers accumulate distinctions the way some professors accumulate committee memberships.

### There Are Infinitely Many Liars (but They're Very Rare)

In 1994, Red Alford, Andrew Granville, and Carl Pomerance proved that Carmichael numbers never run out — there are infinitely many perfect liars. This was a landmark result.

But they're extraordinarily rare. Among all numbers up to $10^{21}$, there are only about 20 million Carmichael numbers — roughly one in 50 trillion. They are rare enough that you'll almost never encounter one by accident, and common enough that you can't pretend they don't exist. In 2021, Daniel Larsen — then a teenager — proved they're also ubiquitous: wherever you look on the number line, a Carmichael number is nearby.

### Why This Matters for Your Credit Card

Every time you visit a website with HTTPS, your browser generates a large prime number (typically 1024 or 2048 bits) as part of the encryption handshake. It does this by picking a random large number and testing whether it's prime.

The test it uses is a descendant of Fermat's — something called Miller-Rabin, which we'll explore in a future post. The critical question is: **could it accidentally pick a pseudoprime instead of a prime?**

If it does, the encryption can be broken. A composite number masquerading as a prime in an RSA key means someone could factor it and read your traffic. Pseudoprimes they're a *threat model* in our daily lives, not just mathemayical curiosities.

In 2018, researchers demonstrated that they could construct composite numbers that fooled the primality tests in OpenSSL, GNU GMP, and several other widely-used cryptographic libraries. OpenSSL claimed an error rate of one in 2^80 (roughly one in a billion billion billion). The actual rate under adversarial conditions was **one in sixteen**.

Pseudoprimes need constant watch as security vulnerabilities. They were discovered in the 1800s and *still* frustrate library implementations in 2018.

### What Comes Next

So if the Fermat test can be fooled — even by perfect liars that no amount of testing will catch — how do cryptographers actually verify primes?

The answer involves combining *fundamentally different* kinds of tests, in a way that no known number can defeat. It's called the Baillie-PSW test, no composite has beaten it in over 45 years, and there's a cash prize for anyone who finds one.

That's next post.


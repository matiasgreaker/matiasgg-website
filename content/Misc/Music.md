---
title: "MUSIC"
draft: false
mathjax: true
weight: 4
---

### MUSIC - (Multiple Signal Classification)

Before going into the details about MUSIC I want to revisit some important topics. As I believe these topics lies in the hearth of MUSIC. 
And a note about the math functions: when defining DSP functions, there are often multiple ways of defining them, e.g. for the DFT, some divide by the number of samples, some by the square root of the number of sample, some define it as an infinite sum, etc. These things are of course just details, and the main idea is the same for all of them. But, these details can be annoying if you want to program and test the theory against real examples. As many of these details makes you end up with a different scaling of your data. And I at least then always end up wondering: did I do it correctly or is it **just** a scaling that I'm missing? I will try to specify all of these details. With the idea that you can program everything in python and get **exactly** the same result. 


#### DFT - Descrete Fourier transform 
With that said, here is the definition of the DFT (exactly as the python library [Numpy](https://numpy.org/doc/stable/reference/routines.fft.html) defines it):

$$X[k] = \mathcal{F}(x[n]) = \sum_{n=0}^{N-1} x[n]e^{-j2\pi nk/N} $$

As you see here, I'm using capital letters to illustrate the DFT of some sequence. That is $Y[k] = \mathcal{F}(y[n])$. I will use this throughout the text.
If you want to read more about the DFT, check out mye page here (link).

#### Autocorrelation
Autocorrelation of some sequence $x[m]$, with length $N$, is defined as the sequence:

$$r[m] = \sum_{n=0}^{N-1} x^*[n]x[n+m]$$

here $x^* [n]$ is the [complex-conjugate](https://mathworld.wolfram.com/ComplexConjugate.html), and $m$ is in the range $0$ to $N-1$.
One more thing we should specify here is what happens with the last part $x[n+m]$ when $n+m>N$. How do we define $x[n]$ out of its bound? It is common to assume that $x[n]$ is periodic with period $N$. That is $x[n+N] = x[n]$. As we will see, we will need this later on.

#### Wiener-Khinchin Theorem 
This theorem states that the DFT of the autocorrelation sequence $r[m]$ is equal to the power spectrum of $x[n]$. Written out this becomes:

$$ R[k] = \mathcal{F}(r[m]) = |X[k]|^2.$$
To me at least this is not a trivial result. So lets prove it.

**Wiener-Khinchin Proof:**

We start by taking the DFT of the covariance function $r[m]$.

$$ R[k] = \mathcal{F}(r[m]) = \sum_{m=0}^{N-1} r[m] e^{- j 2 \pi m k/N} = \sum_{m=0}^{N-1} \left(\sum_{n=0}^{N-1}x^*[n]x[n+m]\right) e^{- j 2 \pi m k/N} $$

All we have done above is using the definitions for the DFT and $r[m]$. Now in the last summation, since $x^*[n]$ is independent of the index $m$ we can write the last summation as:

$$ \sum_{m=0}^{N-1} \left(\sum_{n=0}^{N-1}x^*[n]x[n+m]\right) e^{- j 2 \pi m k/N} $$

$$ = \sum_{n=0}^{N-1} x^*[n] \left( \sum_{m=0}^{N-1}x[n+m] e^{- j 2 \pi m k/N}\right) $$

This trick is not so easy to see by just looking at the equation. But if you try to write out the sum you can more easily see it.

Now since the inner term $x[n+m]$ always starts iterating at index $n$, which is fixed for the whole inner summation, we can write the inner sum like this:
$$ = \sum_{n=0}^{N-1} x^*[n] \left( \sum_{m=n}^{N-1+n}x[m] e^{- j 2 \pi (m-n) k/N}\right) $$

$$ = \sum_{n=0}^{N-1} x^*[n] \left( \sum_{m=n}^{N-1+n}x[m] e^{- j 2 \pi m k/N}e^{ j 2 \pi n k/N}\right) $$

$$= \sum_{n=0}^{N-1} x^*[n]e^{ j 2 \pi n k/N} \left( \sum_{m=n}^{N-1+n}x[m] e^{ -j 2 \pi m k/N}\right) $$

Now for the trick we promised to use (see Autocorrelation)! Since we assumed $x[n+N] = x[n]$ we can write the inner sum as:

$$ = \sum_{n=0}^{N-1} x^*[n]e^{ j 2 \pi n k/N} \left( \sum_{m=0}^{N-1}x[m] e^{ -j 2 \pi m k/N}\right) $$

The inner sum is now independent of the outer summation and can be "pulled out", leaving us with:

$$ = \left( \sum_{n=0}^{N-1} x^*[n]e^{ j 2 \pi n k/N} \right)\left( \sum_{m=0}^{N-1}x[m] e^{ -j 2 \pi m k/N}\right)$$

$$ = \left( \sum_{n=0}^{N-1} x[n]e^{ -j 2 \pi n k/N} \right)^*\left( \sum_{m=0}^{N-1}x[m] e^{ -j 2 \pi m k/N}\right) = X^*[k]X[k] = |X[k]|^2 $$ 

QED.

### Reference
https://mathworld.wolfram.com/Autocorrelation.html
https://mathworld.wolfram.com/Wiener-KhinchinTheorem.html
https://ccrma.stanford.edu/~jos/mdft/Convolution_Theorem.html
https://www.marga.com.ar/6615/wiener-khinchin.pdf

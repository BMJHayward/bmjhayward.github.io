---
title: I have gauged the size of the matter
description: 'I did this in a jupyter notebook:'
date: '2016-03-26T02:25:33.295Z'
categories: []
keywords: []
slug: /@atomcogs/i-have-gauged-the-size-of-the-matter-a8be2bca9f62
---

I did this in a jupyter notebook:

\# %matplotlib inline  
import numpy as np  
import matplotlib.pyplot as plt

trace\_norms = \[np.sqrt(np.eye(i).trace()) for i in range(100)\]

plt.plot(trace\_norms)

![I get this logarithmic curve for all matrices so far](img/1__2yafAhVPZUni5IzONUT6XQ.png)
I get this logarithmic curve for all matrices so far

I got a similar curve for the below matrices:

*   hadamard
*   hankel
*   pascal
*   inverse pascal
*   hilbert
*   inverse hilbert
*   fourier
*   circulant

I may put together something more rigorous in future, as I am still curious as to why this happens.
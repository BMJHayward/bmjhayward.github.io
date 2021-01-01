---
title: Weird matrix with a row of primes in it
description: >-
  I do this thing sometimes where I’ll just make stuff up, stick it in scipy and
  see what happens. I rarely share it publicly, because my ego…
date: '2016-10-07T06:06:06.352Z'
categories: []
keywords: []
slug: /@atomcogs/weird-matrix-with-a-row-of-primes-in-it-59a46496ba3e
---

I do this thing sometimes where I’ll just make stuff up, stick it in scipy and see what happens. I rarely share it publicly, because my ego is intensely fragile and allergic to public exposure. Onward…

import numpy as np

def pmatrix\_rptcol():  
    '''  
    create matrix where first row is primes,  
    first column is repeated 2  
    '''  
    pvec0 = \[i for i in range(30) if isprime(i)\]  
    def make\_prime\_row(pvecn):  
        ret\_pvec = list(range(len(pvecn)))  
        for i in range(len(pvecn)):  
            if i == 0:  
                ret\_pvec\[i\] = pvecn\[i\]  
            else:  
                ret\_pvec\[i\] = pvecn\[i\] - pvecn\[i-1\]  
        return ret\_pvec  
    pvec1 = make\_prime\_row(pvec0)  
    pvec2 = make\_prime\_row(pvec1)  
    pvec3 = make\_prime\_row(pvec2)  
    pvec4 = make\_prime\_row(pvec3)  
    pvec5 = make\_prime\_row(pvec4)  
    pvec6 = make\_prime\_row(pvec5)  
    pvec7 = make\_prime\_row(pvec6)  
    pvec8 = make\_prime\_row(pvec7)  
    pvec9 = make\_prime\_row(pvec8)  
    return np.array(\[pvec0, pvec1, pvec2, pvec3, pvec4, pvec5, pvec6, pvec7, pvec8, pvec9\])

All this means is, there is a matrix, where a number is equal to:

the number above minus the number to the left of the number above it

except for column 1. they are all 2. i don't know why. why not?

It’s a 10x10 invertible matrix of integers, with independent columns where P != P.T (i.e. not symmetric, or not equal to its transpose, if it’s been a while)

I found this handy function to tell if a number is prime or not. I literally could not be bothered writing one myself at this time of night, so stack overflow helped me:

def isprime(n):  
    '''  
    check if integer n is a prime  
    shamelessly ripped from:  
    [http://stackoverflow.com/questions/18833759/python-prime-number-checker#18833845](http://stackoverflow.com/questions/18833759/python-prime-number-checker#18833845)  
    '''  
    # make sure n is a positive integer  
    n = abs(int(n))  
    # 0 and 1 are not primes  
    if n < 2:  
        return False  
    # 2 is the only even prime number  
    if n == 2:   
        return True      
    # all other even numbers are not primes  
    if not n & 1:   
        return False  
    # range starts with 3 and only needs to go up   
    # the square root of n for all odd numbers  
    for x in range(3, int(n\*\*0.5) + 1, 2):  
        if n % x == 0:  
            return False  
    return True

You just do this:

In \[86\]: isprime(12430987123409872134098721340987530987321409872130948721094872134098721340987213409872134098721398765984310968431609814369867)  
Out\[86\]: False

It even works with big numbers!

There is also this matrix, because I thought it a touch silly to have a column of _just 2’s everywhere…_

def pmatrix\_norptcol():  
    '''  
    create matrix of first row prime,  
    first column not repeated  
    '''  
    pvec0 = \[i for i in range(30) if isprime(i)\]  
    pvec1 = \[i-1 for i in pvec0\]  
    pvec2 = \[i-1 for i in pvec1\]  
    pvec3 = \[i-1 for i in pvec2\]  
    pvec4 = \[i-1 for i in pvec3\]  
    pvec5 = \[i-1 for i in pvec4\]  
    pvec6 = \[i-1 for i in pvec5\]  
    pvec7 = \[i-1 for i in pvec6\]  
    pvec8 = \[i-1 for i in pvec7\]  
    pvec9 = \[i-1 for i in pvec8\]  
    return np.array(\[pvec0, pvec1, pvec2, pvec3, pvec4, pvec5, pvec6, pvec7, pvec8, pvec9\])

And then we try all the usual decompositions, basis, eigenvalues, etc etc…

I’ll post back with more once I’ve got my data in a presentable state.
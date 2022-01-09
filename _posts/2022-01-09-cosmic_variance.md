---
title: Cosmic Variance
date: 2022-01-09
permalink: /posts/2022/01/cosmic-variance
excerpt_separator: <!--more-->
toc: true
tags:
  - research
  - notes
  - cosmic variance
---

1. Worked on this website for a bit.
2. Found a mistake in the equation I have been using to calculate the cosmic variance.
    
    $$
    \begin{equation}
    \Delta P_{21}(k) = P_{21}(k)\sqrt{\dfrac{1}{N_c}}
    \end{equation}
    $$
    
    where
    
    $$
    \begin{equation}
    N_c = 2\pi k^2 \Delta k \dfrac{V}{(2\pi)^3}
    \end{equation}
    $$
    
    The mistake was in the $\Delta k$. I used to think that that $\Delta k = \frac{2\pi}{L}$which is just the fundamental frequency of the simulation box. But it should be the bin-width in $k$-space.

<!--more-->
    
    Now, I should have realised this a long time ago since cosmic variance error is similar to a Poisson counting error and $N_c$ is just the number of modes in the $k$-bin.
    
    $$
    \begin{equation}
    \begin{split}
    N_c  & = \dfrac{4\pi k^2 \Delta k}{(dk)^3}\\ 
    &= \dfrac{4\pi k^2 \Delta k}{[2\pi/L]^3}\\ &= 4\pi k^2 \Delta k \dfrac{V}{(2\pi)^3}
    \end{split}
    \end{equation}
    $$
    
    The numerator $4\pi k^2 \Delta k$ is just the volume of the spherical shell in the $k$-space. 
    
    And $dk = \frac{2\pi}{L}$ and each $k$-mode occupies $(dk)^3$ volume in $k$-space.
    
    Dividing these two will obviously give the number of modes in the $k$-bin. (This is very similar to the calculations in solid state physics while finding the Fermi-energy and such.)
    
    Now you will notice that my expression for $N_c$ in eq 3 is off by a factor of 2 with respect to eq 2. This is because of the property of Fourier transform of a real field. The information encoded in the $-k$ and $k$ modes are the same and the number of *useful* modes is only half of what is calculated in eq 3. (This sounds a bit hand-wavy and is because I do not fully understand this myself. Need to look up the FT’s notes.)

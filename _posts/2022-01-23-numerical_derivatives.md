---
title: Numerical Derivatives
date: 2022-01-23
permalink: /posts/2022/01/numerical_derivatives
excerpt_separator: <!--more-->
toc: true
tags:
  - research notes
  - Fisher Matrix
  - numerical derivatives
---

1. Last week was pretty slow and didn’t get much done. Found a numerical bug in the code though!
2. This week was one of the better ones. I have been trying to code up the Fisher matrices and had to look up numerical derivatives.
    
   A good guide to Fisher matrices(and that too with astro examples) is [Dan Coe’s article]([https://ui.adsabs.harvard.edu/abs/2009arXiv0906.4123C/abstract](https://ui.adsabs.harvard.edu/abs/2009arXiv0906.4123C/abstract)). The topic for today is not Fisher matrices so I will not write more about that here, though I will do it in future.

<!--more-->
    
   Below is a screenshot from the paper. 
    
   ![](/images/posts/2022-01-23-1.png){:width="80%" .align-center}
    
   $\chi^2$ is the usual statistical workhorse — chi-squared. Today I just want to write down how the equation 11 comes about.
    
   Instead of $\chi^2(x, y)$ I will use $f(x)$ mostly because that is easier to type up!
    
   Almost all of these numerical derivatives’ formulas can be derived from the Taylor series — this is one of those few series’ which actually make an intuitive sense to me and is easier to come up with on the spot even if you don’t memorise it.
    
   $$
    \begin{equation}
    \begin{split}
    f(x+h) & = \sum_{n=0}^{+\infty} \frac{h^n}{n!} f^n(x) \\
    & = f(x) + h f^1(x) + \frac{h^2}{2!} f^2(x) + \frac{h^3}{3!} f^3(x) + ... 
    \end{split}
    \end{equation}
   $$
    
   where $f^n(x)$ is the $n$-th derivative with respect to $x$, i.e. $(\dfrac{d^n f(x)}{dx^n})$.
    
   If you ignore the terms with $n>1$ you get
    
   $$
    \begin{equation}
    f(x+h) = f(x) + h f^1(x)
    \end{equation}
   $$
    
   Now this is what I said about Taylor series being very intuitive:
    
   Say $f(x)\equiv s(t)$ is the displacement $s$ of a body as a function of time $t$.  If the displacement at some time $t_0$ is $s_0$, then the displacement after some time $dt$ is $s(t+dt) = s_0 + vdt$ where $v$ is simply the velocity of the body and this exactly what one derives in the introductory physics courses. *Displacement then* is simply the displacement between *now and then* added on top of *displacement now.*
    
   Now if we stretch this a bit further and takes the next term in equation 1, we get $s(t+dt) = s_0 + vdt + \frac{1}{2} a(dt)^2$— another well-known equation!
    
   Anyway, back to calculating derivatives.
    
   Rearranging equation 2 gives:
    
   $$
    \begin{equation}
    f^1(x) = \dfrac{f(x+h) - f(x)}{h} 
    \end{equation}
   $$
    
   Wikipedia terms this the [Difference quotient]([https://en.wikipedia.org/wiki/Difference_quotient](https://en.wikipedia.org/wiki/Difference_quotient)) and this is the definition of the derivative as well, in the case when the RHS goes to 0.
    
   We need two data points here, $f(x+h)$ and $f(x)$, and we are throwing away all the terms higher than $n>1$. But we can do better and keep more terms terms from equation 1 but still need only two data points.
    
   Consider:
    
   $$
    \begin{equation}
    f(x+h) = f(x) + h f^1(x) + \frac{h^2}{2!} f^2(x) + \frac{h^3}{3!} f^3(x) + ...\end{equation}
   $$
    
   $$
    \begin{equation}
    f(x-h) = f(x) - h f^1(x) + \frac{h^2}{2!} f^2(x) - \frac{h^3}{3!} f^3(x) + ...\end{equation}
   $$
    
   Now, if we subtract equation 5 from equation 4, we get
    
   $$
    \begin{equation}
    f(x+h) - f(x-h) = 2h f^1(x) + 2\frac{h^3}{3!} f^3(x) + ...\end{equation}
   $$
    
   Ignoring the higher-order terms,
    
   $$
    \begin{equation}
    f^1(x) = \dfrac{f(x+h) - f(x-h)}{2h} 
    \end{equation}
   $$
    
   Equation 2 is known as the [Symmetric derivative]([https://en.wikipedia.org/wiki/Symmetric_derivative](https://en.wikipedia.org/wiki/Symmetric_derivative)). We still need only two data points: $f(x+h)$ and $f(x-h)$. Plus we have only neglected the terms with $n>2$. Interestingly the value of the function where we’re trying to calculate the derivative, $x$, cancels out!
    
   But I promised to explain how the second order derivative in equation 11 comes about.
    
   Back to our Taylor series equation 1:
    
   $$
    \begin{equation}
    f(x+h) = f(x) + h f^1(x) + \frac{h^2}{2!} f^2(x)
    \end{equation}
   $$
    
   Solving for $f^2(x)$,
    
   $$
    \begin{equation}
    \begin{split}
    f^2(x) & = \frac{2}{h^2}\bigg[f(x+h) - f(x) - hf^1(x)\bigg]\\
    & = \frac{2}{h^2}\bigg[f(x+h) - f(x) - \dfrac{f(x+h) - f(x-h)}{2h}\bigg]\\
    & = \dfrac{\big[f(x+h) - 2f(x) + f(x-h)\big]}{h^2}\\
    \end{split}
    \end{equation}
   $$
    
   In the second step I substituted for the last term $f^1(x)$ from equation 7. So to compute the $2^{\rm nd}$ derivative we need three data points which makes sense when you realise that the $2^{\rm nd}$ derivative of a function gives the *curvature* nature of the function. Three points give a better sense of this than simpy two points.
    
   Something else I have noticed is that instead of using equation 7, if I had used the $f^1(x)$ from equation 5, $f^2(x) = \dfrac{2 \big[f(x+h) -  f(x-h)\big]}{h^2}$. Now this seems better in that you only need two data points so I would have guessed this to be the preferred way of doing it, albeit of course it doesn’t agree with my point about *curvature*. But I have not seen any discussion about this anywhere in the literature. Most probably because the time saved in this simplification is not enough to warrant the sacrifice in numerical accuracy(?).

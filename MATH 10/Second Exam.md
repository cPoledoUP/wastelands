# Module 7 - Functions: Properties, Types, and Applications
## Introduction
- a **function** can be considered as a **machine** or **operator**
- takes an input $x$, do something on the input, and produce an output $f(x)$
## What is a function?
- a function $f$ from $X$ to $Y$ ($f: X \rightarrow Y$) is a relation from $X$ to $Y$ wherein every $x$ in Set $X$ is associated to **one element** $y$ in Set $Y$ by a rule $f$
	- Set $X$ is called the **domain of $f$**
	- Set $Y$ is called the **range of $f$**
## Types of Functions
1. **Constant Function**
	- $f(x) = c$
	- any input $x$ will yield the same output $c$
	- technically a linear function with zero slope
![](_attachments/Pasted%20image%2020240425152557.png)
1. **Linear Function**
	- $f(x) = ax + b$
	- $a$ represents the **slope** (sometimes represented with $m$)
	- $b$ represents the **y-intercept**
	- technically a polynomial function of degree 1
	- ![](_attachments/Pasted%20image%2020240425152902.png)
2. **Quadratic Function**
	- $f(x) = ax^2 + bx + c$
	- $a \neq 0$
	- technically a polynomial function of degree 2
	- ![](_attachments/Pasted%20image%2020240425153131.png)
	- ![](_attachments/Pasted%20image%2020240425153141.png)
3. **Polynomial Function**
	- $f(x) = a_nx^n + a_{n-1}x^{n-1} + ... + a_1x + a_0$
	- degree $n$
4. **Rational Function**
	- $f(x) = \frac{P(x)}{Q(x)}$
	- $P(x)$ and $Q(x)$ are polynomial functions
	- $Q(x) \neq 0$
	- **asymptotes** are present in the graph
5. **Radical Function**
	- involves radical expression
6. **Exponential Function**
	- $f(x) = b^x$
	- $b >0$ and $b \neq 1$
	- base $b$
	- **natural exponential function** 
		- $b = e = 2.7182...$
		- $f(x) = e^x$
	- **exponential growth**
		- $b > 1$
	- **exponential decay**
		- b < 1
	- ![](_attachments/Pasted%20image%2020240425154434.png)
7. **Logarithmic Function**
	- $f(x) = \log_bx$
	- $b > 0$ and $b \neq 1$
	- base $b$
	- **natural logarithmic function**
		- if $b = e = 2.7182...$
		- $f(x) = \log_e x = \ln x$
	- ![](_attachments/Pasted%20image%2020240425155133.png)
8. **Sine Function**
	- $f(x) = a\sin(bx$)
	- $a \neq 0$ and $b \neq 0$
	- graph shows a periodic wave with **amplitude** $a$ and **(angular) frequency** $b$
9. **Cosine Function**
	- $f(x) = a\cos(bx)$
	- $a \neq 0$ and $b \neq 0$
	- graph is similar to sine function
## Applications
- Functions can be observed in the real world.
	- **Power consumption**: a function of time of usage, number of appliances, or number of users
	- **COVID-19 Tracker**: the tracker is a function output itself
	- etc.
## Deterministic and Probabilistic/Stochastic Behaviour
1. **Deterministic Behaviour**
	- output is **fully determined** by the parameter values and the initial conditions
2. **Probabilistic or Stochastic Behaviour**
	- same set of parameter values and initial conditions will lead to an **ensemble of different outputs**
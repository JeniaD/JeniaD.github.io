---
title: Calculus course
date: 2024-10-11 19:00:00 +0200
categories: [University, math]
tags: [math]
math: true
---

This is my notes I made studying in my calculus course in university. The content of the course was, of course, broader, but this is the things which were required to know on the final exam.

## Trigonometric functions

| Angle (°) | Radians   | sin(θ)   | cos(θ)   | tan(θ)   | csc(θ)   | sec(θ)   | cot(θ)   |
|-----------|-----------|----------|----------|----------|----------|----------|----------|
| 0°        | 0         | 0        | 1        | 0        | ∞        | 1        | ∞        |
| 30°       | π/6       | 1/2      | √3/2     | 1/√3     | 2        | 2/√3     | √3       |
| 45°       | π/4       | √2/2     | √2/2     | 1        | √2       | √2       | 1        |
| 60°       | π/3       | √3/2     | 1/2      | √3       | 2/√3     | 2        | 1/√3     |
| 90°       | π/2       | 1        | 0        | ∞        | 1        | ∞        | 0        |
| 120°      | 2π/3      | √3/2     | -1/2     | -√3      | 2/√3     | -2       | -1/√3    |
| 135°      | 3π/4      | √2/2     | -√2/2    | -1       | √2       | -√2      | -1       |
| 150°      | 5π/6      | 1/2      | -√3/2    | -1/√3    | 2        | -2/√3    | -√3      |
| 180°      | π         | 0        | -1       | 0        | ∞        | -1       | ∞        |


## Inverse Functions
A function has an inverse if it is **bijective** (one-to-one and onto). To verify that a linear function has an inverse, we ensure it is injective (one-to-one). For example,

$$
f(x) = kx + b
$$

$$
kx_1 + b = kx_2 + b
$$

$$
kx_1 = kx_2 
$$

$$
x_1 = x_2
$$

This confirms that $f(x)$ has an inverse, as it passes the injectivity test.

To find the inverse function:

$$
y = kx + b
$$

$$
x = ky + b \implies ky = x - b
$$

$$
y = \frac{x - b}{k}
$$

Thus, the inverse of $f(x)$ is:

$$
f^{-1}(x) = \frac{x - b}{k}
$$

## Even and Odd Functions
A function is **even** if:

$$
f(x) = f(-x)
$$

Even functions are symmetric about the $y$-axis. Examples include $f(x) = x^2$ and $f(x) = \cos(x)$.

A function is **odd** if:

$$
-f(x) = f(-x)
$$

Odd functions are symmetric about the origin $(0,0)$. Examples include $f(x) = x^3$ and $f(x) = \sin(x)$.

## Limits
Some important limits include:

$$
\lim_{x \to 0} \frac{\sin x}{x} = 1 
$$

$$
\lim_{x \to \infty} \left(1 + \frac{k}{x}\right)^x = e^k 
$$

$$
\lim_{x \to 0} \frac{\cos x - 1}{x^2} = -\frac{1}{2} 
$$

$$
\lim_{x \to a} \frac{f(x)}{g(x)} = \lim_{x \to a} \frac{f'(x)}{g'(x)}
$$

## Derivatives

$(c)' = 0$

$(x^n)' = nx^{n-1}$

$(\sqrt{x})' = \frac{1}{2\sqrt{x}}$

$\left(\frac{1}{x}\right)' = -\frac{1}{x^2}$

$(e^x)' = e^x$

$(a^x)' = a^x \ln a$
$(\ln x)' = \frac{1}{x}$

$(\log_a x)' = \frac{1}{x \ln a}$

$(\sin x)' = \cos x$

$(\cos x)' = -\sin x$

$(\tan x)' = \frac{1}{\cos^2 x} = \sec^2 x$

$(\cot x)' = -\frac{1}{\sin^2 x} = -\csc^2 x$

$(\arcsin x)' = \frac{1}{\sqrt{1 - x^2}}$

$(\arccos x)' = -\frac{1}{\sqrt{1 - x^2}}$

$(\arctan x)' = \frac{1}{1 + x^2}$

$(\text{arccot} x)' = -\frac{1}{1 + x^2}$

$(f(x)g(x))' = f'(x)g(x) + f(x)g'(x)$

$\left( \frac{f(x)}{g(x)} \right)' = \frac{f'(x)g(x) - f(x)g'(x)}{g(x)^2}$

$\frac{d}{dx} f(g(x)) = f'(g(x)) \cdot g'(x)$

---

The derivative at point $ x_0 $ is given by:

$$
f'(x_0) = \lim_{\Delta x \to 0} \frac{f(x_0 + \Delta x) - f(x_0)}{\Delta x}
$$

---

The equation of the tangent line at the point $ (x_0, y_0) $ is:

$$
y - y_0 = f'(x_0)(x - x_0)
$$

The equation of the normal line (perpendicular to the tangent) is:

$$
y - y_0 = -\frac{1}{f'(x_0)}(x - x_0)
$$

...this can be rearranged to express $f(x)$ as follows:

$$
f(x) \approx f(x_0) + f'(x_0)(x - x_0)
$$

## Taylor's Theorem
Taylor's theorem states that a function $ f(x) $ can be approximated by a polynomial $ M_n(f, x) $ near a point $ x_0 $. 

The Taylor polynomial of degree $ n $ around the point $ x_0 $ is given by:

$$
T_n(f, x, x_0) = f(x_0) + \frac{f'(x_0)}{1!}(x - x_0) + \frac{f''(x_0)}{2!}(x - x_0)^2 + \cdots + \frac{f^{(n)}(x_0)}{n!}(x - x_0)^n
$$

This shows that the function can be represented as its polynomial approximation plus the error term.

## Integration
### Substitution Method

The substitution method (or *u-substitution*) is used to simplify integrals by making a substitution that reduces the integrand to a basic form.

**General Idea:**  
Let $u = g(x)$ so that $du = g'(x) dx$. The integral becomes:
$$
\int f(g(x)) g'(x) dx = \int f(u) du.
$$

**Example:**  
Evaluate
$$
\int 2x \cos(x^2) dx.
$$

Let $u = x^2$ so that derivative of it is $du = 2x dx$. Then, we can find $dx=\frac{1}{2x}du$. After we substitute, integral then transforms to:
$$
\int 2x \cos(u) \frac{1}{2x} du = \int \cos(u) du = \sin(u) + C = \sin(x^2) + C.
$$

---

### Integration by Parts

Integration by parts is based on the product rule for differentiation. The formula is:
$$
\int u' v = uv - \int u v' .
$$

**Example:**  
Evaluate
$$
\int x e^x dx.
$$

Choose $u = x$ (later when we'll find derivation of it, it will turn to $1$, which is helpful) and $dv = e^x dx$ ($ e^{x} $ will not change as $ (e^{x})' = e^{x} $). Then,
$$
\int x e^x dx = x e^x - \int e^x dx = x e^x - e^x + C = e^x(x - 1) + C.
$$

---

### Partial Fractions

Partial fractions break down complex rational functions into simpler fractions that are easier to integrate.

**Example:**  
Evaluate
$$
\int \frac{1}{(x-1)(x+2)} dx.
$$

Express the integrand as:
$$
\frac{1}{(x-1)(x+2)} = \frac{A}{x-1} + \frac{B}{x+2}.
$$
Solve for $A$ and $B$ by clearing denominators and equating coefficients. After finding the constants, integrate term by term:
$$
\int \frac{A}{x-1} dx + \int \frac{B}{x+2} dx = A \ln|x-1| + B \ln|x+2| + C.
$$


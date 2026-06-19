# The Ultimate 100 Math Questions for Beginners

This workbook contains exactly 100 foundational questions in **Linear Algebra**, **Calculus**, and **Statistics & Probability**. Working through these questions and understanding their answers will build an exceptionally strong, unbeatable foundation.

---

## 📐 Section 1: Linear Algebra (Questions 1–35)

### Vectors & Operations
1.  **What is a vector in 2D space?**
    *   *Answer:* An arrow starting at the origin \((0,0)\) pointing to a coordinate \((x, y)\), representing a direction and magnitude.
2.  **How do you add two vectors algebraically?**
    *   *Answer:* Add their corresponding components: \([a_1, a_2] + [b_1, b_2] = [a_1+b_1, a_2+b_2]\).
3.  **What is the geometric meaning of vector addition?**
    *   *Answer:* Placing the tail of the second vector at the head of the first vector ("tip-to-tail" method).
4.  **What is scalar multiplication?**
    *   *Answer:* Multiplying each component of a vector by a single number (scalar), which stretches, shrinks, or reverses the vector.
5.  **What is a unit vector?**
    *   *Answer:* A vector with a length (magnitude) of exactly 1.
6.  **How do you normalize a vector?**
    *   *Answer:* Divide the vector by its magnitude: \(\vec{u} = \vec{v} / \|\vec{v}\|\).
7.  **What is the dot product of two vectors?**
    *   *Answer:* The sum of the products of their components: \(\vec{a} \cdot \vec{b} = a_1b_1 + a_2b_2 + \dots + a_nb_n\).
8.  **What does a dot product of 0 mean?**
    *   *Answer:* The two vectors are **orthogonal** (perpendicular, at a 90-degree angle to each other).
9.  **How does the dot product relate to projection?**
    *   *Answer:* It measures how much of one vector points in the direction of another.
10. **What is a linear combination?**
    *   *Answer:* Multiplying vectors by scalars and adding them together: \(c_1\vec{v}_1 + c_2\vec{v}_2 + \dots + c_k\vec{v}_k\).

### Matrices & Transformations
11. **What is a matrix?**
    *   *Answer:* A rectangular grid of numbers representing a linear transformation of space.
12. **What does the dimension of a matrix (e.g., \(3 \times 2\)) mean?**
    *   *Answer:* It has 3 rows and 2 columns.
13. **How do you multiply a matrix by a vector?**
    *   *Answer:* The output is a linear combination of the matrix's columns, weighted by the vector's components.
14. **What is the Identity Matrix (\(I\))?**
    *   *Answer:* A square matrix with 1s on the diagonal and 0s elsewhere. It acts like the number 1 (multiplying a vector by \(I\) doesn't change it).
15. **What is the transpose of a matrix (\(A^T\))?**
    *   *Answer:* A matrix flipped over its diagonal, turning rows into columns and columns into rows.
16. **When is matrix multiplication (\(AB\)) possible?**
    *   *Answer:* When the number of columns in \(A\) equals the number of rows in \(B\).
17. **Is matrix multiplication commutative (\(AB = BA\))?**
    *   *Answer:* No, order matters. Rotating then flipping is different from flipping then rotating.
18. **What is a diagonal matrix?**
    *   *Answer:* A matrix where all non-diagonal elements are 0. It scales each dimension independently.
19. **What is a symmetric matrix?**
    *   *Answer:* A matrix that is equal to its transpose: \(A = A^T\).

### Linear Systems & Space Properties
20. **What is a system of linear equations?**
    *   *Answer:* A collection of equations with multiple variables (e.g., \(2x + y = 5\), \(x - y = 1\)).
21. **What does it mean for a system to be "consistent"?**
    *   *Answer:* The system has at least one solution (where the lines/planes intersect).
22. **What is the Span of a set of vectors?**
    *   *Answer:* The set of all possible vectors that can be reached by any linear combination of the original vectors.
23. **What is linear independence?**
    *   *Answer:* A set of vectors is independent if no vector in the set can be written as a linear combination of the others.
24. **What is a Basis of a vector space?**
    *   *Answer:* A set of linearly independent vectors that spans the entire space.
25. **What is the dimension of a vector space?**
    *   *Answer:* The number of vectors in its basis (e.g., 2D space has a basis of 2 vectors).

### Determinants & Eigensystems
26. **What is the Determinant of a \(2 \times 2\) matrix \(\begin{bmatrix} a & b \\ c & d \end{bmatrix}\)?**
    *   *Answer:* \(ad - bc\).
27. **What is the physical interpretation of a determinant?**
    *   *Answer:* The scaling factor of area/volume after space is transformed by the matrix.
28. **What does a negative determinant mean?**
    *   *Answer:* The transformation flipped the orientation of space (like looking in a mirror).
29. **What is an inverse matrix (\(A^{-1}\))?**
    *   *Answer:* A matrix that reverses the effect of \(A\), satisfying \(AA^{-1} = I\).
30. **Under what condition does a matrix NOT have an inverse?**
    *   *Answer:* When its determinant is 0.
31. **What is an eigenvector?**
    *   *Answer:* A vector that does not change direction when multiplied by a matrix.
32. **What is an eigenvalue?**
    *   *Answer:* The factor by which the eigenvector stretches or shrinks during the transformation.
33. **What is the characteristic equation?**
    *   *Answer:* \(\det(A - \lambda I) = 0\), used to solve for eigenvalues \(\lambda\).
34. **What is the Trace of a matrix?**
    *   *Answer:* The sum of the elements on the main diagonal (also equal to the sum of its eigenvalues).
35. **What is the Rank of a matrix?**
    *   *Answer:* The dimension of the space spanned by its columns (the dimensions of the output space).

---

## 📈 Section 2: Calculus (Questions 36–70)

### Limits & Continuity
36. **What is a limit?**
    *   *Answer:* The value that a function approaches as the input approaches a target value.
37. **What is continuity?**
    *   *Answer:* A function is continuous if you can draw its graph without lifting your pencil (no jumps or holes).
38. **What is an asymptote?**
    *   *Answer:* A line that a curve approaches infinitely closely but never touches.
39. **What is the limit of \(1/x\) as \(x \to \infty\)?**
    *   *Answer:* 0.
40. **What is the limit of \(\frac{\sin(x)}{x}\) as \(x \to 0\)?**
    *   *Answer:* 1.

### Derivatives & Rules
41. **What is a derivative?**
    *   *Answer:* The instantaneous rate of change of a function, or the slope of the tangent line.
42. **What is the Power Rule?**
    *   *Answer:* \(\frac{d}{dx}[x^n] = n x^{n-1}\).
43. **What is the derivative of \(e^x\)?**
    *   *Answer:* \(e^x\) (it is its own derivative).
44. **What is the derivative of \(\ln(x)\)?**
    *   *Answer:* \(1/x\).
45. **What is the Product Rule?**
    *   *Answer:* \(\frac{d}{dx}[f(x)g(x)] = f'(x)g(x) + f(x)g'(x)\).
46. **What is the Quotient Rule?**
    *   *Answer:* \(\frac{d}{dx}\left[\frac{f(x)}{g(x)}\right] = \frac{f'(x)g(x) - f(x)g'(x)}{(g(x))^2}\).
47. **What is the Chain Rule?**
    *   *Answer:* \(\frac{d}{dx}[f(g(x))] = f'(g(x)) \cdot g'(x)\).
48. **What is a second derivative?**
    *   *Answer:* The derivative of the derivative, representing acceleration or curvature.
49. **What does a positive second derivative tell you?**
    *   *Answer:* The function is **concave up** (shaped like a cup \(\cup\)).
50. **What is an inflection point?**
    *   *Answer:* A point on a curve where the concavity changes (from concave up to concave down, or vice versa).

### Optimization & Applications
51. **What is a critical point?**
    *   *Answer:* A point where the derivative is either 0 or undefined.
52. **How do you find the local maximum of a function?**
    *   *Answer:* Find where the first derivative is 0, and the second derivative is negative.
53. **What is the Mean Value Theorem?**
    *   *Answer:* For a smooth curve, there is at least one point where the instantaneous slope equals the average slope over the interval.
54. **What is L'Hôpital's Rule?**
    *   *Answer:* A method to evaluate limits of indeterminate forms (like \(0/0\) or \(\infty/\infty\)) by taking derivatives of the numerator and denominator.
55. **What is a partial derivative?**
    *   *Answer:* The derivative of a multivariable function with respect to one variable, holding all other variables constant.
56. **What is the Gradient vector?**
    *   *Answer:* A vector containing all the partial derivatives of a function.
57. **In which direction does the Gradient point?**
    *   *Answer:* The direction of steepest ascent.
58. **What is Gradient Descent?**
    *   *Answer:* An iterative optimization algorithm that takes steps in the direction of the negative gradient to find a local minimum.
59. **What is the learning rate in Gradient Descent?**
    *   *Answer:* A multiplier that controls the size of the steps taken toward the minimum.

### Integrals & Integration
60. **What is an antiderivative?**
    *   *Answer:* A function whose derivative is the given function.
61. **What is an indefinite integral?**
    *   *Answer:* The family of all antiderivatives of a function, written with a constant \(+ C\).
62. **What is a definite integral?**
    *   *Answer:* The net signed area under a curve between two specific bounds.
63. **What is Riemann Summation?**
    *   *Answer:* Approximating the area under a curve by dividing it into thin vertical rectangles and adding their areas.
64. **What is the Fundamental Theorem of Calculus (Part 1)?**
    *   *Answer:* Differentiation and integration are inverse operations.
65. **What is the Fundamental Theorem of Calculus (Part 2)?**
    *   *Answer:* \(\int_a^b f(x) \, dx = F(b) - F(a)\), where \(F'(x) = f(x)\).
66. **What is Integration by Parts?**
    *   *Answer:* The reverse of the product rule: \(\int u \, dv = uv - \int v \, du\).
67. **What is the integral of \(1/x\)?**
    *   *Answer:* \(\ln|x| + C\).
68. **What does a double integral calculate?**
    *   *Answer:* The volume under a surface over a 2D region.
69. **What is a differential equation?**
    *   *Answer:* An equation that relates a function to its derivatives.
70. **What is a Taylor Series?**
    *   *Answer:* An infinite sum of terms calculated from a function's derivatives at a single point, used to approximate complex functions (like \(\sin(x)\)).

---

## 📊 Section 3: Probability & Statistics (Questions 71–100)

### Basic Probability
71. **What is a sample space?**
    *   *Answer:* The set of all possible outcomes of a random experiment.
72. **What are mutually exclusive events?**
    *   *Answer:* Events that cannot happen at the same time (e.g., rolling a 3 and a 4 on a single die).
73. **What is the sum of the probabilities of all outcomes in a sample space?**
    *   *Answer:* Exactly 1.
74. **What is conditional probability?**
    *   *Answer:* The probability of an event occurring given that another event has already occurred.
75. **What does it mean for two events to be independent?**
    *   *Answer:* The occurrence of one event does not affect the probability of the other: \(P(A \cap B) = P(A) \times P(B)\).
76. **State Bayes' Theorem mathematically.**
    *   *Answer:* \(P(A|B) = \frac{P(B|A)P(A)}{P(B)}\).
77. **What is the difference between permutation and combination?**
    *   *Answer:* Permutations care about order (e.g., race podium); combinations do not (e.g., choosing a committee).
78. **What is the formula for combinations ("n choose k")?**
    *   *Answer:* \(\binom{n}{k} = \frac{n!}{k!(n-k)!}\).

### Random Variables & Distributions
79. **What is a random variable?**
    *   *Answer:* A rule that assigns a numerical value to each outcome in a sample space.
80. **What is the difference between a PMF and a PDF?**
    *   *Answer:* A PMF (Probability Mass Function) is for discrete variables (exact probabilities); a PDF (Probability Density Function) is for continuous variables (probability is the area under the curve).
81. **What is the Cumulative Distribution Function (CDF)?**
    *   *Answer:* A function giving the probability that a random variable is less than or equal to a value \(x\): \(F(x) = P(X \le x)\).
82. **What is the Expected Value of a random variable?**
    *   *Answer:* The long-term average value of the random variable: \(E[X] = \sum x P(x)\).
83. **What is variance mathematically?**
    *   *Answer:* The expected squared deviation from the mean: \(\text{Var}(X) = E[(X - \mu)^2] = E[X^2] - (E[X])^2\).
84. **How do you get Standard Deviation from Variance?**
    *   *Answer:* Take the square root of the variance (\(\sigma = \sqrt{\text{Var}}\)).
85. **What is a Bernoulli Trial?**
    *   *Answer:* A random experiment with exactly two outcomes: success (1) and failure (0).
86. **What does a Binomial Distribution model?**
    *   *Answer:* The number of successes in \(n\) independent Bernoulli trials (e.g., number of heads in 10 coin flips).
87. **Describe the shape of a Normal (Gaussian) Distribution.**
    *   *Answer:* A symmetric, bell-shaped curve centered around the mean.
88. **What is the Empirical Rule (68-95-99.7 rule) for normal distributions?**
    *   *Answer:* 68% of data falls within 1 standard deviation of the mean, 95% within 2, and 99.7% within 3.
89. **What is a Standard Normal Distribution?**
    *   *Answer:* A normal distribution with a mean of 0 and a standard deviation of 1.

### Statistical Inference
90. **What is the difference between a population and a sample?**
    *   *Answer:* A population is the entire group of interest; a sample is a subset of the population used to collect data.
91. **What is the Central Limit Theorem?**
    *   *Answer:* The rule that sample means will be normally distributed regardless of the shape of the population distribution, as long as the sample size is large.
92. **What is a Parameter vs. a Statistic?**
    *   *Answer:* A parameter is a characteristic of a population (e.g., population mean \(\mu\)); a statistic is calculated from a sample (e.g., sample mean \(\bar{x}\)).
93. **What is a Null Hypothesis (\(H_0\))?**
    *   *Answer:* The hypothesis of no effect, no difference, or status quo.
94. **What is an Alternative Hypothesis (\(H_a\))?**
    *   *Answer:* The claim we want to gather evidence to support (e.g., "the drug works").
95. **What is Type I Error?**
    *   *Answer:* A false positive: rejecting the null hypothesis when it is actually true.
96. **What is Type II Error?**
    *   *Answer:* A false negative: failing to reject the null hypothesis when it is actually false.
97. **What is a p-value?**
    *   *Answer:* The probability of getting results as extreme as ours if the null hypothesis is true.
98. **What is a Confidence Interval?**
    *   *Answer:* A range of values that is likely to contain the true population parameter with a specified level of confidence (e.g., 95%).
99. **What is a Z-score?**
    *   *Answer:* The number of standard deviations a data point is from the mean: \(z = (x - \mu)/\sigma\).
100. **What is the difference between Correlation and Causation?**
    *   *Answer:* Correlation means two variables move together; Causation means one variable directly *causes* the change in the other.

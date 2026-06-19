# The Complete Mathematics of Machine Learning

> [!NOTE]
> This is an **exhaustive** guide. Every concept includes: **Definition → Intuition → Worked Example → Where It's Used in ML → How It's Used**. Read it like a textbook.

---

# Chapter 1: Linear Algebra

Linear algebra is the **language** of machine learning. Every dataset, every model parameter, every transformation is expressed using linear algebra.

---

## 1.1 Scalars

### Definition
A scalar is a single real number. Denoted by lowercase italic letters.

```
a = 5,    b = -3.7,    α = 0.01
```

### Where Used in ML
| ML Concept | Scalar Example |
|---|---|
| Learning rate | α = 0.001 |
| Regularization strength | λ = 0.01 |
| Loss value | L = 2.34 |
| A single prediction | ŷ = 0.87 |
| Bias term | b = 1.5 |
| A single pixel value | pixel = 128 |

### How Used
Every hyperparameter you tune is a scalar. When you set `learning_rate = 0.001`, that's a scalar controlling how big each gradient descent step is:
```
w_new = w_old - 0.001 × gradient
           ↑                
        scalar α            
```

---

## 1.2 Vectors

### Definition
A **vector** is an ordered list of numbers. It has both **magnitude** (length) and **direction**.

```
v = [v₁, v₂, ..., vₙ]ᵀ    (column vector by convention)

Example:
v = [3, 1, 4]ᵀ   ← a 3-dimensional vector
```

**Dimension/Size**: The number of elements. Vector v above is 3-dimensional (lives in ℝ³).

### Intuition
Think of a vector as:
- **Geometrically**: An arrow pointing from the origin to a point in space
- **In ML**: A single data point described by its features

```
A house: [area=1500, bedrooms=3, age=10] → a point in 3D feature space
A word:  [0.2, -0.5, 0.8, ...]          → a point in embedding space (Word2Vec)
An image: [pixel₁, pixel₂, ..., pixel₇₈₄] → a point in 784D space (28×28 MNIST)
```

### Key Vector Operations

#### Vector Addition
```
u + v = [u₁+v₁, u₂+v₂, ..., uₙ+vₙ]

Example:
[1, 2, 3] + [4, 5, 6] = [5, 7, 9]
```

**Where used:** Adding bias to a weighted sum in neural networks: `z = Wx + b` (vector + vector).

#### Scalar Multiplication
```
c × v = [c×v₁, c×v₂, ..., c×vₙ]

Example:
3 × [1, 2, 3] = [3, 6, 9]
```

**Where used:** Scaling gradients by the learning rate: `α × ∇L` scales the gradient vector.

#### Vector Norm (Magnitude / Length)

**L2 Norm (Euclidean):**
```
‖v‖₂ = √(v₁² + v₂² + ... + vₙ²) = √(Σ vᵢ²)

Example:
‖[3, 4]‖₂ = √(9 + 16) = √25 = 5
```

**L1 Norm (Manhattan):**
```
‖v‖₁ = |v₁| + |v₂| + ... + |vₙ| = Σ |vᵢ|

Example:
‖[3, -4]‖₁ = 3 + 4 = 7
```

**Where used in ML:**

| Norm | ML Application | How |
|---|---|---|
| L2 Norm | **L2 Regularization (Ridge)** | Penalty = λ‖w‖₂² added to loss. Pushes weights toward zero smoothly. Prevents overfitting. |
| L2 Norm | **Distance in KNN** | Distance between two points: ‖x₁ - x₂‖₂. K-Nearest Neighbors finds the k training points closest to a new input. |
| L2 Norm | **SVM margin** | Margin = 2/‖w‖₂. SVM maximizes this by minimizing ‖w‖₂. |
| L1 Norm | **L1 Regularization (Lasso)** | Penalty = λ‖w‖₁. Pushes some weights to **exactly zero** → automatic feature selection. |
| L1 Norm | **Robust loss (MAE)** | Mean Absolute Error uses L1 distance: (1/m)Σ\|yᵢ - ŷᵢ\|. Less sensitive to outliers than MSE. |

---

## 1.3 The Dot Product (Inner Product)

### Definition
```
a · b = a₁b₁ + a₂b₂ + ... + aₙbₙ = Σᵢ aᵢbᵢ = aᵀb

Example:
[1, 2, 3] · [4, 5, 6] = 1×4 + 2×5 + 3×6 = 4 + 10 + 18 = 32
```

### Geometric Interpretation
```
a · b = ‖a‖ × ‖b‖ × cos(θ)

Where θ is the angle between vectors a and b
```

This means:
- If θ = 0° (same direction) → cos(0) = 1 → dot product is **maximized** (positive)
- If θ = 90° (perpendicular) → cos(90) = 0 → dot product is **zero**
- If θ = 180° (opposite) → cos(180) = -1 → dot product is **negative**

> **The dot product measures how much two vectors point in the same direction — i.e., their SIMILARITY.**

### Where Used in ML — EVERYWHERE

> [!IMPORTANT]
> The dot product is the **single most fundamental operation** in all of ML. Almost every ML computation reduces to a dot product.

| ML Algorithm | How the Dot Product is Used |
|---|---|
| **Linear Regression** | Prediction: `ŷ = wᵀx + b = w·x + b`. The prediction is literally a dot product of weights and features. |
| **Logistic Regression** | `z = wᵀx + b`, then `σ(z)`. Classification starts with a dot product. |
| **Every Neuron** | A neuron computes `activation(wᵀx + b)`. **A neuron IS a dot product + nonlinearity.** |
| **Cosine Similarity** | `sim(a,b) = (a·b)/(‖a‖‖b‖)`. Used in recommendation systems, NLP, search engines to find similar items. |
| **SVM** | Kernel functions compute dot products in high-dimensional spaces: `K(x,z) = φ(x)·φ(z)`. |
| **Attention (Transformers)** | `Attention(Q,K,V) = softmax(QKᵀ/√d)V`. The QKᵀ term is a matrix of dot products measuring how much each query "attends to" each key. |

### Worked Example — A Neuron's Computation

```
Input features:  x = [hours_studied, hours_slept, practice_tests]
                   = [5, 8, 3]

Learned weights: w = [0.6, 0.3, 0.4]
Bias:            b = -2.0

Step 1 — Dot product:
z = w · x + b
  = (0.6×5) + (0.3×8) + (0.4×3) + (-2.0)
  = 3.0 + 2.4 + 1.2 - 2.0
  = 4.6

Step 2 — Activation (sigmoid):
ŷ = σ(4.6) = 1/(1+e⁻⁴·⁶) ≈ 0.99

→ Prediction: 99% chance of passing the exam
```

---

## 1.4 Matrices

### Definition
A matrix is a 2D rectangular array of numbers with **m rows** and **n columns**, written as **A ∈ ℝᵐˣⁿ**.

```
A = | 1  2  3 |
    | 4  5  6 |     ← 2×3 matrix (2 rows, 3 columns)

Aᵢⱼ = element at row i, column j
A₁₂ = 2, A₂₃ = 6
```

### What Matrices Represent in ML

| Matrix | Rows | Columns | Example |
|---|---|---|---|
| **Dataset X** | Samples (m) | Features (n) | 1000 patients × 10 measurements |
| **Weight matrix W** | Output neurons | Input neurons | Layer connecting 784→128 neurons |
| **Image** | Height | Width | 28×28 grayscale image |
| **Confusion Matrix** | Actual classes | Predicted classes | 10×10 for digit classification |
| **Covariance Matrix** | Features | Features | n×n showing feature correlations |

---

## 1.5 Matrix Operations

### Matrix Addition
```
(A + B)ᵢⱼ = Aᵢⱼ + Bᵢⱼ    (element-wise, same shape required)
```

### Matrix-Vector Multiplication
```
If A is (m×n) and x is (n×1), then y = Ax is (m×1)

yᵢ = Σⱼ Aᵢⱼ × xⱼ = row i of A  ·  x    (dot product!)
```

**Where used:** This IS a neural network layer!

```
Layer computation:  z = Wx + b

W = | w₁₁  w₁₂  w₁₃ |     x = | x₁ |     b = | b₁ |
    | w₂₁  w₂₂  w₂₃ |         | x₂ |         | b₂ |
                                | x₃ |

z = | w₁₁x₁ + w₁₂x₂ + w₁₃x₃ + b₁ |   =  | neuron 1 output |
    | w₂₁x₁ + w₂₂x₂ + w₂₃x₃ + b₂ |      | neuron 2 output |
```

> Each **row** of W contains one neuron's weights. Matrix-vector multiplication computes **all neurons simultaneously**.

### Matrix-Matrix Multiplication

```
If A is (m×n) and B is (n×p), then C = AB is (m×p)

Cᵢⱼ = Σₖ Aᵢₖ × Bₖⱼ = row i of A  ·  column j of B
```

**Where used:**
- **Batch processing**: `Z = XW` computes predictions for ALL samples at once
- **Multi-layer networks**: Output of one layer feeds into the next
- **Attention mechanism**: `QKᵀ` computes all attention scores simultaneously

### Worked Example — Batch Prediction

```
Dataset X (3 samples × 2 features):     Weights W (2 features → 1 output):
| 1  2 |                                  | 0.5 |
| 3  4 |                                  | 0.3 |
| 5  6 |

Predictions: ŷ = XW
| 1×0.5 + 2×0.3 |   | 1.1 |
| 3×0.5 + 4×0.3 | = | 2.7 |   ← all 3 predictions computed at once!
| 5×0.5 + 6×0.3 |   | 4.3 |
```

---

## 1.6 Matrix Transpose

### Definition
```
(Aᵀ)ᵢⱼ = Aⱼᵢ     (flip rows and columns)

If A is (m×n), then Aᵀ is (n×m)

Example:
A = | 1  2  3 |      Aᵀ = | 1  4 |
    | 4  5  6 |           | 2  5 |
                           | 3  6 |
```

### Properties
```
(AB)ᵀ = BᵀAᵀ     ← order reverses!
(Aᵀ)ᵀ = A
(A+B)ᵀ = Aᵀ+Bᵀ
```

### Where Used in ML

| Application | How |
|---|---|
| **Dot product as matrix multiply** | `aᵀb = [a₁ a₂ a₃] × [b₁; b₂; b₃]` = scalar |
| **Normal Equation** | `w = (XᵀX)⁻¹Xᵀy` — XᵀX creates a square matrix of feature correlations |
| **Covariance matrix** | `C = (1/m) XᵀX` (after centering) |
| **Gradient of linear layer** | `∂L/∂W = (∂L/∂z)ᵀ × x` — the transpose aligns dimensions |
| **Attention scores** | `QKᵀ` — transpose K so each query gets dotted with each key |

---

## 1.7 Matrix Inverse

### Definition
```
A⁻¹ is the matrix such that:  A × A⁻¹ = A⁻¹ × A = I

Where I is the identity matrix:
I = | 1  0  0 |
    | 0  1  0 |
    | 0  0  1 |
```

Only **square** matrices can have inverses, and only if they are **non-singular** (determinant ≠ 0).

### 2×2 Inverse Formula
```
A = | a  b |     A⁻¹ = (1/det(A)) × |  d  -b |
    | c  d |                          | -c   a |

Where det(A) = ad - bc

Example:
A = | 2  1 |    det = 2×4 - 1×3 = 5
    | 3  4 |    

A⁻¹ = (1/5) × |  4  -1 | = | 0.8  -0.2 |
               | -3   2 |   | -0.6  0.4 |

Verify: A × A⁻¹ = | 2×0.8+1×(-0.6)   2×(-0.2)+1×0.4 | = | 1  0 | = I  ✓
                   | 3×0.8+4×(-0.6)   3×(-0.2)+4×0.4 |   | 0  1 |
```

### Where Used in ML

| Application | Formula | How |
|---|---|---|
| **Normal Equation (Linear Regression)** | `w = (XᵀX)⁻¹Xᵀy` | Solves linear regression in one step without iteration. XᵀX is a (n×n) matrix where n = number of features. Inversion costs O(n³), so this is only practical when n is small (<10,000). |
| **Multivariate Gaussian** | `p(x) = ... exp(-½(x-μ)ᵀΣ⁻¹(x-μ))` | The inverse covariance matrix Σ⁻¹ (called the "precision matrix") appears in the Gaussian density formula. Used in Gaussian Mixture Models, Bayesian methods. |
| **Newton's Method** | `w ← w - H⁻¹∇L` | Uses the inverse of the Hessian matrix (second derivatives) for faster convergence than gradient descent. Used in logistic regression optimization. |

> [!WARNING]
> Matrix inversion is numerically unstable and expensive. In practice, we solve systems of equations using decompositions (LU, Cholesky, QR) instead of explicitly computing A⁻¹.

---

## 1.8 Determinant

### Definition
A scalar that tells you about the matrix's properties.

**2×2:**
```
det(| a  b |) = ad - bc
    | c  d |
```

**3×3 (cofactor expansion):**
```
det(| a  b  c |)
    | d  e  f |  = a(ei-fh) - b(di-fg) + c(dh-eg)
    | g  h  i |
```

### Key Properties
```
det(A) = 0  ↔  A is singular (no inverse, linearly dependent columns)
det(A) ≠ 0  ↔  A is invertible
det(AB) = det(A) × det(B)
det(Aᵀ) = det(A)
det(cA) = cⁿ × det(A)   for n×n matrix
```

### Geometric Meaning
The determinant tells you the **volume scaling factor** of the transformation. If det = 0, the transformation collapses space to a lower dimension (you lose information).

### Where Used in ML

| Application | How |
|---|---|
| **Multivariate Gaussian** | The normalization constant includes `1/√det(Σ)`. If det(Σ)=0, the distribution is degenerate. |
| **Checking data quality** | If det(XᵀX) ≈ 0, features are nearly linearly dependent → **multicollinearity** → regression is unstable. |
| **Linear independence** | det ≠ 0 means the feature columns are linearly independent → no redundant features. |

---

## 1.9 Eigenvalues and Eigenvectors

### Definition
For a square matrix A, a vector **v** is an eigenvector and scalar **λ** is its eigenvalue if:

```
Av = λv

"Multiplying v by A only SCALES v by λ — it doesn't change v's direction"
```

### How to Find Them
```
Av = λv
Av - λv = 0
(A - λI)v = 0

This has a non-trivial solution when:
det(A - λI) = 0    ← "characteristic equation"
```

### Worked Example

```
A = | 4  1 |
    | 2  3 |

Characteristic equation:
det(A - λI) = det(| 4-λ   1  |) = (4-λ)(3-λ) - 2 = 0
                  |  2   3-λ |

λ² - 7λ + 10 = 0
(λ - 5)(λ - 2) = 0

λ₁ = 5,  λ₂ = 2
```

Finding eigenvectors:

```
For λ₁ = 5:
(A - 5I)v = 0 → | -1  1 | v = 0 → v₁ = [1, 1]ᵀ
                 |  2 -2 |

For λ₂ = 2:
(A - 2I)v = 0 → | 2  1 | v = 0 → v₂ = [1, -2]ᵀ
                 | 2  1 |
```

### Intuition
- **Eigenvectors** = the special directions that don't rotate under the transformation, only stretch/shrink
- **Eigenvalues** = how much stretching happens along each eigenvector direction

### Where Used in ML

| Application | How | Why |
|---|---|---|
| **PCA** | Find eigenvectors of the covariance matrix | Eigenvectors point in directions of **maximum variance**. Eigenvalues tell you **how much** variance. You keep the top-k eigenvectors to reduce dimensionality while losing minimal information. |
| **Spectral Clustering** | Find eigenvectors of the graph Laplacian matrix | Eigenvectors reveal the natural cluster structure of a graph. The second-smallest eigenvector (Fiedler vector) gives the optimal 2-way partition. |
| **Google PageRank** | Find the dominant eigenvector of the web link matrix | The eigenvector corresponding to λ=1 of the transition matrix gives the steady-state probability of being on each page = the page's importance ranking. |
| **Stability Analysis** | Check eigenvalues of the Jacobian | If all eigenvalues have |λ| < 1, the system (e.g., recurrent neural network) is stable. If |λ| > 1, gradients explode. |
| **Covariance understanding** | Eigendecompose Σ | Reveals the principal axes of the data's ellipsoidal distribution. |

### PCA Walkthrough — Using Eigenvalues

```
Dataset: 3 students, 2 features (math score, physics score)

X = | 90  85 |     (after centering: subtract mean of each column)
    | 80  75 |     
    | 70  65 |     

X̃ = | 10  10 |
    |  0   0 |
    |-10 -10 |

Covariance matrix:
C = (1/3) X̃ᵀX̃ = (1/3) | 200  200 | = | 66.7  66.7 |
                        | 200  200 |   | 66.7  66.7 |

Eigenvalues:  λ₁ = 133.3,  λ₂ = 0
Eigenvectors: v₁ = [1/√2, 1/√2],  v₂ = [1/√2, -1/√2]

Variance explained by first component: λ₁/(λ₁+λ₂) = 133.3/133.3 = 100%

→ Math and physics scores are perfectly correlated!
→ One component captures ALL the information.
→ Reduce from 2D to 1D with zero information loss.
```

---

## 1.10 Singular Value Decomposition (SVD)

### Definition
ANY matrix (not just square) can be decomposed as:
```
A = UΣVᵀ

Where:
  A is (m×n)
  U is (m×m) — orthogonal matrix (left singular vectors)
  Σ is (m×n) — diagonal matrix (singular values σ₁ ≥ σ₂ ≥ ... ≥ 0)
  V is (n×n) — orthogonal matrix (right singular vectors)
```

### Where Used in ML

| Application | How |
|---|---|
| **PCA (practical)** | SVD of the data matrix directly gives PCA components without computing the covariance matrix (numerically more stable) |
| **Recommendation Systems** | Decompose the user-item rating matrix. Users and items get mapped to a shared latent factor space. |
| **Image Compression** | Keep only the top k singular values. Reconstruct: A ≈ U[:,:k] Σ[:k,:k] V[:,:k]ᵀ |
| **Pseudoinverse** | A⁺ = VΣ⁺Uᵀ — used to solve overdetermined systems (more equations than unknowns) |
| **Latent Semantic Analysis** | SVD of term-document matrix reveals latent topics in text data |

---

## 1.11 Broadcasting and Hadamard Product

### Hadamard Product (Element-wise Multiplication)
```
A ⊙ B = element-by-element multiplication

| 1  2 |   | 5  6 |   | 5   12 |
| 3  4 | ⊙ | 7  8 | = | 21  32 |
```

**Where used:**
- **Backpropagation**: `∂L/∂z = (∂L/∂h) ⊙ σ'(z)` — multiply gradient by activation derivative element-wise
- **Dropout**: `h_dropped = h ⊙ mask` — randomly zero out neurons by multiplying with a binary mask
- **Attention masking**: `scores = scores ⊙ mask` — prevent attending to padding or future tokens

### Broadcasting
Adding a vector to every row of a matrix:
```
Z = XW + b    where b is a vector, but gets added to every row of XW
```

**Where used:** Adding bias in neural network layers. Every sample gets the same bias added.

---

# Chapter 2: Calculus

Calculus answers the question: **How do I adjust my model's parameters to reduce the error?**

---

## 2.1 Limits

### Definition
```
lim(x→a) f(x) = L

"As x gets closer and closer to a, f(x) gets closer and closer to L"
```

### Where Used in ML
- The **derivative** is defined as a limit
- Understanding what happens as learning rate → 0 (convergence behavior)
- Understanding asymptotic behavior (e.g., sigmoid as z → ±∞)

---

## 2.2 Derivatives

### Definition
```
f'(x) = df/dx = lim(h→0) [f(x+h) - f(x)] / h

"The instantaneous rate of change of f at point x"
"The slope of the tangent line at x"
```

### Essential Derivative Rules

| Rule | Formula | Example |
|---|---|---|
| **Constant** | d/dx [c] = 0 | d/dx [5] = 0 |
| **Power** | d/dx [xⁿ] = nxⁿ⁻¹ | d/dx [x³] = 3x² |
| **Sum** | d/dx [f+g] = f'+g' | d/dx [x²+3x] = 2x+3 |
| **Product** | d/dx [fg] = f'g + fg' | d/dx [x·eˣ] = eˣ + xeˣ |
| **Quotient** | d/dx [f/g] = (f'g-fg')/g² | |
| **Chain** | d/dx [f(g(x))] = f'(g(x))·g'(x) | d/dx [e²ˣ] = 2e²ˣ |
| **Exponential** | d/dx [eˣ] = eˣ | |
| **Logarithm** | d/dx [ln(x)] = 1/x | |

### Derivatives of ML Activation Functions

These are critical — you'll use them every time you do backpropagation:

**Sigmoid:**
```
σ(z) = 1/(1+e⁻ᶻ)

σ'(z) = σ(z) × (1 - σ(z))

Proof:
σ'(z) = d/dz [1/(1+e⁻ᶻ)]
      = e⁻ᶻ / (1+e⁻ᶻ)²
      = [1/(1+e⁻ᶻ)] × [e⁻ᶻ/(1+e⁻ᶻ)]
      = σ(z) × [1 - 1/(1+e⁻ᶻ)]
      = σ(z) × (1 - σ(z))           ✓
```
> **Where used:** Output layer of binary classifiers. During backprop, the gradient flows through σ'(z). Note: σ'(z) is maximum 0.25 (at z=0), which causes the **vanishing gradient problem** for deep networks.

**ReLU:**
```
ReLU(z) = max(0, z)

ReLU'(z) = { 0  if z < 0
            { 1  if z > 0
            { undefined at z = 0 (conventionally set to 0 or 1)
```
> **Where used:** Default hidden layer activation. Gradient is either 0 or 1 — no vanishing gradient for positive values! But "dead neurons" can occur when z < 0 always.

**Tanh:**
```
tanh(z) = (eᶻ - e⁻ᶻ) / (eᶻ + e⁻ᶻ)

tanh'(z) = 1 - tanh²(z)
```
> **Where used:** Hidden layers when you need output centered around 0 (range [-1, 1]). Used in LSTMs and GRUs.

**Softmax (for vector inputs):**
```
softmax(zᵢ) = eᶻⁱ / Σⱼ eᶻʲ

∂softmax(zᵢ)/∂zⱼ = softmax(zᵢ) × (δᵢⱼ - softmax(zⱼ))

Where δᵢⱼ = 1 if i=j, else 0 (Kronecker delta)
```
> **Where used:** Output layer for multi-class classification (e.g., digit recognition with 10 classes). Converts raw scores (logits) to probabilities that sum to 1.

---

## 2.3 Partial Derivatives

### Definition
```
∂f/∂x = the derivative of f with respect to x, treating all other variables as constants
```

### Worked Example
```
f(x, y) = 3x²y + 2xy³ + 5

∂f/∂x = 6xy + 2y³       (treat y as constant)
∂f/∂y = 3x² + 6xy²      (treat x as constant)
```

### Where Used in ML

In ML, your loss function depends on **many** parameters (weights). You need the partial derivative with respect to each one.

```
Loss L(w₁, w₂, w₃, b) = (1/2m) Σᵢ (w₁x₁ᵢ + w₂x₂ᵢ + w₃x₃ᵢ + b - yᵢ)²

∂L/∂w₁ = (1/m) Σᵢ (ŷᵢ - yᵢ) × x₁ᵢ     ← how much does L change if I nudge w₁?
∂L/∂w₂ = (1/m) Σᵢ (ŷᵢ - yᵢ) × x₂ᵢ     ← how much does L change if I nudge w₂?
∂L/∂w₃ = (1/m) Σᵢ (ŷᵢ - yᵢ) × x₃ᵢ
∂L/∂b  = (1/m) Σᵢ (ŷᵢ - yᵢ)

Each partial derivative tells you: "if you increase this particular weight by a tiny amount,
how much does the loss go up or down?"
```

---

## 2.4 The Gradient

### Definition
The gradient is a **vector of all partial derivatives**:
```
∇f = [∂f/∂x₁, ∂f/∂x₂, ..., ∂f/∂xₙ]ᵀ
```

### Key Property
> **The gradient points in the direction of steepest ascent of f.**
>
> Therefore, **−∇f** points in the direction of **steepest descent**.

### Where Used in ML — GRADIENT DESCENT

This is THE core learning algorithm:

```
Repeat until convergence:
    1. Compute gradient: g = ∇L(w)
    2. Update weights:   w ← w - α × g
```

**Intuition — Rolling a Ball Downhill:**
```
Imagine the loss function as a hilly landscape:
- Your model's weights define your position on this landscape
- The height at any point = the loss value
- The gradient = the direction of the steepest uphill slope
- You take a step in the OPPOSITE direction (downhill)
- α controls how big each step is:
    - Too small: takes forever to reach the bottom
    - Too large: you overshoot and bounce around
    - Just right: smooth convergence to the minimum
```

### Worked Example — Gradient Descent on MSE

```
Model: ŷ = wx + b
Data: (1, 2), (2, 4), (3, 6)   ← clearly y = 2x

Initialize: w = 0, b = 0, α = 0.1

ITERATION 1:
  Predictions: ŷ = [0, 0, 0]
  Errors: ŷ-y = [-2, -4, -6]
  
  ∂L/∂w = (1/3) × [(-2)(1) + (-4)(2) + (-6)(3)] = (1/3)(-28) = -9.33
  ∂L/∂b = (1/3) × [(-2) + (-4) + (-6)] = -4.0

  w ← 0 - 0.1 × (-9.33) = 0.933
  b ← 0 - 0.1 × (-4.0)  = 0.4

ITERATION 2:
  ŷ = [0.933×1+0.4, 0.933×2+0.4, 0.933×3+0.4] = [1.333, 2.266, 3.199]
  Errors: [-0.667, -1.734, -2.801]
  
  ∂L/∂w = (1/3) × [(-0.667)(1) + (-1.734)(2) + (-2.801)(3)] = -4.18
  ∂L/∂b = (1/3) × [-0.667 + (-1.734) + (-2.801)] = -1.734

  w ← 0.933 - 0.1 × (-4.18) = 1.351
  b ← 0.4 - 0.1 × (-1.734) = 0.573

... (continues converging toward w = 2, b = 0)
```

---

## 2.5 The Chain Rule

### Definition
```
If y = f(g(x)), then:

dy/dx = (dy/du) × (du/dx)    where u = g(x)

Or equivalently:
dy/dx = f'(g(x)) × g'(x)
```

### Multi-variable Chain Rule
```
If L depends on ŷ, which depends on z, which depends on w:

∂L/∂w = (∂L/∂ŷ) × (∂ŷ/∂z) × (∂z/∂w)
```

> [!IMPORTANT]
> **The chain rule IS backpropagation.** It's the mathematical reason neural networks can learn. Without it, we couldn't compute gradients for weights in early layers.

### Complete Backprop Example — 2-Layer Network

```
NETWORK ARCHITECTURE:
Input:    x (scalar, for simplicity)
Hidden:   h = σ(w₁x + b₁)
Output:   ŷ = w₂h + b₂
Loss:     L = ½(ŷ - y)²

FORWARD PASS (with numbers):
x = 2, y = 1 (true label)
w₁ = 0.5, b₁ = 0.1, w₂ = 0.8, b₂ = 0.2

z₁ = w₁x + b₁ = 0.5(2) + 0.1 = 1.1
h  = σ(1.1) = 1/(1+e⁻¹·¹) = 0.7503
ŷ  = w₂h + b₂ = 0.8(0.7503) + 0.2 = 0.8002
L  = ½(0.8002 - 1)² = ½(0.0399) = 0.01996

BACKWARD PASS (chain rule, step by step):

Step 1: ∂L/∂ŷ = ŷ - y = 0.8002 - 1 = -0.1998

Step 2: ∂L/∂w₂ = (∂L/∂ŷ)(∂ŷ/∂w₂) = (-0.1998)(h) = (-0.1998)(0.7503) = -0.1499
         ∂L/∂b₂ = (∂L/∂ŷ)(∂ŷ/∂b₂) = (-0.1998)(1) = -0.1998

Step 3: ∂L/∂h = (∂L/∂ŷ)(∂ŷ/∂h) = (-0.1998)(w₂) = (-0.1998)(0.8) = -0.1599

Step 4: ∂L/∂z₁ = (∂L/∂h)(∂h/∂z₁) = (-0.1599)(σ'(z₁)) = (-0.1599)(0.7503)(1-0.7503)
                = (-0.1599)(0.1874) = -0.02997

Step 5: ∂L/∂w₁ = (∂L/∂z₁)(∂z₁/∂w₁) = (-0.02997)(x) = (-0.02997)(2) = -0.05994
         ∂L/∂b₁ = (∂L/∂z₁)(∂z₁/∂b₁) = (-0.02997)(1) = -0.02997

UPDATE (α = 0.1):
w₂ ← 0.8 - 0.1(-0.1499) = 0.815        ← w₂ increases (prediction was too low)
b₂ ← 0.2 - 0.1(-0.1998) = 0.220
w₁ ← 0.5 - 0.1(-0.05994) = 0.506
b₁ ← 0.1 - 0.1(-0.02997) = 0.103

Notice: ∂L/∂w₁ is MUCH smaller than ∂L/∂w₂ because the gradient had to pass through
σ'(z₁) ≈ 0.19, which shrunk it. This is the VANISHING GRADIENT PROBLEM!
```

---

## 2.6 Jacobian Matrix

### Definition
The Jacobian generalizes the gradient to **vector-valued functions**:

```
If f: ℝⁿ → ℝᵐ (input is n-dimensional, output is m-dimensional)

J = | ∂f₁/∂x₁  ∂f₁/∂x₂  ...  ∂f₁/∂xₙ |
    | ∂f₂/∂x₁  ∂f₂/∂x₂  ...  ∂f₂/∂xₙ |
    |   ...       ...      ...    ...     |
    | ∂fₘ/∂x₁  ∂fₘ/∂x₂  ...  ∂fₘ/∂xₙ |
```

Jᵢⱼ = how much does output i change when input j changes?

### Where Used in ML
| Application | How |
|---|---|
| **Backpropagation (general form)** | Gradient flows backward through each layer's Jacobian: `∂L/∂x = Jᵀ × ∂L/∂y` |
| **RNN stability** | The Jacobian ∂hₜ/∂hₜ₋₁ controls gradient flow through time. If its eigenvalues > 1 → exploding gradients. If < 1 → vanishing gradients. |
| **Softmax derivative** | The Jacobian of softmax is the full n×n matrix of ∂softmax(zᵢ)/∂zⱼ |

---

## 2.7 Hessian Matrix

### Definition
The Hessian is the matrix of **second-order partial derivatives**:

```
H = | ∂²f/∂x₁²      ∂²f/∂x₁∂x₂  ...  ∂²f/∂x₁∂xₙ |
    | ∂²f/∂x₂∂x₁    ∂²f/∂x₂²     ...  ∂²f/∂x₂∂xₙ |
    |   ...            ...          ...     ...        |
    | ∂²f/∂xₙ∂x₁    ∂²f/∂xₙ∂x₂  ...  ∂²f/∂xₙ²    |
```

### What It Tells You
- The Hessian captures the **curvature** of the loss function
- Eigenvalues of H tell you about the shape of the loss surface:
  - All positive → **local minimum** (bowl-shaped)
  - All negative → **local maximum** (dome-shaped)
  - Mixed → **saddle point** (saddle-shaped)

### Where Used in ML
| Application | How |
|---|---|
| **Newton's Method** | `w ← w - H⁻¹∇L` — uses curvature to take optimal steps (converges much faster than gradient descent) |
| **L-BFGS optimizer** | Approximates H⁻¹ cheaply for logistic regression and other convex problems |
| **Loss landscape analysis** | Eigenvalues of H reveal if the network is at a minimum, saddle point, or plateau |
| **Fisher Information Matrix** | Related to the expected Hessian; used in natural gradient descent |

---

## 2.8 Integral Calculus

### Definition
```
∫ f(x) dx = F(x) + C     (indefinite integral — antiderivative)

∫ₐᵇ f(x) dx = F(b) - F(a)   (definite integral — area under curve)
```

### Key Integrals

| Function | Integral |
|---|---|
| xⁿ | xⁿ⁺¹/(n+1) |
| eˣ | eˣ |
| 1/x | ln\|x\| |
| e⁻ˣ² | √π (Gaussian integral, for limits -∞ to ∞) |

### Where Used in ML

| Application | How |
|---|---|
| **Probability distributions** | Total probability must integrate to 1: ∫ p(x) dx = 1. This determines normalization constants. |
| **Expected value** | E[X] = ∫ x × p(x) dx — the average outcome weighted by probability |
| **Marginal probability** | P(X) = ∫ P(X,Y) dY — sum out (integrate over) a variable to remove it |
| **Bayesian inference** | P(θ\|data) = P(data\|θ)P(θ) / ∫P(data\|θ)P(θ)dθ — the denominator is often intractable, leading to approximations like MCMC |
| **KL Divergence** | KL(P‖Q) = ∫ p(x) log(p(x)/q(x)) dx — measures how different two distributions are |
| **AUC-ROC** | Area Under the ROC Curve = ∫ TPR d(FPR) — computed by numerical integration |

---

# Chapter 3: Probability Theory

Probability is how ML handles **uncertainty** — noisy data, unseen examples, and imperfect models.

---

## 3.1 Axioms of Probability

```
1. P(A) ≥ 0                    (probabilities are non-negative)
2. P(Ω) = 1                    (something must happen)
3. P(A ∪ B) = P(A) + P(B)      if A and B are mutually exclusive
```

From these, everything else follows:
```
P(not A) = 1 - P(A)
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)     (inclusion-exclusion)
0 ≤ P(A) ≤ 1
```

---

## 3.2 Conditional Probability

### Definition
```
P(A|B) = P(A ∩ B) / P(B)

"The probability of A happening, given that B has already happened"
```

### Worked Example
```
Email classification:
P(spam) = 0.3                          ← 30% of emails are spam
P(contains "free" | spam) = 0.8        ← 80% of spam contains "free"
P(contains "free" | not spam) = 0.1    ← 10% of ham contains "free"

P(contains "free") = P(free|spam)P(spam) + P(free|not spam)P(not spam)
                   = 0.8×0.3 + 0.1×0.7 = 0.24 + 0.07 = 0.31
```

### Where Used in ML
| Application | How |
|---|---|
| **All classification** | We're computing P(class \| features) — "what's the probability of each class, given these features?" |
| **Language models** | P(next_word \| previous_words) — GPT predicts the next token conditioned on all previous tokens |
| **Hidden Markov Models** | P(hidden_state \| observations) |

---

## 3.3 Bayes' Theorem

### Definition
```
P(A|B) = P(B|A) × P(A) / P(B)

Or in ML terms:

P(hypothesis | data) = P(data | hypothesis) × P(hypothesis) / P(data)
     posterior         =     likelihood       ×     prior     / evidence
```

### Deep Intuition

| Term | Meaning | Example |
|---|---|---|
| **Prior** P(A) | What you believed before seeing data | 30% of emails are spam |
| **Likelihood** P(B\|A) | How likely the data is IF hypothesis is true | If it IS spam, 80% chance it says "free" |
| **Evidence** P(B) | How likely the data is overall | 31% of all emails say "free" |
| **Posterior** P(A\|B) | Updated belief after seeing data | Given it says "free", what's the spam probability? |

### Worked Example — Spam Classifier

```
P(spam | "free") = P("free" | spam) × P(spam) / P("free")
                 = 0.8 × 0.3 / 0.31
                 = 0.24 / 0.31
                 = 0.774

→ An email containing "free" has a 77.4% chance of being spam!
```

### Where Used in ML

| Application | How |
|---|---|
| **Naive Bayes Classifier** | P(class\|features) ∝ P(class) × Π P(featureᵢ\|class). Assumes features are conditionally independent (the "naive" part). Despite this simplification, works well for text classification. |
| **Bayesian Neural Networks** | Instead of learning single weight values, learn **distributions** over weights. P(w\|data) = P(data\|w)P(w)/P(data). Gives uncertainty estimates for free! |
| **MAP Estimation** | Find w that maximizes P(w\|data) = maximize [log P(data\|w) + log P(w)]. The log P(w) term IS regularization! Gaussian prior → L2 regularization. Laplacian prior → L1 regularization. |
| **Bayesian Optimization** | Used for hyperparameter tuning. Models the objective function as a Gaussian process and uses Bayes' theorem to update beliefs about which hyperparameters are best. |
| **A/B Testing** | Bayesian A/B testing: P(variant A is better \| data) using posterior distributions |

> [!TIP]
> **Regularization is secretly Bayesian!**
> - L2 regularization = assuming a Gaussian prior on weights (most weights should be near zero)
> - L1 regularization = assuming a Laplacian prior (many weights should be exactly zero)

---

## 3.4 Probability Distributions

### Discrete Distributions

**Bernoulli Distribution** — Single coin flip:
```
P(X = 1) = p
P(X = 0) = 1 - p

Mean: E[X] = p
Variance: Var(X) = p(1-p)
```
> **Used in:** Binary classification output, dropout (each neuron independently kept/dropped)

**Binomial Distribution** — n coin flips:
```
P(X = k) = C(n,k) × pᵏ × (1-p)ⁿ⁻ᵏ

Mean: np
Variance: np(1-p)
```
> **Used in:** Modeling the number of successes in n trials, accuracy distribution

**Categorical Distribution** — Die roll with k faces:
```
P(X = i) = pᵢ    where Σ pᵢ = 1
```
> **Used in:** Multi-class classification output, language model token prediction

**Poisson Distribution** — Count of rare events:
```
P(X = k) = (λᵏ × e⁻λ) / k!

Mean: λ
Variance: λ
```
> **Used in:** Modeling count data (number of clicks, events per time period)

---

### Continuous Distributions

**Uniform Distribution** — Equal probability everywhere:
```
f(x) = 1/(b-a)    for a ≤ x ≤ b

Mean: (a+b)/2
Variance: (b-a)²/12
```
> **Used in:** Random weight initialization (some schemes), random data splitting

**Gaussian (Normal) Distribution** — The most important distribution in ML:
```
f(x) = (1/√(2πσ²)) × exp(-(x-μ)² / (2σ²))

Mean: μ
Variance: σ²

Standard Normal: μ=0, σ=1 → f(x) = (1/√(2π)) × exp(-x²/2)
```

> **Why is the Gaussian so fundamental?**
> 1. **Central Limit Theorem**: Sum of many independent random variables → Gaussian (regardless of their individual distributions)
> 2. **Maximum entropy**: Among all distributions with a known mean and variance, the Gaussian has maximum entropy (makes fewest assumptions)
> 3. **Mathematical convenience**: Closed-form solutions for many operations

> **Used in:**

| ML Application | How the Gaussian is Used |
|---|---|
| **Weight Initialization** | Weights ~ N(0, 1/√n). Xavier/He initialization uses specific variances to keep signal magnitude stable. |
| **Gaussian Noise** | Adding noise ~ N(0, σ²) to data for augmentation or to gradients for privacy (differential privacy). |
| **Gaussian Naive Bayes** | Assumes each feature follows a Gaussian: P(xᵢ\|class) = N(μ_class, σ²_class). |
| **Gaussian Mixture Models** | Data is modeled as a mixture of K Gaussians: P(x) = Σ πₖ N(x; μₖ, Σₖ). |
| **Variational Autoencoders** | The latent space is forced to follow N(0, I). The "reparameterization trick": z = μ + σ ⊙ ε, where ε ~ N(0,1). |
| **Bayesian Linear Regression** | Prior on weights: w ~ N(0, σ²I). Posterior is also Gaussian (conjugate prior). |
| **Gaussian Processes** | A distribution over FUNCTIONS. Any finite subset of function values follows a multivariate Gaussian. |

**Multivariate Gaussian** — Multiple correlated variables:
```
f(x) = (1/((2π)^(n/2) |Σ|^(1/2))) × exp(-½ (x-μ)ᵀ Σ⁻¹ (x-μ))

Where:
  x, μ ∈ ℝⁿ          (n-dimensional vectors)
  Σ ∈ ℝⁿˣⁿ           (covariance matrix — captures correlations)
  |Σ| = det(Σ)
  Σ⁻¹ = precision matrix
```

> The term `(x-μ)ᵀ Σ⁻¹ (x-μ)` is the **Mahalanobis distance** — it measures distance accounting for correlations and scale. This is why Gaussian-based classifiers work well even when features have different scales.

---

## 3.5 Expected Value and Variance

### Expected Value
```
Discrete:    E[X] = Σ xᵢ P(xᵢ)
Continuous:  E[X] = ∫ x f(x) dx

Properties:
E[aX + b] = aE[X] + b     (linearity)
E[X + Y] = E[X] + E[Y]     (always true, even if dependent!)
E[XY] = E[X]E[Y]           (only if X,Y are independent)
```

### Variance
```
Var(X) = E[(X - E[X])²] = E[X²] - (E[X])²

Properties:
Var(aX + b) = a² Var(X)          (constants scale quadratically)
Var(X + Y) = Var(X) + Var(Y)      (if independent)
```

### Where Used in ML

| Concept | Application |
|---|---|
| **E[X]** | The expected loss over the data distribution is what we're truly trying to minimize. Training loss is an empirical estimate of this. |
| **Var(X)** | High variance in model predictions → overfitting. Random Forest reduces variance by averaging predictions: Var(avg) = Var(single)/n |
| **E[∇L]** | Stochastic gradient descent: the expected value of the mini-batch gradient equals the true gradient: E[∇L_batch] = ∇L_full |
| **Var(∇L)** | SGD noise: larger batches → lower gradient variance → smoother convergence |
| **Bias-Variance** | E[error] = Bias² + Variance + Noise. The fundamental ML tradeoff. |

---

## 3.6 Independence and Conditional Independence

### Independence
```
X and Y are independent if:
P(X, Y) = P(X) × P(Y)

Equivalently: P(X|Y) = P(X)    (knowing Y tells you nothing about X)
```

### Conditional Independence
```
X and Y are conditionally independent given Z if:
P(X, Y | Z) = P(X|Z) × P(Y|Z)

"Once you know Z, X and Y become independent"
```

### Where Used in ML
| Application | How |
|---|---|
| **Naive Bayes** | Assumes features are conditionally independent given the class: P(x₁,x₂,...\|class) = Π P(xᵢ\|class). This is the "naive" assumption. |
| **Graphical Models** | Bayesian networks encode conditional independence assumptions as a directed graph |
| **i.i.d. assumption** | Training samples are assumed **independent and identically distributed** — the foundational assumption of most ML algorithms |
| **Dropout** | Each neuron is independently dropped with probability p |

---

## 3.7 Information Theory

### Entropy — Measuring Uncertainty
```
H(X) = -Σ P(xᵢ) log₂ P(xᵢ)

"The average amount of surprise/information in a random variable"
```

| Distribution | Entropy | Intuition |
|---|---|---|
| Always heads: P=[1, 0] | H = 0 | No surprise, no information |
| Fair coin: P=[0.5, 0.5] | H = 1 bit | Maximum surprise for 2 outcomes |
| Biased: P=[0.9, 0.1] | H = 0.47 bits | Somewhat predictable |
| 4 equal outcomes | H = 2 bits | Need 2 bits to encode |

### Cross-Entropy — Comparing Distributions
```
H(P, Q) = -Σ P(xᵢ) log Q(xᵢ)

"The average surprise when using distribution Q to encode data that actually follows P"
```

> **This is THE loss function for classification!**

```
Binary Cross-Entropy (logistic regression):
L = -[y log(ŷ) + (1-y) log(1-ŷ)]

Here P = true distribution (y), Q = predicted distribution (ŷ)
```

```
Categorical Cross-Entropy (multi-class):
L = -Σ yₖ log(ŷₖ)

Since y is one-hot, this simplifies to: L = -log(ŷ_correct_class)
```

> **Why cross-entropy and not MSE for classification?**
> 
> MSE has a **flat gradient** when predictions are very wrong (sigmoid saturates). Cross-entropy has a **steep gradient** when wrong → faster learning!
> ```
> MSE gradient:    (ŷ-y) × σ'(z)    ← σ' ≈ 0 when z is large → tiny gradient
> CE gradient:     (ŷ-y)            ← no σ' term → always strong gradient
> ```

### KL Divergence — Distance Between Distributions
```
KL(P ‖ Q) = Σ P(xᵢ) log(P(xᵢ)/Q(xᵢ))

         = H(P, Q) - H(P)

"How much extra surprise you get from using Q instead of the true P"
```

Properties:
- KL(P‖Q) ≥ 0 (always non-negative)
- KL(P‖Q) = 0 iff P = Q
- KL(P‖Q) ≠ KL(Q‖P) (not symmetric!)

### Where Used in ML

| Application | How |
|---|---|
| **VAE Loss** | ELBO = Reconstruction_loss - KL(q(z\|x) ‖ p(z)). The KL term forces the learned latent distribution q to be close to the prior p (usually N(0,1)). |
| **Knowledge Distillation** | Student network minimizes KL divergence from teacher's soft predictions |
| **Policy Gradient (RL)** | PPO clips the KL divergence between old and new policies to prevent too-large updates |
| **Decision Trees** | Information Gain = H(parent) - weighted H(children). Splits that maximize information gain reduce entropy the most. |
| **Language Models** | Perplexity = 2^H(P,Q) — measures how well a language model predicts text. Lower = better. |

---

## 3.8 Joint, Marginal, and Conditional Distributions

```
Joint:       P(X, Y)              — probability of X AND Y together
Marginal:    P(X) = Σᵧ P(X, Y)   — "sum out" Y to get P(X) alone
Conditional: P(X|Y) = P(X,Y)/P(Y) — probability of X given Y

These three are related by:
P(X, Y) = P(X|Y) × P(Y) = P(Y|X) × P(X)
```

### Where Used in ML
| Application | How |
|---|---|
| **Generative models** | Learn the joint P(X,Y) and use it to generate new samples or classify: P(Y\|X) = P(X,Y)/P(X) |
| **Marginalization** | In Bayesian inference, compute P(prediction) = ∫ P(prediction\|θ) P(θ\|data) dθ — average over all possible parameter values |
| **EM Algorithm** | E-step: compute P(latent \| observed, θ). M-step: maximize expected joint log-likelihood |
| **Latent variable models** | VAE models P(x) = ∫ P(x\|z) P(z) dz where z is the latent variable |

---

## 3.9 The Law of Large Numbers & Central Limit Theorem

### Law of Large Numbers
```
As n → ∞:  (1/n) Σ xᵢ → E[X]

"The sample mean converges to the true mean"
```

> **In ML:** This is why training on more data gives better estimates. Empirical risk (training loss) converges to true risk (expected loss) as dataset size → ∞.

### Central Limit Theorem (CLT)
```
If X₁, X₂, ..., Xₙ are i.i.d. with mean μ and variance σ²:

√n × (X̄ - μ) / σ  →  N(0, 1)    as n → ∞

"The sample mean is approximately Gaussian for large n, regardless of the underlying distribution"
```

> **In ML:**
> - Why mini-batch gradient estimates work: the average gradient over a batch is approximately Gaussian
> - Justifies Gaussian assumptions in many models
> - Explains why ensemble methods (averaging many weak learners) work so well

---

# Chapter 4: Statistics

Statistics connects **data** to **mathematical models**.

---

## 4.1 Descriptive Statistics

### Measures of Central Tendency

```
Mean:       μ = (1/n) Σ xᵢ           ← sensitive to outliers
Median:     middle value when sorted   ← robust to outliers
Mode:       most frequent value
```

**In ML:** Data preprocessing. Understanding feature distributions before modeling. Median is used in robust statistics and outlier detection.

### Measures of Spread

```
Variance:            σ² = (1/n) Σ (xᵢ - μ)²
Standard Deviation:  σ = √(σ²)
Range:               max - min
IQR:                 Q3 - Q1 (interquartile range)
```

**In ML:**
- **Feature normalization**: z = (x - μ)/σ → standardizes features to mean=0, std=1. Critical for gradient-based optimization (unscaled features cause elongated loss surfaces → slow convergence).
- **Batch Normalization**: Normalizes layer activations using batch mean and variance, dramatically improving training stability and speed.

---

## 4.2 Covariance and Correlation

### Covariance
```
Cov(X, Y) = (1/n) Σ (xᵢ - μₓ)(yᵢ - μᵧ) = E[XY] - E[X]E[Y]

Cov > 0: X and Y increase together
Cov < 0: X increases, Y decreases
Cov = 0: no linear relationship
```

### Covariance Matrix
```
For features X₁, X₂, ..., Xₙ:

Σ = | Var(X₁)      Cov(X₁,X₂)  ...  Cov(X₁,Xₙ) |
    | Cov(X₂,X₁)   Var(X₂)     ...  Cov(X₂,Xₙ) |
    |  ...            ...        ...     ...       |
    | Cov(Xₙ,X₁)   Cov(Xₙ,X₂) ...  Var(Xₙ)     |
```

Properties: Σ is **symmetric** and **positive semi-definite**.

### Correlation
```
r(X, Y) = Cov(X, Y) / (σₓ × σᵧ)

r ∈ [-1, 1]
r = 1:  perfect positive linear relationship
r = -1: perfect negative linear relationship
r = 0:  no linear relationship (but could be nonlinear!)
```

### Where Used in ML

| Application | How |
|---|---|
| **PCA** | Eigendecompose the covariance matrix to find directions of maximum variance |
| **Feature selection** | Highly correlated features are redundant — keep one, drop the other |
| **Multicollinearity detection** | If features are highly correlated, linear regression becomes unstable (XᵀX becomes nearly singular) |
| **Gaussian models** | The covariance matrix Σ parameterizes the multivariate Gaussian — encodes all pairwise feature relationships |
| **Mahalanobis distance** | d = √((x-μ)ᵀ Σ⁻¹ (x-μ)) — accounts for correlations, unlike Euclidean distance |

---

## 4.3 Maximum Likelihood Estimation (MLE)

### Idea
Find the parameters θ that **maximize the probability of observing the data**:

```
θ̂_MLE = argmax_θ P(data | θ)
       = argmax_θ Π P(xᵢ | θ)     (assuming i.i.d. data)
       = argmax_θ Σ log P(xᵢ | θ)  (log-likelihood — easier to optimize)
```

### Example 1 — Deriving MSE Loss

```
Assume: y = wᵀx + ε, where ε ~ N(0, σ²)   (noise is Gaussian)

Then: P(yᵢ | xᵢ, w) = N(yᵢ; wᵀxᵢ, σ²) = (1/√(2πσ²)) exp(-(yᵢ - wᵀxᵢ)² / (2σ²))

Log-likelihood:
ℓ(w) = Σ log P(yᵢ | xᵢ, w)
     = Σ [-½ log(2πσ²) - (yᵢ - wᵀxᵢ)² / (2σ²)]

Maximize ℓ(w) ↔ Minimize Σ (yᵢ - wᵀxᵢ)²     ← THIS IS MSE!
```

> **MLE with Gaussian noise assumption → MSE loss!** This is WHY we use MSE for regression.

### Example 2 — Deriving Cross-Entropy Loss

```
Assume: P(yᵢ=1 | xᵢ) = σ(wᵀxᵢ)    (Bernoulli distribution with sigmoid)

P(yᵢ | xᵢ, w) = ŷᵢ^yᵢ × (1-ŷᵢ)^(1-yᵢ)

Log-likelihood:
ℓ(w) = Σ [yᵢ log(ŷᵢ) + (1-yᵢ) log(1-ŷᵢ)]

Maximize ℓ(w) ↔ Minimize -ℓ(w) = -Σ [y log(ŷ) + (1-y) log(1-ŷ)]    ← CROSS-ENTROPY!
```

> **MLE with Bernoulli assumption → Cross-Entropy loss!** This is WHY we use cross-entropy for classification.

---

## 4.4 Maximum A Posteriori (MAP) Estimation

### Idea
MLE can overfit with limited data. MAP adds a **prior** on parameters:

```
θ̂_MAP = argmax_θ P(θ | data)
       = argmax_θ P(data | θ) × P(θ)           (Bayes' theorem, P(data) is constant)
       = argmax_θ [log P(data | θ) + log P(θ)]
       =          [  log-likelihood  + prior  ]
```

### Connecting MAP to Regularization

**Gaussian prior → L2 regularization:**
```
P(w) = N(0, τ²I) → log P(w) = -‖w‖₂² / (2τ²) + const

MAP objective: Σ log P(yᵢ|xᵢ,w)  -  (1/2τ²) ‖w‖₂²
             =     likelihood      -   λ ‖w‖₂²
             
Where λ = 1/(2τ²)
```

**Laplacian prior → L1 regularization:**
```
P(w) = Laplace(0, b) → log P(w) = -‖w‖₁ / b + const

MAP objective: Σ log P(yᵢ|xᵢ,w)  -  (1/b) ‖w‖₁
             =     likelihood      -   λ ‖w‖₁
```

> [!IMPORTANT]
> **This is a profound connection:**
> - MLE = no regularization (pure likelihood)
> - MAP with Gaussian prior = L2 regularization (Ridge)
> - MAP with Laplacian prior = L1 regularization (Lasso)
> - Full Bayesian = integrate over ALL possible parameter values (most robust, most expensive)

---

## 4.5 Bias-Variance Decomposition

### The Fundamental Theorem of ML Generalization

For any estimator f̂ trying to predict f:

```
E[(y - f̂(x))²] = Bias²(f̂) + Var(f̂) + σ²

Where:
  Bias(f̂) = E[f̂(x)] - f(x)           ← how far off is the average prediction?
  Var(f̂)  = E[(f̂(x) - E[f̂(x)])²]    ← how much do predictions vary?
  σ²      = irreducible noise           ← can never be reduced
```

### Mathematical Proof Sketch
```
E[(y - f̂)²] = E[(y - f + f - E[f̂] + E[f̂] - f̂)²]

Expanding and using E[ε] = 0:
= E[(f - E[f̂])²] + E[(E[f̂] - f̂)²] + E[ε²]
=     Bias²        +     Variance     +  Noise
```

### Where Used in ML

| Scenario | Bias | Variance | Fix |
|---|---|---|---|
| **Simple model (few features, low capacity)** | High | Low | Underfitting → add features, use more complex model |
| **Complex model (many features, high capacity)** | Low | High | Overfitting → add regularization, get more data, use dropout |
| **Random Forest** | Same as single tree | Reduced by 1/n | Bagging reduces variance! |
| **Boosting** | Reduced each iteration | May increase | Sequential trees reduce bias! |

---

## 4.6 Hypothesis Testing (for Model Evaluation)

### Key Concepts

```
Null Hypothesis (H₀): "There is no difference / no effect"
Alternative (H₁):     "There IS a difference / effect"

p-value: P(data at least this extreme | H₀ is true)
  - p < 0.05 → reject H₀ (result is "statistically significant")
  - p ≥ 0.05 → cannot reject H₀

Type I Error (False Positive):  Reject H₀ when it's true   (rate = α)
Type II Error (False Negative): Fail to reject H₀ when H₁ is true (rate = β)
Power = 1 - β = probability of correctly detecting a real effect
```

### Where Used in ML

| Application | How |
|---|---|
| **A/B Testing** | Is model B significantly better than model A? Use t-test on accuracy/metrics across cross-validation folds. |
| **Feature Importance** | Is a feature statistically significantly related to the target? Use F-test, chi-squared test, or permutation test. |
| **Model Comparison** | McNemar's test: do two classifiers make significantly different errors? |
| **Confidence Intervals** | 95% CI for model accuracy: p̂ ± 1.96 × √(p̂(1-p̂)/n). Gives a range of plausible values. |

---

## 4.7 Cross-Validation

### The Mathematical Foundation

We want to estimate the true generalization error E_true, but we only have finite data.

```
k-Fold Cross-Validation:

1. Split data into k equal folds
2. For i = 1 to k:
     - Train on all folds except fold i
     - Test on fold i → get error eᵢ
3. CV estimate = (1/k) Σ eᵢ

Variance of estimate = (1/k) Σ (eᵢ - ē)² / (k-1)
```

| k value | Bias | Variance | Cost |
|---|---|---|---|
| k = 2 | High (half the data for training) | Low | Low |
| k = 5 | Medium | Medium | Medium |
| k = 10 | Low | Medium-High | High |
| k = n (LOOCV) | Very Low (almost all data) | High | Very High |

> Used to select hyperparameters (learning rate, regularization strength, model complexity) by estimating how each setting will perform on unseen data.

---

# Quick Reference — Where Each Math Concept Powers ML

| Math Concept | ML Algorithm / Technique |
|---|---|
| Dot product | Linear/logistic regression, neurons, attention, cosine similarity |
| Matrix multiply | Neural network layers, batch processing, PCA |
| Eigenvalues | PCA, spectral clustering, PageRank, stability analysis |
| SVD | Recommender systems, LSA, image compression |
| Derivatives | Gradient descent (all models) |
| Chain rule | Backpropagation (neural networks) |
| Hessian | Newton's method, L-BFGS, loss landscape analysis |
| Bayes' theorem | Naive Bayes, Bayesian networks, MAP, hyperparameter tuning |
| Gaussian distribution | Weight init, noise, GMM, VAE, GP, Bayesian regression |
| Cross-entropy | Classification loss (logistic regression, softmax) |
| KL divergence | VAE, knowledge distillation, PPO (RL) |
| Entropy | Decision trees (information gain), feature selection |
| MLE | Derives MSE loss, cross-entropy loss, EM algorithm |
| MAP | Regularization (L1/L2), Bayesian learning |
| Variance | Bias-variance tradeoff, ensemble methods, batch normalization |
| Covariance matrix | PCA, multivariate Gaussian, Mahalanobis distance |
| Norms (L1/L2) | Regularization, distance metrics, SVM margin |

---

> [!TIP]
> **Next steps:** If you want, I can create **interactive Python notebooks** that implement each concept from scratch — so you can see the math running on real data. Just let me know which topics to start with!

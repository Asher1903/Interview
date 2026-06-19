# Every ML Term Explained — The Complete Dictionary

> [!NOTE]
> Every term includes: **Definition → Intuitive Analogy → The Math → Practical Tips**. Terms are grouped logically so related concepts are near each other.

---

# 1. The Core Training Loop

Understanding these terms means understanding how a model **learns**.

---

## 1.1 Training

### What It Is
The process of feeding data to a model and adjusting its internal parameters (weights) so that the model's predictions get closer to the correct answers.

### Analogy
**Studying for an exam.** You look at practice questions (training data), check the answer key (labels), see where you were wrong (loss), and adjust your understanding (update weights). You repeat this many times until you get good.

### What Actually Happens (Step by Step)
```
1. Feed input data → Model makes a prediction (FORWARD PASS)
2. Compare prediction to true answer → Compute loss (ERROR MEASUREMENT)
3. Calculate how to adjust each weight to reduce the loss (BACKWARD PASS / BACKPROP)
4. Adjust the weights slightly (PARAMETER UPDATE)
5. Repeat steps 1-4 for all the data
6. Repeat the whole thing many times (EPOCHS)
```

---

## 1.2 Learning Rate (α)

### What It Is
A small positive number (typically 0.001 to 0.01) that controls **how big of a step** you take when updating weights during gradient descent.

### The Math
```
w_new = w_old - α × gradient

         ↑         ↑        ↑
    new weight   learning  direction & magnitude
                  rate     of change needed
```

### Analogy
**You're blindfolded on a hill, trying to reach the lowest point.**
- The gradient tells you which direction is downhill
- The **learning rate** is your **step size**:
  - **Too large (α = 1.0):** You take huge steps, overshoot the valley, bounce back and forth, maybe fly off the hill entirely → **divergence** (loss goes to infinity)
  - **Too small (α = 0.00001):** You take tiny baby steps → you'll eventually get there, but it takes **forever** → extremely slow training
  - **Just right (α = 0.001):** Smooth, efficient descent to the bottom → **convergence**

### Visual Intuition
```
Loss
 ↑
 |  ×                          
 |    \     Too large → bounces       
 |     ×  ×                     
 |      ×    ×  ×               
 |           ×                  
 |  × ─ ─ × ─ ─ ×──── Just right → smooth descent
 |              ×
 |        ×...×...×...×...  Too small → barely moves
 +──────────────────────→ Iterations
```

### Practical Values
| Learning Rate | Typical Use |
|---|---|
| 0.1 – 1.0 | Too high for most cases |
| **0.001 – 0.01** | **Good starting point** |
| 0.0001 | Fine-tuning pretrained models |
| 0.00001 | Very delicate adjustments |

### Pro Tips
- **Start with 0.001** (the default in Adam optimizer)
- If loss explodes → learning rate too high
- If loss barely changes → learning rate too low or model issue
- Use **learning rate schedulers** to decrease it over time (start fast, slow down near the optimum)

---

## 1.3 Epoch

### What It Is
**One complete pass through the entire training dataset.**

If you have 10,000 images and you show ALL 10,000 to the model once, that's 1 epoch.

### Analogy
**Reading a textbook cover to cover once = 1 epoch.** To really learn the material, you might read it 50-100 times (50-100 epochs).

### How Many Epochs?
```
Too few epochs:  Model hasn't learned enough → underfitting
Too many epochs: Model memorizes training data → overfitting
Just right:      Use EARLY STOPPING (stop when validation loss stops improving)
```

### The Math
```
Total weight updates = epochs × (dataset_size / batch_size)

Example:
  50,000 training images
  batch_size = 64
  epochs = 10

  Updates per epoch = 50,000 / 64 ≈ 781
  Total updates = 10 × 781 = 7,810 gradient descent steps
```

### Typical Values
| Task | Typical Epochs |
|---|---|
| Simple tabular data | 10 – 100 |
| Image classification | 50 – 200 |
| Fine-tuning pretrained model | 3 – 10 |
| Language model pretraining | 1 – 3 (datasets are huge) |

---

## 1.4 Batch Size

### What It Is
The number of training samples processed **before performing one weight update**.

### Why Not Use All Data at Once?
```
Full dataset (Batch GD):     Stable but SLOW (one update per epoch)
One sample (Stochastic GD):  Fast but NOISY (one update per sample)
Mini-batch (Mini-batch GD):  BEST OF BOTH (one update per batch)
```

### Analogy
**Imagine grading student essays to improve your rubric:**
- **Batch size = ALL (10,000 essays):** Read every single essay before updating your rubric. Very thorough, but painfully slow.
- **Batch size = 1:** Update your rubric after EVERY essay. Very reactive, but your rubric changes wildly.
- **Batch size = 32:** Read 32 essays, then update. Good balance of speed and stability.

### The Math
```
For a batch of B samples:

Gradient estimate = (1/B) Σᵢ₌₁ᴮ ∇L(xᵢ, yᵢ, w)

As B increases:
  - Gradient estimate becomes more accurate (lower variance)
  - But each step is more expensive (more computation)
  - And you get fewer updates per epoch
```

### Practical Values
| Batch Size | Behavior |
|---|---|
| 1 | Maximum noise, most updates (SGD) |
| **16 – 64** | **Common sweet spot** |
| 128 – 256 | Smoother, needs more memory |
| 512+ | Requires special learning rate tuning |

### Pro Tips
- **Larger batch → increase learning rate** (linear scaling rule: double batch = double LR)
- Batch size is limited by **GPU memory** (larger images = smaller batch)
- Small batches add beneficial noise → can escape local minima → sometimes generalize **better**

---

## 1.5 Iteration (Step)

### What It Is
**One single weight update.** Process one batch → compute loss → update weights = 1 iteration.

### Relationship
```
1 epoch = dataset_size / batch_size iterations

Example:
  60,000 samples, batch_size = 32
  1 epoch = 60,000 / 32 = 1,875 iterations
```

---

## 1.6 Convergence

### What It Is
When the model's loss stops significantly decreasing — it has found a (local) minimum of the loss function.

### How to Know
```
Converged when:  |L(t) - L(t-1)| < ε   for some small threshold ε

In practice: watch the loss plot — when it flattens out, you've converged.
```

### Types
```
          Loss
           ↑
           |╲
           | ╲
Global     |  ╲    ╱╲
Minimum ── |   ╲  ╱  ╲     ╱╲
           |    ╲╱    ╲   ╱  ╲
           |   local   ╲ ╱    ╲
           |   min      ╲      ── global min
           +──────────────────────→
```
- **Global minimum:** The absolute lowest loss (ideal)
- **Local minimum:** A low point surrounded by higher values (gradient descent can get stuck here)
- **Saddle point:** A point that's a minimum in some directions and maximum in others (common in high dimensions)

> In deep learning, local minima are rarely a problem. Most local minima are nearly as good as the global minimum. Saddle points are the bigger issue — but SGD's noise helps escape them.

---

# 2. Model Components

What's actually inside a model.

---

## 2.1 Weights (w)

### What It Is
Numbers that the model **learns** during training. They determine **how much influence** each input feature has on the output.

### Analogy
**Recipe ingredients.** If you're making a prediction smoothie:
- Weight for "study hours" = 0.8 (important!)
- Weight for "shoe size" = 0.01 (not important)
- The model learns which ingredients matter and how much of each to use

### The Math
```
For a single neuron:
output = w₁x₁ + w₂x₂ + w₃x₃ + ... + wₙxₙ + b

The wᵢ values are the weights. Larger |wᵢ| = feature xᵢ matters more.
```

### Weight Initialization
Weights must be initialized carefully — not too large, not too small:

| Method | Formula | When to Use |
|---|---|---|
| **Xavier/Glorot** | w ~ N(0, 2/(n_in + n_out)) | Sigmoid/Tanh activations |
| **He** | w ~ N(0, 2/n_in) | **ReLU activations (most common)** |
| **Zero** | w = 0 | **NEVER for weights** (all neurons learn the same thing — "symmetry breaking" problem) |

---

## 2.2 Bias (b)

### What It Is
An extra learnable number added to the weighted sum. It lets the model **shift the activation function left or right**.

### Analogy
**The y-intercept of a line.** In `y = mx + b`:
- Without bias: the line must pass through the origin → very restrictive
- With bias: the line can be anywhere → much more flexible

### The Math
```
z = w₁x₁ + w₂x₂ + b
         ↑              ↑
    weighted sum     shift/offset

Without bias: decision boundary MUST pass through origin
With bias:    decision boundary can be ANYWHERE
```

### Visual
```
Without bias (b=0):          With bias (b=2):
y                            y
↑        /                   ↑           /
|       /                    |          /
|      /                     |         /
|     /                      |        /
|    /                       |       /
+───/────→ x                 +──────/──→ x
   must pass through 0            shifted up by 2
```

---

## 2.3 Parameters vs Hyperparameters

### Parameters
Numbers the model learns **automatically** during training.

```
Examples: weights (w), biases (b)

A neural network with 2 layers of 128 neurons:
  Layer 1: 784×128 weights + 128 biases = 100,480 parameters
  Layer 2: 128×10 weights + 10 biases   = 1,290 parameters
  Total: 101,770 parameters
```

### Hyperparameters
Numbers **YOU set before training** — the model can't learn these by itself.

```
Examples:
  - Learning rate (α = 0.001)
  - Batch size (32)
  - Number of epochs (50)
  - Number of layers (3)
  - Number of neurons per layer (128)
  - Regularization strength (λ = 0.01)
  - Dropout rate (0.5)
  - Activation function (ReLU)
```

### Analogy
- **Parameters** = the answers on a test (the student figures these out)
- **Hyperparameters** = the study strategy (how many hours to study, which textbook to use — the teacher decides)

### How to Tune Hyperparameters
| Method | How It Works |
|---|---|
| **Grid Search** | Try all combinations from a predefined set |
| **Random Search** | Try random combinations (often better than grid!) |
| **Bayesian Optimization** | Use past results to smartly pick next combination |
| **Learning Rate Finder** | Gradually increase LR, find where loss drops fastest |

---

## 2.4 Activation Function

### What It Is
A function applied **after** the weighted sum in each neuron. It introduces **nonlinearity** — without it, a neural network is just a fancy linear regression.

### Why Nonlinearity Is Essential

```
Without activation functions:
  Layer 1: z₁ = W₁x + b₁
  Layer 2: z₂ = W₂z₁ + b₂ = W₂(W₁x + b₁) + b₂ = (W₂W₁)x + (W₂b₁ + b₂) = W'x + b'
  
  → Multiple layers COLLAPSE into one linear transformation!
  → No matter how many layers, it can only learn linear patterns.
  → Can't learn XOR, can't classify circles, can't recognize images.

With activation functions:
  Layer 1: h₁ = ReLU(W₁x + b₁)
  Layer 2: h₂ = ReLU(W₂h₁ + b₂)
  
  → Each layer adds nonlinearity
  → Can approximate ANY function (Universal Approximation Theorem)
```

### The Activation Function Zoo

#### ReLU (Rectified Linear Unit) — The Default Choice
```
ReLU(z) = max(0, z) = { z  if z > 0
                       { 0  if z ≤ 0

Derivative: { 1  if z > 0
            { 0  if z ≤ 0
```
```
Output
  ↑        /
  |       /
  |      /
  |     /
  |    /
──+───/────→ Input
  |  (flat at 0 for negative inputs)
```

**Pros:** Fast, simple, no vanishing gradient for positive values
**Cons:** "Dead neurons" — if z is always negative, gradient is always 0, neuron never updates
**Used in:** Hidden layers of almost all modern networks

#### Leaky ReLU — Fixes Dead Neurons
```
LeakyReLU(z) = { z       if z > 0
               { 0.01z   if z ≤ 0
```
**Pros:** No dead neurons (small gradient for negative values)
**Used in:** When you encounter dead neuron problems

#### Sigmoid — Squashes to [0, 1]
```
σ(z) = 1 / (1 + e⁻ᶻ)
```
```
Output
1 ─ ─ ─ ─ ─ ── ─ ─────
              ╱
             ╱
0.5 ─ ─ ─ ╱
          ╱
         ╱
0 ─────── ─ ─ ─ ─ ─ ─
```
**Pros:** Output is a probability (0 to 1)
**Cons:** Vanishing gradient (σ'(z) max = 0.25), outputs not zero-centered
**Used in:** **Output layer for binary classification ONLY**. Avoid in hidden layers.

#### Softmax — Probabilities Over Multiple Classes
```
softmax(zᵢ) = eᶻⁱ / Σⱼ eᶻʲ

All outputs sum to 1 → probability distribution over classes
```

```
Input logits: [2.0, 1.0, 0.5]

e²·⁰ = 7.39,  e¹·⁰ = 2.72,  e⁰·⁵ = 1.65
Sum = 11.76

softmax = [7.39/11.76, 2.72/11.76, 1.65/11.76]
        = [0.63, 0.23, 0.14]    ← sums to 1.0 ✓

→ Class 1: 63% probability (winner)
```

**Used in:** **Output layer for multi-class classification** (e.g., digit recognition: 10 classes)

---

# 3. Loss, Cost, and Optimization

How the model knows it's wrong and how it gets better.

---

## 3.1 Loss Function (Objective Function)

### What It Is
A function that measures **how wrong** the model's prediction is compared to the true answer. **The entire goal of training is to minimize this number.**

### Analogy
**Your exam score — but inverted.** Loss = 0 means perfect. Loss = 100 means terrible. Training = studying to bring your score down.

### Common Loss Functions

#### MSE (Mean Squared Error) — For Regression
```
L = (1/m) Σ (ŷᵢ - yᵢ)²

Example:
  True prices:      [200K, 300K, 250K]
  Predicted prices: [210K, 280K, 260K]
  Errors:           [10K, -20K, 10K]
  Squared errors:   [100M, 400M, 100M]
  MSE = (100M + 400M + 100M) / 3 = 200M
```
**Why squared?**
1. Makes all errors positive (no cancellation)
2. Penalizes large errors much more than small ones (10² = 100, but 2² = 4)
3. Smooth and differentiable everywhere (nice for gradient descent)

**Downside:** Very sensitive to outliers (one huge error dominates)

#### MAE (Mean Absolute Error) — Robust Regression
```
L = (1/m) Σ |ŷᵢ - yᵢ|
```
**More robust to outliers** than MSE, but not differentiable at 0.

#### Binary Cross-Entropy — For Binary Classification
```
L = -(1/m) Σ [yᵢ log(ŷᵢ) + (1-yᵢ) log(1-ŷᵢ)]
```
```
When true label y = 1:
  L = -log(ŷ)
  If ŷ = 0.99 → L = -log(0.99) = 0.01   ← low loss, correct! ✓
  If ŷ = 0.01 → L = -log(0.01) = 4.6    ← HIGH loss, wrong! ✗

When true label y = 0:
  L = -log(1-ŷ)
  If ŷ = 0.01 → L = -log(0.99) = 0.01   ← low loss, correct! ✓
  If ŷ = 0.99 → L = -log(0.01) = 4.6    ← HIGH loss, wrong! ✗
```

#### Categorical Cross-Entropy — For Multi-Class
```
L = -Σₖ yₖ log(ŷₖ)

Since y is one-hot [0, 0, 1, 0, ...]:
L = -log(ŷ_correct_class)

If model gives 90% to correct class: L = -log(0.9) = 0.105   ← good
If model gives 10% to correct class: L = -log(0.1) = 2.303   ← bad
```

---

## 3.2 Loss vs Cost vs Objective

People use these interchangeably, but technically:

| Term | Definition |
|---|---|
| **Loss** | Error on a **single** training example: L(ŷᵢ, yᵢ) |
| **Cost** | Average loss over the **entire** training set: J = (1/m) Σ L(ŷᵢ, yᵢ) |
| **Objective** | Cost + any regularization: J_total = J + λR(w) |

---

## 3.3 Gradient Descent (The Learning Algorithm)

### What It Is
The algorithm that **minimizes the loss** by repeatedly moving the weights in the direction that decreases the loss.

### The Algorithm
```
Initialize weights randomly
Repeat until convergence:
    1. Compute predictions:  ŷ = model(X)
    2. Compute loss:         L = loss(ŷ, y)
    3. Compute gradients:    g = ∂L/∂w    (backpropagation)
    4. Update weights:       w = w - α × g
```

### Analogy — Expanded
**You're blindfolded on a hilly landscape (the loss surface). You want to reach the lowest valley.**

1. You feel the ground around you with your feet (compute gradient — the slope)
2. You figure out which direction goes downhill the steepest (negative gradient)
3. You take a step in that direction (update weights)
4. Step size = learning rate
5. Repeat until the ground feels flat (convergence — gradient ≈ 0)

### Variants

| Variant | Uses | Updates Per Epoch | Trade-off |
|---|---|---|---|
| **Batch GD** | ALL data each step | 1 | Accurate gradient, very slow |
| **Stochastic GD (SGD)** | 1 random sample | m (dataset size) | Noisy but fast, good regularizer |
| **Mini-batch GD** | B samples (batch) | m/B | **Best balance — universally used** |

---

## 3.4 Optimizers

Smarter versions of gradient descent.

### SGD with Momentum
**Problem with plain SGD:** Oscillates in steep directions, slow in flat directions.
**Solution:** Add "velocity" — remember previous gradients.

```
vₜ = β × vₜ₋₁ + (1-β) × ∇L     ← velocity accumulates past gradients
w  = w - α × vₜ                  ← update using velocity, not raw gradient

β = 0.9 typically (90% momentum from past, 10% from current gradient)
```

**Analogy:** A ball rolling downhill gains momentum. It doesn't stop immediately when the slope changes — it keeps rolling in the general direction, smoothing out bumps.

### Adam (Adaptive Moment Estimation) — THE Default Optimizer

**Combines momentum + per-parameter learning rates.**

```
mₜ = β₁ × mₜ₋₁ + (1-β₁) × gₜ           ← 1st moment (running average of gradient)
vₜ = β₂ × vₜ₋₁ + (1-β₂) × gₜ²          ← 2nd moment (running average of gradient²)
m̂ₜ = mₜ / (1 - β₁ᵗ)                     ← bias-corrected mean
v̂ₜ = vₜ / (1 - β₂ᵗ)                     ← bias-corrected variance
wₜ = wₜ₋₁ - α × m̂ₜ / (√v̂ₜ + ε)         ← update

Default values: β₁=0.9, β₂=0.999, ε=10⁻⁸, α=0.001
```

**Why Adam is great:**
- Parameters with large gradients get smaller effective learning rates (prevents overshooting)
- Parameters with small gradients get larger effective learning rates (speeds up in flat regions)
- Momentum smooths out noise
- Works well with almost no tuning

> [!TIP]
> **When in doubt, use Adam with learning rate 0.001.** It's the best starting point for almost any problem.

---

## 3.5 Learning Rate Schedule

### What It Is
A strategy to **change the learning rate during training** — typically decreasing it over time.

### Why?
```
Early training:  Far from minimum → big steps are good → high LR
Late training:   Near minimum → big steps overshoot → need small LR
```

### Common Schedules

| Schedule | Formula | Behavior |
|---|---|---|
| **Step Decay** | α = α₀ × 0.1 every 30 epochs | Sudden drops |
| **Exponential Decay** | α = α₀ × e⁻ᵏᵗ | Smooth decrease |
| **Cosine Annealing** | α = α_min + ½(α₀-α_min)(1+cos(πt/T)) | Smooth, popular in vision |
| **Warmup + Decay** | Linear increase for N steps, then decay | Used in Transformers |
| **ReduceOnPlateau** | Halve α when validation loss stops improving | Adaptive, practical |

---

# 4. Data Concepts

Understanding how data is structured and prepared.

---

## 4.1 Features (Inputs / Independent Variables)

### What It Is
The **input attributes** the model uses to make predictions. Each feature is one measurable property.

```
Predicting house prices:
Features = [area_sqft, num_bedrooms, num_bathrooms, age_years, distance_to_city]
             1500         3              2              10           5.2

Each feature is one column in your dataset.
Each sample (house) is one row.
```

### Feature Types
| Type | Examples | How to Handle |
|---|---|---|
| **Numerical (continuous)** | Height, temperature, price | Use directly (may need scaling) |
| **Numerical (discrete)** | Number of rooms, age in years | Use directly |
| **Categorical (nominal)** | Color, country, brand | One-hot encoding |
| **Categorical (ordinal)** | Rating (low/med/high), education level | Label encoding or ordinal encoding |
| **Text** | Reviews, emails | Tokenization → embeddings |
| **Image** | Photos | Pixel values → CNN features |

---

## 4.2 Labels (Targets / Dependent Variable)

### What It Is
The **correct answer** the model is trying to predict. Only available in **supervised learning**.

```
Regression label:  house_price = 350,000  (continuous number)
Classification label: is_spam = 1          (discrete category)
Multi-class label: digit = 7               (one of many categories)
```

---

## 4.3 Samples / Examples / Data Points

### What It Is
A **single row** in your dataset = one observation with its features and (optionally) label.

```
Sample #42: [1500 sqft, 3 beds, 2 baths, 10 years, 5.2 km] → $350,000
              ────────────── features ──────────────────        ── label ──
```

---

## 4.4 Training Set / Validation Set / Test Set

### What They Are and Why You Need All Three

| Set | Purpose | Typical % | Analogy |
|---|---|---|---|
| **Training Set** | Model learns from this data | 70-80% | Practice problems |
| **Validation Set** | Tune hyperparameters, monitor overfitting | 10-15% | Mock exam (you can take it multiple times) |
| **Test Set** | Final evaluation — used ONCE | 10-15% | Final exam (you only take it once) |

### Why Not Just Train and Test?

```
Workflow WITHOUT validation set (WRONG):
  Train → Test → Adjust hyperparameters → Test again → Adjust → Test again
  
  Problem: You're indirectly "training" on the test set by tuning to it!
  Your test score is now optimistic — won't reflect real performance.

Workflow WITH validation set (CORRECT):
  Train → Validate → Adjust hyperparameters → Validate → ... → satisfied
  → Test ONCE → this is your true performance estimate
```

> [!CAUTION]
> **Never use the test set for any decisions during training.** If you peek at test performance and adjust anything, your test score is no longer a reliable estimate of real-world performance.

---

## 4.5 Feature Scaling / Normalization

### Why It's Needed
Features on different scales cause problems:

```
Feature 1: Age (0-100)
Feature 2: Income ($20,000 - $500,000)

Without scaling, the gradient for Income dominates because its values are huge.
The optimizer takes zigzag paths instead of a direct route to the minimum.
```

### Methods

#### Standardization (Z-score Normalization)
```
x_scaled = (x - μ) / σ

Result: mean = 0, standard deviation = 1

Example:
Ages = [20, 30, 40, 50, 60],  μ = 40,  σ = 14.14
Scaled = [-1.41, -0.71, 0, 0.71, 1.41]
```

**When to use:** Most algorithms (neural networks, SVM, logistic regression). Best when data is roughly Gaussian.

#### Min-Max Normalization
```
x_scaled = (x - x_min) / (x_max - x_min)

Result: all values in [0, 1]

Example:
Ages = [20, 30, 40, 50, 60]
Scaled = [0, 0.25, 0.5, 0.75, 1.0]
```

**When to use:** When you need values in a specific range (e.g., pixel values for images, output of sigmoid).

> [!IMPORTANT]
> **Fit scaling on TRAINING data only.** Then apply the SAME μ, σ (or min, max) to validation and test sets. If you fit on the whole dataset, you're leaking information from test into training.

---

## 4.6 One-Hot Encoding

### What It Is
Converting categorical labels into binary vectors.

```
Categories: [cat, dog, bird]

cat  → [1, 0, 0]
dog  → [0, 1, 0]
bird → [0, 0, 1]
```

### Why Not Just Use Numbers (0, 1, 2)?
```
If cat=0, dog=1, bird=2:
  The model thinks dog is "between" cat and bird
  The model thinks bird is "twice" dog
  These relationships are MEANINGLESS → model learns nonsense patterns
```

One-hot encoding treats each category as equally different.

**Used in:**
- Multi-class classification labels (target encoding for cross-entropy loss)
- Categorical feature encoding for models that need numerical input

---

## 4.7 Embedding

### What It Is
A **dense, learned vector representation** of a categorical item (word, user, product) in a continuous space.

```
One-hot (sparse, high-dimensional):
  "king"  → [0, 0, 0, 1, 0, 0, ..., 0]     (10,000-dimensional, mostly zeros)

Embedding (dense, low-dimensional):
  "king"  → [0.21, -0.45, 0.89, 0.12, ...]   (e.g., 300-dimensional, all meaningful)
```

### Why Better Than One-Hot?
```
One-hot: Every word is equally different. "king" and "queen" are as different as "king" and "banana".

Embedding: Captures semantic meaning!
  king - man + woman ≈ queen     (famous Word2Vec result)
  paris - france + italy ≈ rome
```

### Where Used
| Application | What Gets Embedded |
|---|---|
| **Word2Vec, GloVe** | Words → vectors |
| **Transformers (BERT, GPT)** | Tokens → vectors |
| **Recommender Systems** | Users & items → vectors |
| **Graph Neural Networks** | Nodes → vectors |

---

# 5. Overfitting, Underfitting & Regularization

The most important practical concepts.

---

## 5.1 Overfitting

### What It Is
The model **memorizes** the training data instead of learning general patterns. It performs great on training data but poorly on new, unseen data.

### Analogy
**Memorizing exam answers word-for-word instead of understanding the concepts.** You ace the practice test, but fail the real exam because the questions are phrased differently.

### How to Detect
```
Training accuracy: 99%  ← model has memorized training data
Validation accuracy: 72% ← but can't generalize to new data

Gap between training and validation performance = OVERFITTING
```

### Causes
- Model too complex (too many parameters relative to data)
- Too few training samples
- Training for too many epochs
- No regularization

### Fixes
| Fix | How It Helps |
|---|---|
| More training data | More examples → harder to memorize, must learn patterns |
| Regularization (L1/L2) | Penalizes large weights → simpler model |
| Dropout | Randomly disables neurons → prevents co-adaptation |
| Early stopping | Stop before the model starts memorizing |
| Data augmentation | Create modified copies of data → artificially more data |
| Reduce model size | Fewer layers/neurons → less capacity to memorize |

---

## 5.2 Underfitting

### What It Is
The model is **too simple** to capture the patterns in the data. Performs poorly on BOTH training and validation data.

### Analogy
**Trying to predict house prices using only the color of the front door.** The model simply doesn't have the right features or enough capacity.

### How to Detect
```
Training accuracy: 55%    ← can't even learn training data
Validation accuracy: 52%  ← also bad on validation

Both are bad = UNDERFITTING
```

### Fixes
| Fix | How It Helps |
|---|---|
| More features | Give the model more information |
| More complex model | More layers, more neurons → more capacity |
| Train longer | Maybe it hasn't finished learning yet |
| Reduce regularization | Too much regularization prevents learning |
| Better features | Feature engineering, domain knowledge |

---

## 5.3 Regularization

### What It Is
**Any technique that prevents overfitting** by constraining the model's complexity.

### L2 Regularization (Ridge / Weight Decay)
```
L_total = L_original + (λ/2m) Σ wⱼ²

Gradient update becomes:
wⱼ ← wⱼ(1 - α×λ/m) - α × ∂L/∂wⱼ
            ↑
      "weight decay" — weights shrink by a factor each step
```

**Effect:** Pushes all weights toward zero (but never exactly zero). Prevents any single feature from having too much influence.

### L1 Regularization (Lasso)
```
L_total = L_original + (λ/m) Σ |wⱼ|
```

**Effect:** Pushes some weights to **exactly zero** → automatic feature selection. The model learns to ignore irrelevant features.

### Dropout

```
During training:
  Randomly set each neuron's output to 0 with probability p (typically p=0.5)
  Scale remaining neurons by 1/(1-p) to keep expected values the same

During inference:
  Use ALL neurons (no dropout)
```

**Analogy:** Like training a football team where random players sit out each practice. Every player must learn to be useful without relying on specific teammates. The team becomes more robust.

**Why it works (mathematically):**
- Forces redundancy — information can't flow through a single path
- Approximates training an ensemble of 2ⁿ different sub-networks (where n = number of neurons)
- Acts as a regularizer by adding noise to training

### Early Stopping

```
Monitor validation loss during training:

Epoch  Train Loss  Val Loss
  1      2.5        2.4
  5      1.2        1.1
  10     0.5        0.6    ← val loss starts increasing!
  15     0.2        0.9    ← overfitting confirmed
  20     0.1        1.3    ← getting worse

→ STOP at epoch 5 (where val loss was lowest)
→ Restore weights from epoch 5
```

### Batch Normalization

```
For each mini-batch during training:
  1. Compute batch mean:      μ_B = (1/B) Σ xᵢ
  2. Compute batch variance:  σ²_B = (1/B) Σ (xᵢ - μ_B)²
  3. Normalize:               x̂ᵢ = (xᵢ - μ_B) / √(σ²_B + ε)
  4. Scale and shift:         yᵢ = γ × x̂ᵢ + β     (γ and β are learned!)
```

**Why it works:**
- Keeps activations in a reasonable range throughout the network
- Reduces "internal covariate shift" (each layer's input distribution keeps changing)
- Allows higher learning rates → faster training
- Acts as a regularizer (batch statistics add noise)

### Data Augmentation

Create modified versions of training data to artificially increase dataset size.

```
For images:
  - Random rotation (±15°)
  - Random flip (horizontal)
  - Random crop and resize
  - Color jitter (brightness, contrast, saturation)
  - Random erasing (cutout)

For text:
  - Synonym replacement
  - Random insertion/deletion
  - Back-translation (English → French → English)

For audio:
  - Time stretching
  - Pitch shifting
  - Adding background noise
```

---

# 6. Model Evaluation

How to measure if your model is actually good.

---

## 6.1 Accuracy

```
Accuracy = (Correct Predictions) / (Total Predictions)

Example: 90 correct out of 100 → 90% accuracy
```

> [!WARNING]
> **Accuracy is misleading with imbalanced data!**
> If 99% of emails are not spam, a model that ALWAYS predicts "not spam" has 99% accuracy but catches 0% of spam. Useless!

---

## 6.2 Confusion Matrix

```
                    Predicted
                 Positive  Negative
Actual Positive [  TP    |   FN   ]
Actual Negative [  FP    |   TN   ]

TP = True Positive   (correctly predicted positive)
TN = True Negative   (correctly predicted negative)
FP = False Positive  (predicted positive, actually negative) — "false alarm"
FN = False Negative  (predicted negative, actually positive) — "missed detection"
```

### Example — Cancer Detection
```
                    Predicted
                 Cancer   No Cancer
Actual Cancer   [  85   |    15    ]    ← 15 cancers MISSED (dangerous!)
Actual No Cancer[  10   |   890    ]    ← 10 false alarms (annoying but ok)

TP=85, FN=15, FP=10, TN=890
```

---

## 6.3 Precision, Recall, F1 Score

### Precision — "Of all positive predictions, how many were correct?"
```
Precision = TP / (TP + FP) = 85 / (85 + 10) = 89.5%

"When the model says cancer, it's right 89.5% of the time"
```
**Optimize for precision when:** False positives are costly (spam filter — annoying to lose real emails)

### Recall (Sensitivity) — "Of all actual positives, how many did we catch?"
```
Recall = TP / (TP + FN) = 85 / (85 + 15) = 85%

"The model catches 85% of actual cancer cases"
```
**Optimize for recall when:** False negatives are costly (cancer detection — missing a cancer patient is dangerous)

### F1 Score — Harmonic Mean of Precision and Recall
```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
   = 2 × (0.895 × 0.85) / (0.895 + 0.85)
   = 87.2%
```
**Use F1 when:** You need to balance both precision and recall, especially with imbalanced classes.

### The Precision-Recall Trade-off
```
Threshold ↑ (more strict positive predictions):
  → Precision ↑ (fewer false positives)
  → Recall ↓ (more false negatives — miss more real positives)

Threshold ↓ (more lenient positive predictions):
  → Precision ↓ (more false positives)
  → Recall ↑ (catch more real positives)
```

---

## 6.4 AUC-ROC

### ROC Curve
Plot of **True Positive Rate** vs **False Positive Rate** at every classification threshold.

```
TPR (Recall) = TP / (TP + FN)     ← "of all positives, how many caught?"
FPR          = FP / (FP + TN)     ← "of all negatives, how many false alarms?"
```

### AUC (Area Under the ROC Curve)
```
AUC = 1.0  → Perfect classifier
AUC = 0.5  → Random guessing (useless)
AUC = 0.0  → Perfectly wrong (just flip the predictions!)

AUC = 0.8-0.9 → Good
AUC > 0.9     → Excellent
```

**AUC measures how well the model ranks positive examples higher than negative examples, regardless of the threshold.**

---

## 6.5 Cross-Validation (k-Fold)

### What It Is
A method to get a **reliable** estimate of model performance by using ALL data for both training and validation.

```
5-Fold Cross-Validation:

Fold 1: [TEST | train | train | train | train]  → score₁
Fold 2: [train | TEST | train | train | train]  → score₂
Fold 3: [train | train | TEST | train | train]  → score₃
Fold 4: [train | train | train | TEST | train]  → score₄
Fold 5: [train | train | train | train | TEST]  → score₅

Final score = average(score₁, score₂, score₃, score₄, score₅)
Uncertainty = std(scores)
```

**Why better than a single train/test split:**
- Uses all data for training (important when data is limited)
- Uses all data for testing (every sample gets tested exactly once)
- Gives confidence interval, not just a single number

---

# 7. Neural Network Architecture Terms

---

## 7.1 Layer

### What It Is
A group of neurons that processes input and passes output to the next group.

```
Input Layer → Hidden Layer 1 → Hidden Layer 2 → Output Layer
   (raw        (learned          (higher-level     (final
   features)    features)         features)       prediction)
```

| Layer Type | What It Does |
|---|---|
| **Dense (Fully Connected)** | Every neuron connects to every neuron in the next layer. `z = Wx + b` |
| **Convolutional (Conv)** | Slides a small filter across the input, detecting local patterns (edges, textures) |
| **Recurrent (RNN/LSTM)** | Processes sequences step by step, maintaining a hidden state (memory) |
| **Attention** | Each element can attend to all other elements, learning relationships |
| **Pooling** | Reduces spatial dimensions (e.g., max pooling takes the maximum in each region) |
| **Normalization** | Batch norm, layer norm — stabilizes and speeds up training |

---

## 7.2 Depth vs Width

```
DEEP network (many layers):       WIDE network (many neurons per layer):
Input                             Input
  ↓                                 ↓
[10 neurons]                     [1000 neurons]
  ↓                                 ↓
[10 neurons]                     [1000 neurons]
  ↓                                 ↓
[10 neurons]                     Output
  ↓
[10 neurons]
  ↓
Output

Depth = number of layers          Width = number of neurons per layer
```

**Depth** lets the network learn **hierarchical features** (edges → shapes → objects → scenes). This is why "deep" learning works so well.

**Width** lets the network learn **more features at each level** but doesn't compose them hierarchically.

> In practice, **deeper is usually better than wider** for the same total parameter count.

---

## 7.3 Forward Pass vs Backward Pass

```
FORWARD PASS (Inference):
  Input → Layer 1 → Layer 2 → ... → Output → Loss
  "Making a prediction and computing the error"
  
  Direction: left → right (input to output)
  What happens: data flows through the network, producing a prediction

BACKWARD PASS (Backpropagation):
  Loss → ∂L/∂Wₙ → ∂L/∂Wₙ₋₁ → ... → ∂L/∂W₁
  "Computing how much each weight contributed to the error"
  
  Direction: right → left (output to input)
  What happens: gradients flow backward using the chain rule

PARAMETER UPDATE:
  w ← w - α × ∂L/∂w   for every weight in the network
```

---

## 7.4 Inference vs Training

| | Training | Inference |
|---|---|---|
| **Goal** | Learn the weights | Make predictions |
| **Data** | Training set with labels | New, unseen data |
| **Dropout** | Active (randomly dropping neurons) | Disabled (use all neurons) |
| **Batch Norm** | Uses batch statistics | Uses running average statistics |
| **Gradients** | Computed (backprop) | Not needed (faster) |
| **Speed** | Slower (need gradients, updates) | Faster (forward pass only) |

---

# 8. Advanced Concepts

---

## 8.1 Transfer Learning

### What It Is
Taking a model trained on **Task A** (e.g., ImageNet: 14M images, 1000 classes) and **reusing** its learned features for **Task B** (e.g., your 500 medical images, 2 classes).

```
Pretrained model (ImageNet):
  Layer 1: Detects edges          ← universal, useful for any image task
  Layer 2: Detects textures       ← universal
  Layer 3: Detects parts          ← somewhat specific
  Layer 4: Detects objects        ← specific to ImageNet

Transfer Learning strategy:
  1. Take layers 1-3 (freeze their weights — don't update)
  2. Replace layer 4 with a new layer for YOUR task
  3. Train only the new layer on YOUR data
```

**Why it works:** Early layers learn **universal features** (edges, textures) that are useful for any vision task. You get the benefit of training on millions of images without needing millions of images.

---

## 8.2 Fine-Tuning

### What It Is
A specific type of transfer learning where you **unfreeze some or all pretrained layers** and train them with a **very small learning rate**.

```
Step 1: Freeze all pretrained layers, train only the new head (3-5 epochs)
Step 2: Unfreeze all layers, train with very small LR (0.00001) for 3-10 epochs
```

**The small LR is critical** — you don't want to destroy the pretrained features, just gently adapt them to your task.

---

## 8.3 Latent Space

### What It Is
A compressed, learned representation of data in a lower-dimensional space where similar items are close together.

```
Original space:                     Latent space:
  Images are 28×28 = 784 pixels       Encoded to 2D or 32D vector
  Hard to compare                     Similar digits are nearby
  Mostly empty space                  Meaningful structure

  Image of "3" → [0.2, 0.8, -0.1, ...]   ← latent vector
  Image of "8" → [0.3, 0.7, -0.2, ...]   ← nearby (visually similar!)
  Image of "1" → [-0.9, 0.1, 0.5, ...]   ← far away (visually different)
```

**Used in:** Autoencoders, VAEs, GANs — you can generate new data by sampling from the latent space.

---

## 8.4 Generative vs Discriminative Models

| | Discriminative | Generative |
|---|---|---|
| **Learns** | P(y\|x) — boundary between classes | P(x,y) or P(x\|y) — full data distribution |
| **Goal** | Classify / predict | Generate new data |
| **Examples** | Logistic regression, SVM, neural net classifiers | GANs, VAEs, GPT, Naive Bayes, diffusion models |
| **Analogy** | A security guard who decides "allowed or not" | An artist who can create new faces from scratch |

---

## 8.5 Ensemble Methods

### What It Is
Combining multiple models to get better predictions than any single model.

```
Model 1 predicts: "cat" (80%)
Model 2 predicts: "cat" (70%)
Model 3 predicts: "dog" (60%)

Majority vote: "cat" wins (2 vs 1)
Average probability: cat = (0.8+0.7+0.4)/3 = 63%
```

### Types
| Method | How It Combines | Key Idea |
|---|---|---|
| **Bagging** (Random Forest) | Average independent models | Reduces **variance** |
| **Boosting** (XGBoost, AdaBoost) | Sequential models fix each other's mistakes | Reduces **bias** |
| **Stacking** | Use a meta-model to combine base models | Learns optimal combination |

---

# Quick Cheat Sheet — Term Relationships

```
┌─────────────────────────────────────────────────────────────┐
│                    THE ML TRAINING LOOP                      │
│                                                              │
│  Dataset ──split──→ Training Set   Validation Set  Test Set  │
│                          │              │             │       │
│                          ↓              │             │       │
│  ┌── for each EPOCH: ───────────┐      │             │       │
│  │  ┌── for each BATCH: ──────┐ │      │             │       │
│  │  │  1. FORWARD PASS        │ │      │             │       │
│  │  │     (compute prediction)│ │      │             │       │
│  │  │  2. LOSS FUNCTION       │ │      │             │       │
│  │  │     (measure error)     │ │      │             │       │
│  │  │  3. BACKWARD PASS       │ │      │             │       │
│  │  │     (compute gradients) │ │      │             │       │
│  │  │  4. OPTIMIZER step      │ │      │             │       │
│  │  │     (update WEIGHTS     │ │      │             │       │
│  │  │      using LEARNING     │ │      │             │       │
│  │  │      RATE)              │ │      │             │       │
│  │  │  = 1 ITERATION          │ │      │             │       │
│  │  └─────────────────────────┘ │      │             │       │
│  │  = 1 EPOCH                   │      │             │       │
│  └──────────────────────────────┘      │             │       │
│           │                             │             │       │
│           ↓                             ↓             │       │
│    Training Loss              Validation Loss         │       │
│    (getting better?)          (OVERFITTING?)          │       │
│           │                        │                  │       │
│           └──── EARLY STOPPING ────┘                  │       │
│                     │                                 │       │
│                     ↓                                 │       │
│              Final Model ────────────────→ Test Set   │       │
│                                            (ONCE!)    │       │
└─────────────────────────────────────────────────────────────┘
```

---

> [!TIP]
> **You now have all the vocabulary.** Whenever you read an ML paper, blog, or tutorial, every term should make sense. Refer back to this guide as a dictionary!

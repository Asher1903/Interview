# ML & AI Coursework: Beginner's Comprehensive Guide

This report breaks down the essential topics of academic coursework in **Machine Learning (ML)** and **Artificial Intelligence (AI)**. It translates abstract mathematical and programming concepts into simple, real-world analogies and practical examples, with definitions of complex terms.

---

## 🗺️ The AI & ML Family Tree

Before diving in, let's understand how these terms relate to each other:

1. **Artificial Intelligence (AI)**: The broadest category. Any machine that mimics human intelligence (e.g., chess-playing computers, rule-based systems).
2. **Machine Learning (ML)**: A subset of AI. Instead of writing rules manually, we feed data to the machine and let it learn the rules on its own.
3. **Deep Learning (DL)**: A subset of ML. It uses multi-layered structure (Neural Networks) to learn from complex data like images, audio, and text.
4. **Generative AI (GenAI)**: A subset of DL. Models trained to create *new* content (text, images, code) instead of just analyzing existing data.

---

## Module 1: Supervised Learning
> **The Analogy**: **Learning with a Teacher**. The teacher gives the student exam questions alongside the answer keys. The student studies the patterns between questions and answers so they can answer new questions in the final exam.

* **What it is**: Training a model on a dataset where the correct answers (**labels**) are already provided.

### A. Regression (Predicting a Continuous Number)
* **Concept**: Predicting a numeric, continuous value (like price, temperature, or size).
* **Example**: Predicting the price of a house based on its size (square feet).
* **Core Algorithm (Linear Regression)**: Finding the best straight line through a scatter plot of data points so we can predict the price of a house for any given size.
  * *Simple Math*: \(y = mx + c\) (where \(y\) is house price, \(x\) is size, \(m\) is the slope, and \(c\) is the intercept).
* **Complex Word - Continuous Value**: A number that can take any value within a range (e.g., 20.5 degrees, $350,230.12).

### B. Classification (Predicting a Category)
* **Concept**: Grouping data into specific buckets or categories.
* **Example**: Predicting whether an email is "Spam" or "Not Spam" based on its text.
* **Core Algorithms**:
  * **Logistic Regression**: Used for binary classification (yes/no, spam/not spam). Despite the word "regression" in its name, it is a classification tool! It calculates probability (0% to 100%) and uses a threshold (like 50%) to put items into categories.
  * **Decision Trees**: Making predictions by asking a series of yes/no questions (like a flow chart: *"Is the email from an unknown sender? Yes. Does it contain the word 'Free'? Yes. -> Spam"*).
  * **Random Forest**: An ensemble method that combines the answers of multiple Decision Trees to vote on the final category. It reduces errors because a crowd is smarter than a single individual.
* **Complex Word - Ensemble Method**: A machine learning technique that combines predictions from multiple individual models to make a more accurate, stable prediction.

---

## Module 2: Unsupervised Learning
> **The Analogy**: **Exploring an Unfamiliar Country**. You don't have a guide or map. You look around, see what things look similar, and group them together (e.g., grouping all beaches together and all mountains together) based on their features.

* **What it is**: Training a model on data *without* any labels (no correct answers provided). The model must find hidden patterns on its own.

### A. Clustering (Grouping Similar Data)
* **Concept**: Dividing data points into groups so that points in the same group are more similar to each other than to those in other groups.
* **Example**: Customer Segmentation. Grouping customers based on their buying habits (e.g., "bargain hunters", "impulsive buyers", "luxury shoppers") so a company can target them with custom advertisements.
* **Core Algorithm (K-Means Clustering)**: You choose the number of groups you want (let's say \(K=3\)). The algorithm randomly starts with 3 points as centers, calculates the distance of every customer to these centers, groups them, and recalculates the centers iteratively until the groups stop changing.

### B. Dimensionality Reduction (Simplifying the View)
* **Concept**: Reducing the number of variables (features) in your dataset while keeping the most important information.
* **Example**: If you are analyzing a car, instead of measuring 50 variables (length, width, height, wheel size, steering wheel diameter, etc.), you simplify it into a few core dimensions like "vehicle volume" and "performance".
* **Core Algorithm (PCA - Principal Component Analysis)**: A mathematical tool that squashes a dataset with many dimensions down to fewer dimensions while retaining as much variance (information) as possible.
* **Complex Word - Dimensionality**: The number of features or variables in a dataset. High-dimensionality means you have too many variables, which makes computation slow and hard to visualize.

---

## Module 3: Deep Learning & Neural Networks
> **The Analogy**: **The Human Brain**. A biological brain has interconnected cells called neurons. When you see a cat, light hits your eyes, triggers certain neurons, which trigger other neurons, until the final neuron fires: *"That's a cat!"*

* **What it is**: A subset of machine learning that models data using layered networks of nodes, imitating the way biological brains learn.

### Key Components:
1. **Neurons (Nodes)**: The basic processing units. They take numbers as inputs, multiply them by **weights** (importance factors), add a **bias**, and pass the result forward.
2. **Layers**:
   * **Input Layer**: Receives the raw data (e.g., pixel colors of an image).
   * **Hidden Layers**: Perform calculations to extract patterns (e.g., Layer 1 detects edges, Layer 2 detects shapes, Layer 3 detects faces).
   * **Output Layer**: Gives the final prediction (e.g., "Cat" or "Dog").
3. **Activation Function (The Gatekeeper)**: A mathematical function applied to a neuron's output to decide whether it should "fire" (send information to the next layer) and how strongly. Common functions:
   * **ReLU (Rectified Linear Unit)**: If the number is negative, turn it to 0. If positive, leave it as is. This is the most popular function because it makes computation very fast.
   * **Sigmoid**: Squashes any number into a range between 0 and 1, representing a probability.
4. **Backpropagation (Learning from Mistakes)**: The core training process.
   * **Step 1**: The network makes a guess (Forward Pass).
   * **Step 2**: We measure how wrong the guess was using a **Loss Function**.
   * **Step 3**: We calculate the error and send it *backward* through the layers (Backpropagation) to adjust the weights of the neurons using **Gradient Descent** so it makes a better guess next time.

---

## Module 4: Natural Language Processing (NLP) & GenAI
> **The Analogy**: **Translating Words into Maps**. A computer doesn't know what "king" or "queen" means. We represent them as coordinates on a map. If you start at the point for "king", subtract the direction of "man", and add the direction of "woman", you land right next to the point for "queen".

### A. Embeddings (Vector Representations)
* **Concept**: Converting words or sentences into a list of numbers (vectors) that capture their semantic meaning.
* **Why it matters**: It allows computers to mathematically compute relationships between words. Words with similar meanings end up close to each other in the vector space.

### B. Transformers & Attention Mechanisms
* **Concept**: The architecture behind modern LLMs (like Claude, GPT, or Llama).
* **Attention Mechanism**: Allows the model to look at a whole sentence and understand which words relate to each other, even if they are far apart.
  * *Example*: In the sentence *"The animal didn't cross the street because **it** was too tired"*, the model uses attention to connect **"it"** to **"animal"**. If the sentence was *"because **it** was too wide"*, it connects **"it"** to **"street"**.

---

## Module 5: Model Evaluation (Testing)
> **The Analogy**: **Grading a Student's Exam**. You don't just check if they passed; you check *why* they failed. Did they guess too much? Did they miss obvious things?

* **What it is**: Measuring how well a trained model performs on a separate **test dataset** (data it has never seen before).

### A. The Confusion Matrix (Classification Test)
A table used to evaluate classification performance:

| | Predicted Positive | Predicted Negative |
| :--- | :--- | :--- |
| **Actual Positive** | **True Positive (TP)** <br> (Correctly caught spam) | **False Negative (FN)** <br> (Spam slipped into inbox) |
| **Actual Negative** | **False Positive (FP)** <br> (Normal email marked as spam) | **True Negative (TN)** <br> (Correctly left normal email alone) |

### B. Key Evaluation Metrics:
* **Accuracy**: The percentage of overall correct predictions: \(\frac{TP + TN}{Total}\).
  * *Warning*: If you have a dataset of 99 normal transactions and 1 fraud transaction, a model that predicts "always normal" has 99% accuracy but is completely useless at detecting fraud!
* **Precision**: *"When the model predicts Positive, how often is it right?"* Formula: \(\frac{TP}{TP + FP}\).
  * *Use case*: High precision is crucial when a False Positive is very bad. (e.g., you don't want a spam filter to delete an important business email).
* **Recall (Sensitivity)**: *"Out of all actual Positives, how many did the model find?"* Formula: \(\frac{TP}{TP + FN}\).
  * *Use case*: High recall is crucial when a False Negative is catastrophic. (e.g., a cancer detection model must find every patient with cancer, even if it causes a few false alarms).
* **F1-Score**: The balance between Precision and Recall. It is the harmonic mean of the two.

---

## Module 6: Math Foundations (The Under-the-Hood Mechanics)

### A. Linear Algebra
* **Vectors & Matrices**: Data is structured in grids. A single image is a matrix of pixel values (e.g., \(28 \times 28\) pixels). Linear algebra gives us the rules to add, multiply, and manipulate these grids rapidly.

### B. Calculus
* **Derivatives & Slopes**: In calculus, a derivative calculates the rate of change or the slope of a curve.
* **Why it's used**: In ML, we want to slide down a "hill of errors" (minimize the loss function) to find the lowest point. The derivative tells us which direction is "downhill" so we can adjust our model parameters.

### C. Statistics
* **Mean, Median, Mode**: Essential to summarize datasets.
* **Standard Deviation**: Measures how spread out the numbers are. High standard deviation means the data is highly varied; low means it is clustered tightly around the average.
* **Correlation**: Measures the strength and direction of the linear relationship between two variables.




# Model Training, Testing, Evaluation, & Documentation: Interview Q&A

This guide is designed to prepare you for the **"Support model testing, evaluation, and documentation"** and **"Basic knowledge of ML algorithms and AI concepts"** responsibilities of the AWS AI Intern role. 

---

## 🚀 Part 1: Model Training & Tuning

### Q1: Why do we split datasets into Training, Validation, and Test sets? How is it done?
* **Answer**: "We split our data so we don't cheat by testing the model on the same data it studied.
  * **Training Set (e.g., 70%)**: The textbook the model studies. The model uses this to learn patterns and adjust its weights.
  * **Validation Set (e.g., 15%)**: The practice quizzes. We use this data to tune model settings (hyperparameters) and see if the model is learning well without letting it study the final exam questions.
  * **Test Set (e.g., 15%)**: The final exam. This data is kept in a 'vault' and only used at the very end to evaluate how the model performs on completely unseen data."

### Q2: What is "K-Fold Cross-Validation" and why do we use it?
* **Answer**: "Instead of splitting the data once, **K-Fold Cross-Validation** means we split the data into \(K\) equal parts (folds). We train the model \(K\) times. Each time, we use a different fold as the validation set and the remaining folds as the training set. Finally, we average the performance.
  * *Why we use it*: If we have a small dataset, a single split might accidentally put all the 'easy' examples in the training set and the 'hard' ones in the test set. K-Fold ensures every piece of data gets to be in both training and validation, giving us a much more accurate evaluation."

### Q3: What is "Data Leakage" and how can an intern prevent it?
* **Answer**: "Data Leakage happens when information from the future (test/validation set) accidentally leaks into the past (training set). This makes the model look amazingly accurate during testing, but it fails completely in production.
  * *Example*: If you calculate the average age of your customers using the *entire* dataset before splitting it, and use that average to fill in missing values, the training set now knows information about the test set.
  * *How to prevent it*: Always perform data preprocessing steps (like scaling, normalization, or filling missing values) **after** splitting the dataset, using statistics calculated *only* from the training set."

### Q4: What is "Hyperparameter Tuning"? What is the difference between Grid Search and Random Search?
* **Answer**: "Hyperparameters are the settings of the machine learning model that we must set *before* training starts (like how many trees in a Random Forest, or the learning rate of a neural network). They are like the knobs on a radio. Hyperparameter tuning is finding the best combination of these settings.
  * **Grid Search**: Trying every single possible combination of settings from a list you provide. It is thorough but very slow and expensive.
  * **Random Search**: Randomly picking combinations of settings. It is much faster and often finds a great combination in far fewer trials."
  * *AWS Tool Connection*: **Amazon SageMaker Automatic Model Tuning** automates this by using machine learning (Bayesian optimization) to find the best hyperparameters quickly.

---

## 📊 Part 2: Model Testing & Evaluation

### Q5: How do you evaluate a model if your dataset is highly imbalanced?
* **Answer**: "If you have a dataset where 99% of transactions are normal and 1% are fraud, a model that predicts 'always normal' is 99% accurate but completely useless. In imbalanced datasets, we **do not use Accuracy**. Instead, we use:
  * **Precision**: Out of all predicted fraud cases, how many were actually fraud?
  * **Recall**: Out of all actual fraud cases, how many did we catch?
  * **F1-Score**: The balance between the two.
  * **ROC-AUC (Receiver Operating Characteristic - Area Under Curve)**: A score between 0 and 1 that tells us how good the model is at separating the two classes. A score of 0.5 means random guessing; 1.0 means perfect separation."
* **Complex Word - Imbalanced Dataset**: A dataset where one category (class) has vastly more examples than the other category.

### Q6: Explain the "Bias-Variance Tradeoff" in simple terms.
* **Answer**: "This is the balance between underfitting and overfitting:
  * **High Bias (Underfitting)**: The model is too simple. It makes strong assumptions and fails to capture the patterns in the training data. (Like a student who only reads the summary of a book and fails both the practice quizzes and the exam).
  * **High Variance (Overfitting)**: The model is too complex. It memorizes the training data perfectly, including the noise/mistakes. (Like a student who memorizes the exact questions of a practice test but fails the real exam because the numbers changed).
  * **The Tradeoff**: As we make our model more complex to reduce Bias, Variance increases. Our goal is to find the sweet spot in the middle where both training and test errors are minimized."

### Q7: You built a RAG chatbot. How do you test and evaluate a Generative AI application?
* **Answer**: "Unlike traditional models that output simple numbers or categories, LLMs output paragraphs of text, making evaluation harder. We evaluate them in three ways:
  1. **Grounding (Faithfulness)**: We check if the LLM's answer is based *only* on the retrieved context documents, ensuring it didn't hallucinate.
  2. **Answer Relevance**: Does the generated answer actually address the user's question?
  3. **Evaluation Frameworks**: We can use open-source frameworks like **Ragas** or use a larger model (like Claude 3.5 Sonnet on Amazon Bedrock) to act as a grader (**LLM-as-a-judge**), scoring the outputs on a scale from 1 to 5 based on strict criteria."

---

## 📝 Part 3: Model Documentation & Governance

### Q8: What is a "Model Card" and why is it important in production AI?
* **Answer**: "A Model Card is like a nutrition label for a machine learning model. It is a short document that explains:
  * What the model is designed to do (intended use).
  * What data was used to train it.
  * How it was evaluated and its performance metrics.
  * Its limitations and potential biases (e.g., does it perform worse on certain demographics?).
  It is critical for transparency, ethical AI, and helping other developers understand how to use the model safely."
* **AWS Connection**: **Amazon SageMaker Model Cards** automates the creation of these documents, pulling training details, datasets, and evaluation metrics directly from SageMaker.

### Q9: How do you document a data preprocessing pipeline?
* **Answer**: "Data preprocessing is the cleaning phase. To document it properly, I write down:
  1. **Data Sources**: Where the raw data comes from.
  2. **Data Cleaning Steps**: How we handled missing values (e.g., did we drop them or replace them with the median?), removed duplicates, and corrected label errors.
  3. **Feature Engineering**: What new inputs we created (e.g., extracting 'Day of the Week' from a date).
  4. **Data Dictionary**: A list of all final columns and what they mean.
  This ensures that if the model needs to be retrained in the future, the data can be prepared in the exact same way."

### Q10: What is a Model Registry and why do we need it?
* **Answer**: "In a company, models are updated constantly. A **Model Registry** is a catalog or version control system for trained models (like Git, but for model files instead of code).
  * It stores different versions of a model (e.g., version 1.0, version 2.0).
  * It tracks the status of each model (e.g., 'Approved', 'Pending Review', 'Deployed to Staging').
  * It ensures that only approved models are promoted to production."
* **AWS Connection**: **Amazon SageMaker Model Registry**.

---

## 💡 Part 4: Practical Intern Scenario Questions

### Q11: "As an intern, you are asked to test a newly trained classification model. What steps do you take?"
* **Answer**: "I would follow a structured process:
  1. **Check the Split**: Ensure the test dataset has been kept separate and has no data leakage.
  2. **Run Predictions**: Pass the test data through the model to get predictions.
  3. **Generate a Confusion Matrix**: Visualize the True Positives, False Positives, True Negatives, and False Negatives.
  4. **Calculate Metrics**: Compute Accuracy, Precision, Recall, and F1-Score.
  5. **Analyze Errors**: Look at the instances where the model failed to see if there is a pattern (e.g., is the model failing on a specific sub-category?).
  6. **Document Results**: Create an evaluation report summarizing these findings."

### Q12: "If you find that your model performs exceptionally well on the training data (e.g., 99% accuracy) but poorly on the test data (e.g., 60% accuracy), what is happening and how do you fix it?"
* **Answer**: "The model is **overfitting** (memorizing the training data). To fix it, I would propose:
  1. **Simplifying the Model**: Using a simpler algorithm, reducing the depth of decision trees, or reducing the number of parameters.
  2. **Regularization**: Adding mathematical penalties to prevent the model from becoming too complex.
  3. **Getting More Data**: More training examples help the model generalize rather than memorize.
  4. **Applying Dropout**: If using neural networks, randomly disabling neurons during training so they don't rely too heavily on specific pathways."
 


# ML Foundational Topics: Beginner's Deep-Dive Guide

This report covers all core **Machine Learning foundational topics**, including supervised algorithms, unsupervised algorithms, optimization techniques, and the underlying mathematical mechanics. Each topic is explained using simple real-world analogies, examples, and key interview terms.

---

## 📈 Section 1: Supervised ML Algorithms

### 1. Linear Regression
* **Analogy**: Drawing a straight line through a cloud of data points to see the general direction.
* **How it works**: It models the relationship between an input variable (\(x\)) and an output variable (\(y\)) by fitting a straight line equation:
  \[y = mx + c\]
  where \(m\) is the slope (how much \(y\) changes when \(x\) increases) and \(c\) is the intercept (the value of \(y\) when \(x = 0\)).
* **How it learns**: It uses **Ordinary Least Squares (OLS)**. This calculates the vertical distance (error) from each data point to the line, squares the distances (to remove negative signs), adds them up, and adjusts the line to make this sum as small as possible.
* **Use Case**: Predicting future salary based on years of experience.

### 2. Logistic Regression
* **Analogy**: A gatekeeper deciding whether an email is spam (Yes/No).
* **How it works**: Despite the name "regression," it is a **classification** algorithm. It calculates a probability score (between 0 and 1) representing the likelihood that an input belongs to a certain class.
* **The Math (Sigmoid Function)**: To squash any raw number into a probability range (0 to 1), it passes the linear regression equation through the **Sigmoid function**:
  \[S(z) = \frac{1}{1 + e^{-z}}\]
* **Use Case**: Predicting whether a loan applicant will default (Default vs. No Default).

### 3. Decision Trees
* **Analogy**: A game of "20 Questions." You ask a series of yes/no questions to narrow down the correct answer.
* **How it works**: It splits the dataset into subsets based on the features. The split is chosen to make the resulting groups as "pure" (containing mostly one class) as possible.
* **Key Vocabulary**:
  * **Entropy**: A measure of disorder or randomness (impurity) in a group. High entropy means the group is mixed (e.g., 50% cats, 50% dogs). Goal is to reduce entropy to 0.
  * **Gini Impurity**: Another way to measure impurity. It is the probability of misclassifying an item if it were randomly labeled.
  * **Information Gain**: The reduction in entropy after a dataset is split. The tree chooses the question that gives the highest Information Gain.
* **Use Case**: Determining if a customer will buy a product based on age, income, and gender.

### 4. Random Forest (Bagging)
* **Analogy**: Asking 100 people for advice, then taking a majority vote. The group decision is usually much less biased and more stable than a single person's.
* **How it works**: An **ensemble method** that builds multiple decision trees during training. It uses **Bagging (Bootstrap Aggregating)**:
  * **Bootstrap**: Create different training datasets by randomly selecting rows from the main dataset (with replacement).
  * **Aggregate**: Train a decision tree on each dataset. For a new prediction, run it through all trees and take the majority vote (for classification) or average (for regression).
* **Why it's good**: It dramatically reduces the risk of **overfitting** (high variance) compared to a single decision tree.

### 5. Gradient Boosting (Boosting)
* **Analogy**: A team project where each member focuses entirely on correcting the mistakes made by the previous member.
* **How it works**: Unlike Random Forest (where trees are built side-by-side independently), Boosting trains trees **sequentially** (one after another):
  * Tree 1 is trained on the data and makes predictions.
  * Tree 2 is trained specifically on the **residuals** (the errors/mistakes) of Tree 1.
  * Tree 3 is trained on the errors of Tree 2, and so on.
  The final prediction is a weighted sum of all trees.
* **Popular Tool**: **XGBoost** (Extreme Gradient Boosting) is a highly optimized version of this algorithm used widely in Kaggle competitions.

### 6. Support Vector Machines (SVM)
* **Analogy**: Building a wide wall to separate two warring factions. The wall should be as far away from both sides as possible.
* **How it works**: It finds the best boundary line (called a **Hyperplane**) that separates different classes with the maximum possible distance (**Margin**).
* **Key Vocabulary**:
  * **Support Vectors**: The data points that are closest to the boundary line. These points are the most critical because moving them shifts the line.
  * **Kernel Trick**: If data points cannot be separated by a straight line in 2D, the SVM uses a mathematical formula (Kernel) to project the data into a higher dimension (3D) where a flat sheet can easily separate them.

### 7. Naive Bayes
* **Analogy**: Estimating whether it will rain today based on the clouds, wind speed, and yesterday's weather, assuming each factor acts completely independently of the others.
* **How it works**: It uses **Bayes' Theorem** to calculate the probability of a class given some features:
  \[P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}\]
  It is called **Naive** because it assumes that all input features are completely independent of each other (which is rarely true in real life, but the algorithm still works surprisingly well!).
* **Use Case**: Spam filters and text classification (sentiment analysis).

### 8. K-Nearest Neighbors (KNN)
* **Analogy**: "Birds of a feather flock together." If you want to know your class, look at the K nearest neighbors around you.
* **How it works**: A "lazy learner" because it doesn't build a model during training. It simply memorizes the training data. When a new query point arrives:
  1. It calculates the distance (e.g., **Euclidean distance**) to all points in the dataset.
  2. It finds the nearest K points.
  3. The final class is the majority vote of those K points.
* **Use Case**: Recommender systems (recommending movies similar to what similar users liked).

---

## 🎨 Section 2: Unsupervised ML Algorithms

### 9. K-Means Clustering
* **Analogy**: Dividing students in a gymnasium into K separate groups based on where they stand, and adjusting the group boundaries until everyone is grouped with the people closest to them.
* **How it works**:
  1. Pick K random center points (centroids).
  2. Assign each data point to the nearest centroid.
  3. Calculate the new center (average coordinate) of each group.
  4. Repeat steps 2 and 3 until the centroids stop moving.
* **Determining K (The Elbow Method)**: Run K-Means for different values of K (1, 2, 3, 4...). Plot the sum of squared distances of points from their centroids. You will see a curve that looks like an arm. The point where the rate of improvement drops sharply (the "Elbow") is the ideal K.

### 10. Principal Component Analysis (PCA)
* **Analogy**: Taking a 3D object and casting its shadow onto a flat 2D wall. You rotate the object until its shadow captures the most detail (variance) possible.
* **How it works**: A dimensionality reduction technique. It calculates new directions (called **Principal Components**) along which the data is most spread out. It projects the data onto these components, reducing the number of columns while retaining the variance.
* **Complex Word - Variance**: A measure of how spread out data is. If all data points have the same value, variance is 0 (there is no information).

---

## ⚙️ Section 3: Optimizers & Regularization

### 11. Gradient Descent
* **Analogy**: Being blindfolded on a foggy mountain and trying to find the valley (lowest point) by feeling the slope of the ground under your feet. You take steps in the direction that goes down the steepest.
* **How it works**: An optimization algorithm used to update weights in neural networks and linear models to minimize the loss function:
  * **Learning Rate (\(\alpha\))**: The size of the step you take. If it's too small, it takes forever to reach the bottom. If it's too large, you might overshoot the bottom and climb up the other side.
  * **Batch Gradient Descent**: Calculates the gradient using the *entire* dataset. Slow but stable.
  * **Stochastic Gradient Descent (SGD)**: Calculates the gradient using only *one* random data point at a time. Very fast but noisy.
  * **Mini-batch Gradient Descent**: The middle ground; uses small batches of data (e.g., 32 or 64 samples).

### 12. Regularization (L1 vs. L2)
* **Analogy**: Adding a constraint to a building budget. It prevents you from spending too much money on any single luxury item, forcing you to build a simpler, sturdier house.
* **Why we use it**: To prevent **overfitting** by penalizing large weights.
* **L1 Regularization (Lasso)**: Adds a penalty proportional to the absolute value of the weights:
  \[\text{Penalty} = \lambda \sum |w_i|\]
  * *Effect*: It drives less important weights down to exactly 0. This performs automatic **feature selection** by completely turning off unimportant inputs.
* **L2 Regularization (Ridge)**: Adds a penalty proportional to the square of the weights:
  \[\text{Penalty} = \lambda \sum w_i^2\]
  * *Effect*: It shrinks all weights to be very small, but never exactly 0. It is great when you have many useful features that all contribute slightly.


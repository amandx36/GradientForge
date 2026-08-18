# Support Vector Machine (SVM) — 0 to Advanced Notes

---

## 0. Definition

**Simple:** SVM is a computer method that looks at labeled examples and draws the safest possible boundary line between groups.

**Technical:** SVM is a supervised machine learning algorithm that finds the **hyperplane** which **maximizes the margin** between classes. It can also use **kernels** to create non-linear boundaries.

---

## 1. Foundation Concepts (0 → basics)

| Term | Simple meaning |
|---|---|
| Machine Learning | Computer learns a pattern from examples instead of fixed rules |
| Classification | Sorting things into fixed groups (spam / not spam) |
| Feature | A measured input (e.g. `petal_length`) |
| Label/Target | The correct answer (e.g. `species`) |
| Training data | Examples the model learns from |
| Test data | New examples used to check if it really learned |

**Why SVM exists:** Many lines can separate two groups. SVM picks the one with the most "safe space" on both sides, so new points are less likely to be misclassified.

```text
Class A       |       Class B
  ● ● ●       |        ▲ ▲ ▲
  ● ● ●       |        ▲ ▲ ▲
              |
        decision boundary (max margin)
```

---

## 2. Core Vocabulary

| Term | Simple meaning |
|---|---|
| Hyperplane | The "best line" (or flat surface) separating classes |
| Margin | The empty safe space on both sides of the hyperplane |
| Support Vectors | Closest points to the boundary — they alone decide where it sits |
| Kernel | Trick to separate data that isn't separable by a straight line |
| C | How strictly SVM punishes mistakes |
| Gamma | How far one point's influence reaches (non-linear kernels) |

**Support vectors, deeper:** Only nearest points matter. Moving a support vector can shift the boundary; moving a far-away point usually changes nothing.
- `support_` → index numbers of support vector points
- `support_vectors_` → their actual feature values
- `n_support_` → count of support vectors per class

---

## 3. Hard Margin vs Soft Margin

| | Hard Margin | Soft Margin |
|---|---|---|
| Assumes | Perfectly separable, no noise | Real messy data with overlap |
| Allows mistakes | No | Yes, controlled by `C` |
| Real-world use | Rare | **Standard choice** |

---

## 4. C Parameter — depth

**Memory trick:** `C = Cost of mistakes`

- **High C** → "I hate mistakes" → tight fit → risk of **overfitting**
- **Low C** → "few mistakes okay" → wider margin → risk of **underfitting**

```text
C=0.01 → very tolerant, wide margin
C=1    → balanced default
C=100  → strict, narrow margin
```

---

## 5. Feature Scaling

SVM boundary depends on **distance** between points. Unscaled features (e.g. 1–5 vs 1–10,000) let the big-range feature dominate.

```python
X_train_scaled = scaler.fit_transform(X_train)  # fit only on train
X_test_scaled  = scaler.transform(X_test)        # reuse same scaling
```
 Never fit the scaler on test data — that's **data leakage**.

---

## 6. Kernel Trick (going non-linear)

Used when a straight line can't separate the classes.

| Kernel | Boundary | Best for |
|---|---|---|
| `linear` | Straight | Roughly linear, many features |
| `poly` | Curved (polynomial) | Some non-linear structure |
| `rbf` | Very flexible curve | Unknown/complex patterns (most common default) |
| `sigmoid` | S-shaped | Rare, RBF usually preferred |

### Gamma (rbf/poly/sigmoid)

**Memory trick:** `Gamma = how far one point's influence reaches`
- Low gamma = big flashlight beam → smooth boundary
- High gamma = small flashlight beam → complex, wiggly boundary → overfit risk

### C + Gamma combined

| C \ Gamma | Low gamma | High gamma |
|---|---|---|
| Low C | Smooth, may underfit | Some complexity, still tolerant |
| High C | Smooth but strict | Very complex + strict → high overfit risk |

---

## 7. Multiclass Handling

SVM is naturally binary. For 3+ classes (e.g. Iris), sklearn uses **One-vs-One (OvO)** internally — trains a classifier per class pair, majority vote wins. `decision_function_shape="ovr"` just reshapes the output.

---

## 8. Math — light version (0 → advanced path)

| Level | Idea |
|---|---|
| Beginner | `w · x + b = 0` is the line equation; sign tells which side |
| Intermediate | Margin = distance from boundary to nearest points; SVM maximizes it |
| Advanced | Optimization uses Lagrange multipliers → **dual form** → allows kernel trick |
| Expert | KKT conditions decide which points become support vectors (non-zero multipliers) |

Kernel formulas:
```text
Linear:     K(x, x') = x · x'
Polynomial: K(x, x') = (gamma * x·x' + coef0)^degree
RBF:        K(x, x') = exp(-gamma * ||x - x'||²)
```

---

## 9. Pros  vs Cons 

| Pros | Cons |
|---|---|
| Works well on small/medium datasets | Slow/expensive on very large datasets |
| Strong in high-dimensional spaces | Kernel SVM needs more compute/memory |
| Handles non-linear data via kernels | No single "best" kernel — must choose |
| Max-margin idea generalizes well | Needs careful tuning (`C`, `gamma`, kernel) |
| Works for classification (SVC) and regression (SVR) | Needs feature scaling |
| Good with sparse/high-dim data | Harder to interpret than tree/linear models |
| Robust when there's a clear margin | No probability output by default |

**Memory line:** *SVM can be powerful, but needs the right data, scaling, kernel, and parameters.*

---

## 10. Where to Use SVM 

- Text classification, spam detection, sentiment analysis
- Image classification on small/medium datasets
- Handwriting recognition
- Bioinformatics classification
- High-dimensional data (features ≳ samples)

## 11. Where NOT to Use SVM 

- Extremely large datasets (training cost grows fast)
- Need for very fast/simple training
- Need for high interpretability → use trees instead
- Huge complex raw data (images/audio at scale) → deep learning wins
- Central need for clean probability outputs → simpler probabilistic models

---

## 12. Common Mistakes

| Wrong | Correct |
|---|---|
| Not scaling features | Always scale (`StandardScaler`) |
| Fitting scaler before train/test split | Fit only on train, transform test |
| Judging only by accuracy | Check precision/recall/F1, confusion matrix |
| Assuming RBF is always best | Compare kernels via cross-validation |
| Treating all points as support vectors | Only nearest points are (`n_support_`) |
| Skipping hyperparameter tuning | Use `GridSearchCV`/`RandomizedSearchCV` |

---

## 13.  Q&A (short answers)

1. **What is SVM?** Algorithm that finds the max-margin boundary between classes.
2. **Why "Support Vector"?** Because the closest points "support"/hold up the boundary's position.
3. **What is a margin?** Safe empty space around the decision boundary.
4. **What is a hyperplane?** The separating line/surface SVM draws.
5. **Why maximize margin?** Wider margin → better generalization on new data.
6. **What is C?** Penalty strength for training mistakes.
7. **What is gamma?** How far a point's influence reaches (non-linear kernels).
8. **C vs gamma?** C controls error tolerance; gamma controls boundary curviness/locality.
9. **What is the kernel trick?** Lets SVM separate non-linear data without explicitly computing high-dimensional coordinates.
10. **Why scale features?** Distance-based method; unscaled features distort results.
11. **Hard vs soft margin?** Hard = no errors allowed (rare, needs clean data); soft = allows some errors (`C` controls it).
12. **SVC vs SVR?** SVC = classification; SVR = regression, uses epsilon-insensitive loss.
13. **How does SVM handle multiclass?** Via One-vs-One internally in sklearn.
14. **Effect of increasing C?** Stricter, tighter fit, overfit risk.
15. **Effect of increasing gamma?** More local, complex boundary, overfit risk.
16. **Why can RBF overfit?** High gamma makes it fit tiny local noise patterns.
17. **Why can SVM be slow on large data?** Kernel computation grows with number of samples.
18. **What are support vectors?** Training points closest to the boundary, defining it.
19. **What's the dual form?** Reformulated optimization using Lagrange multipliers — enables kernels.
20. **Linear vs RBF?** Linear for roughly separable data/many features; RBF for unknown non-linear patterns.
21. **What if data isn't linearly separable?** Use kernel trick (e.g. RBF) or allow soft margin errors.
22. **How to handle imbalanced classes?** `class_weight="balanced"` adjusts penalty per class.
23. **Why doesn't SVM give probabilities by default?** It outputs a decision score, not probability; `probability=True` adds extra calibration cost.
24. **When would you replace SVM?** Huge datasets → trees/neural nets; need interpretability → decision tree; need probabilities → logistic regression.

---

## 14. One-Page Cheat Sheet

```text
Definition: Finds the boundary with max margin between classes.

Key params:
  C      → cost of mistakes (low=tolerant, high=strict)
  gamma  → influence range (low=wide, high=narrow) [rbf/poly/sigmoid]
  kernel → linear / poly / rbf / sigmoid

Must scale features before training.

support_          → indices of support vectors
support_vectors_  → their feature values
n_support_        → count per class

Multiclass → One-vs-One internally (sklearn SVC)

Use when: small/medium data, high-dimensional, clear/complex margins
Avoid when: huge datasets, need speed, need interpretability, need probabilities
```
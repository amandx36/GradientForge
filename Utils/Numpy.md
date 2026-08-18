# 📘 Complete NumPy Notes (Simple + Easy)

> Sab kuch ek hi file mein — easy language, seedha samajh aane wala. Hinglish + simple examples.

---

## 1. NumPy Kya Hai?

NumPy ek Python library hai jo **numbers ke bade arrays** ko fast handle karti hai.

```python
import numpy as np
```

**Kyun use karte hain?** Python list slow hoti hai bade numeric data pe. NumPy fast hai kyunki wo C language mein likhi gayi hai andar se.

```python
list1 = [1, 2, 3]
arr1 = np.array([1, 2, 3])

# List mein add karne ke liye loop chahiye
# NumPy mein direct:
print(arr1 + arr1)   # [2 4 6]
```

Install: `pip install numpy`

---

## 2. Array Kya Hota Hai (ndarray)

Array matlab numbers ka ek container — 1D (line), 2D (table), 3D (cube), aur aage bhi.

```python
a0 = np.array(5)                     # 0D - single number
a1 = np.array([1, 2, 3])             # 1D - line
a2 = np.array([[1, 2], [3, 4]])      # 2D - table
a3 = np.array([[[1,2],[3,4]],[[5,6],[7,8]]])  # 3D - cube
```

- **0D** = ek number
- **1D** = list jaisa
- **2D** = rows + columns (table)
- **3D** = tables ka stack (jaise image ke RGB channels)

---

## 3. Array Banane Ke Tarike

| Function | Kya karta hai |
|---|---|
| `np.array([1,2,3])` | apna data se array banao |
| `np.zeros((2,3))` | sab 0 se bhara array |
| `np.ones((2,3))` | sab 1 se bhara array |
| `np.full((2,3), 7)` | sab custom value (7) se bhara |
| `np.empty((2,3))` | khali (garbage values) — fast, risky |
| `np.arange(0,10,2)` | step wale numbers: [0 2 4 6 8] |
| `np.linspace(0,1,5)` | 0 se 1 tak 5 equal parts: [0. 0.25 0.5 0.75 1.] |
| `np.eye(3)` | identity matrix (diagonal pe 1) |

```python
print(np.zeros((2,3)))
# [[0. 0. 0.]
#  [0. 0. 0.]]

print(np.arange(0, 10, 2))   # [0 2 4 6 8]
print(np.linspace(0, 1, 5))  # [0.   0.25 0.5  0.75 1.  ]
```

**Easy yaad rakhne ka tarika:** `arange` = step batao, `linspace` = count batao.

---

## 4. Array Ki Properties

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

arr.ndim      # 2        -> kitni dimensions
arr.shape     # (2, 3)   -> rows, columns
arr.size      # 6        -> total elements
arr.dtype     # int64    -> data type
arr.itemsize  # 8        -> ek element kitne bytes ka
arr.nbytes    # 48       -> total memory
```

**Simple trick:** `shape` = size har dimension mein, `size` = total elements, `ndim` = kitni dimensions.

---

## 5. Indexing & Slicing

Indexing = ek element nikalna. Slicing = ek range nikalna.

```python
arr = np.array([10, 20, 30, 40, 50])

arr[0]      # 10 (pehla)
arr[-1]     # 50 (last)
arr[1:4]    # [20 30 40]
arr[::2]    # [10 30 50]  -> har 2nd element
arr[::-1]   # [50 40 30 20 10]  -> reverse
```

2D array mein:

```python
mat = np.array([[1,2,3],[4,5,6],[7,8,9]])

mat[0, 0]   # 1  (row 0, col 0)
mat[1]      # [4 5 6]  (poori row 1)
mat[:, 0]   # [1 4 7]  (poora column 0)
mat[0:2, 0:2]  # top-left 2x2 chunk
```

**Boolean indexing** — condition se filter karna:

```python
arr = np.array([10, 25, 30, 45])
print(arr[arr > 20])   # [25 30 45]
```

**Fancy indexing** — specific indexes ki list se select karna:

```python
arr = np.array([10, 20, 30, 40, 50])
print(arr[[0, 2, 4]])  # [10 30 50]
```

---

## 6. Reshaping

Array ka shape badalna without data change kiye.

```python
arr = np.arange(1, 7)          # [1 2 3 4 5 6]
reshaped = arr.reshape(2, 3)
# [[1 2 3]
#  [4 5 6]]

arr.flatten()   # 2D ko wapas 1D bana do (copy)
arr.ravel()     # same, but view (memory share) jab possible ho
arr.T           # transpose - rows <-> columns

np.expand_dims(arr, axis=0)   # ek extra dimension add
np.squeeze(arr)               # extra dimension (size=1) hatao
```

**Easy yaad:** `flatten` = hamesha copy, `ravel` = jab possible ho to view (fast).

---

## 7. Copy vs View (Bahut Important!)

- **View** = original array se memory share karta hai. Ek badla to dusra bhi badal jayega.
- **Copy** = poori nayi memory, independent.

```python
arr = np.array([1, 2, 3])

view_arr = arr[0:2]      # slicing -> view (memory shared)
view_arr[0] = 100
print(arr)                # [100 2 3]  <- original bhi badal gaya!

copy_arr = arr.copy()     # explicit copy -> independent
copy_arr[0] = 999
print(arr)                # arr change nahi hua
```

**Simple rule:** Basic slicing = view. Boolean/fancy indexing = copy. Confusion ho to `.copy()` use kar lo safe rehne ke liye.

---

## 8. Data Types (dtype)

```python
arr = np.array([1, 2, 3], dtype=np.float64)
arr2 = arr.astype(np.int32)   # type convert karo
```

Common dtypes: `int32`, `int64`, `float32`, `float64`, `bool`, `object` (strings ke liye)

**Kyun matter karta hai?** Chhota dtype = kam memory, fast processing. Bade dataset mein `float64` ki jagah `float32` use karke memory bacha sakte ho.

---

## 9. Basic Operations (Vectorization)

Array pe direct math operations — element by element, bina loop ke.

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

a + b   # [11 22 33]
a - b   # [-9 -18 -27]
a * b   # [10 40 90]
a / b   # [0.1 0.1 0.1]
a ** 2  # [1 4 9]
a > 1   # [False True True]
```

**Loop vs vectorization speed:**

```python
# Slow (loop)
result = []
for x in range(1000000):
    result.append(x * 2)

# Fast (vectorized)
arr = np.arange(1000000)
result = arr * 2
```

NumPy hamesha faster hai bade data pe kyunki loop nahi chalta — sab ek saath C code mein hota hai.

---

## 10. Axis — Easy Samjho

`axis` batata hai operation **kis direction mein** chalega.

```python
mat = np.array([[1, 2, 3],
                 [4, 5, 6]])

mat.sum(axis=0)   # [5 7 9]   -> column-wise (upar se neeche add)
mat.sum(axis=1)   # [6 15]    -> row-wise (left se right add)
mat.sum()         # 21        -> sab kuch add
```

**Super simple trick:**
- `axis=0` matlab **columns ke through neeche jao** (rows collapse hoti hain)
- `axis=1` matlab **rows ke through side jao** (columns collapse hoti hain)

Same logic `mean`, `max`, `min` sab pe chalta hai:

```python
mat.mean(axis=0)  # column-wise average
mat.max(axis=1)   # row-wise maximum
```

---

## 11. Aggregation Functions

```python
arr = np.array([3, 7, 1, 9, 4])

arr.sum()      # 24
arr.mean()     # 4.8
arr.median()   # not a method, use np.median(arr) -> 4.0
arr.min()      # 1
arr.max()      # 9
arr.std()      # standard deviation
arr.var()      # variance
arr.argmin()   # 2  (index of min value)
arr.argmax()   # 3  (index of max value)
arr.cumsum()   # [ 3 10 11 20 24]  -> running total
```

---

## 12. Broadcasting

Broadcasting = different shapes ke arrays ke beech operation karna, NumPy automatically size match kar deta hai.

```python
arr = np.array([[1, 2, 3],
                 [4, 5, 6]])

arr + 10
# [[11 12 13]
#  [14 15 16]]
# -> 10 (scalar) har element mein add ho gaya, automatically

row = np.array([1, 0, 1])
arr + row
# [[2 2 4]
#  [5 5 7]]
# -> row ko har row pe apply kar diya
```

**Simple rule:** Chhoti shape ko NumPy "stretch" kar deta hai badi shape ke jaisa banane ke liye, bina extra memory use kiye — **agar shapes compatible hon** (ek dimension match kare ya 1 ho).

**Error jab shapes match na ho:**

```python
a = np.array([1, 2, 3])
b = np.array([1, 2])
# a + b -> ValueError: shapes (3,) (2,) not compatible
```

---

## 13. Universal Functions (ufuncs)

Math functions jo poore array pe ek saath chalte hain.

```python
arr = np.array([1, 4, 9, 16])

np.sqrt(arr)   # [1. 2. 3. 4.]
np.exp(arr)    # e^x
np.log(arr)    # natural log
np.abs([-1,-2])# [1 2]
np.round(3.567, 2)  # 3.57
np.floor(3.7)  # 3.0
np.ceil(3.2)   # 4.0
np.sin(arr)
np.cos(arr)
```

---

## 14. Conditional Operations

```python
arr = np.array([45000, 60000, 30000, 80000])

# where(condition, if_true, if_false)
result = np.where(arr > 50000, "High", "Low")
print(result)  # ['Low' 'High' 'Low' 'High']

# multiple conditions
np.select(
    [arr < 40000, arr < 70000, arr >= 70000],
    ["Low", "Medium", "High"]
)
```

---

## 15. Array Manipulation

```python
a = np.array([1, 2])
b = np.array([3, 4])

np.concatenate([a, b])   # [1 2 3 4]
np.vstack([a, b])        # stack vertically -> [[1 2] [3 4]]
np.hstack([a, b])        # stack horizontally -> [1 2 3 4]

np.split(np.arange(10), 2)   # 2 equal parts
np.append(a, 5)              # [1 2 5]
np.insert(a, 1, 99)          # [1 99 2]
np.delete(a, 0)              # [2]
```

---

## 16. Sorting & Searching

```python
arr = np.array([5, 2, 9, 1])

np.sort(arr)        # [1 2 5 9]
np.argsort(arr)     # [3 1 0 2]  -> indexes jo sort order dete hain
np.unique(arr)      # unique values
np.where(arr > 3)   # indexes jaha condition true hai
np.nonzero(arr)     # non-zero elements ke indexes
```

---

## 17. Random Module

```python
rng = np.random.default_rng(seed=42)   # seed = same result har baar (reproducibility)

rng.integers(1, 100, size=5)    # 5 random integers between 1-100
rng.random(5)                   # 5 random floats between 0-1
rng.normal(0, 1, 5)             # normal distribution
rng.choice([1,2,3,4], size=2)   # random pick
rng.shuffle(arr)                # array ko shuffle karo (in-place)
```

**Seed kyun important hai?** Same seed = same random numbers har run mein. Isse results reproducible hote hain (dusra insaan bhi wahi output dekh sake).

---

## 18. Statistics

```python
arr = np.array([2, 4, 4, 6, 8])

np.mean(arr)        # average
np.median(arr)      # middle value
np.std(arr)         # spread of data
np.var(arr)         # std squared
np.percentile(arr, 50)  # 50th percentile = median
np.corrcoef(a, b)   # correlation between two arrays
```

---

## 19. Linear Algebra (Basics)

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

A @ B              # matrix multiplication (dot product)
np.dot(A, B)       # same thing
A.T                # transpose
np.linalg.inv(A)   # inverse
np.linalg.det(A)   # determinant
np.linalg.norm(A)  # magnitude/length
```

**Kab use hota hai?** Machine learning, image processing, equations solve karne mein.

---

## 20. Performance Tips

- **Loop se bacho** — jitna ho sake vectorized operations use karo.
- **dtype optimize karo** — `float32` use karo agar `float64` ki precision nahi chahiye.
- **Unnecessary copies avoid karo** — sirf jab zaroorat ho tab `.copy()` karo.
- Speed check karne ke liye:

```python
import time
start = time.time()
# ... code ...
print(time.time() - start)
```

---

## 🎯 Quick Cheat Sheet

| Kaam | Function |
|---|---|
| Array banao | `np.array()` |
| Zeros/Ones | `np.zeros()`, `np.ones()` |
| Sequence | `np.arange()`, `np.linspace()` |
| Shape dekho | `arr.shape` |
| Reshape | `arr.reshape()` |
| Filter | `arr[arr > x]` |
| Sum/Mean | `arr.sum()`, `arr.mean()` |
| Row/Column-wise | `axis=0` (columns) / `axis=1` (rows) |
| Sort | `np.sort()` |
| Random | `np.random.default_rng()` |
| Matrix multiply | `A @ B` |

---

## 📝 Quick Self-Test

1. `axis=0` aur `axis=1` mein kya difference hai?
2. `view` aur `copy` mein kya farak hai?
3. `arange` aur `linspace` mein kab konsa use karoge?
4. Boolean indexing kaise kaam karta hai?
5. Broadcasting kya hai, easy example do.

<details>
<summary>Answers</summary>

1. `axis=0` = column-wise (top se bottom), `axis=1` = row-wise (left se right).
2. View memory share karta hai (original change hota hai), copy independent hoti hai.
3. `arange` jab exact step size pata ho; `linspace` jab exact count of points chahiye ho.
4. Condition se True/False mask banta hai, phir sirf True wale elements select hote hain: `arr[arr > 5]`.
5. Broadcasting = chhoti shape ko badi shape ke sath match karke operation karna, jaise `arr + 5` (scalar automatically har element pe apply hota hai).

</details>

---

**Ye poora NumPy notes ek hi file mein ho gaya bhai! Ab agar Pandas bhi chahiye isi easy style mein, bol dena — wo bhi bana dunga.** 🔥


# Twelvefold Way

## 1. Introduction

Counting problems are a staple of middle school and high school mathematics competitions like the AMC. Often among the more challenging questions, these problems require not only creativity but also a deep understanding of combinatorial principles. One powerful framework for organizing and solving these types of problems is the **Twelvefold Way**. This method provides a structured lens through which all common ball-and-box counting scenarios can be classified, making it easier for learners to grasp the subtle differences and connections between them.

The Twelvefold Way is especially useful for students who already have a firm grasp of permutations, combinations, and the stars-and-bars technique. Interestingly, all three of these classic topics are actually special cases within the Twelvefold framework. The purpose of this essay is to introduce the Twelvefold Way in an intuitive and accessible manner, with middle and high school learners in mind.

To make the abstract concepts more tangible, we will use the metaphor of placing balls into boxes. The twelve cases arise from three dimensions of variation:

1. **Nature of the balls**: Are they **distinct** or **identical**?
2. **Nature of the boxes**: Are they **distinct** or **identical**?
3. **Rules of placement**: Are there **no restrictions**, **at most one ball per box**, or **at least one ball per box**?

It’s not uncommon for people to use different terminology to describe the same concept. For instance, instead of saying "distinct balls," one might also say "labeled," "marked," or "distinguishable" balls. Similarly, "identical boxes" may be referred to as "unlabeled," "unmarked," or "indistinguishable" boxes. Understanding these synonyms is essential for correctly identifying and categorizing the counting scenario described in a problem. 

Mathematicians also tend to adopt concise terminology for efficiency. For example, the requirement that each box receives at least one ball is known as the **surjective condition**, which implies that the number of balls must be greater than or equal to the number of boxes. Conversely, the condition that each box receives at most one ball is known as the **injective condition**, which implies that there are at least as many boxes as balls.

Before diving into the full twelve-case classification, we will begin by exploring a fundamental case that will appear in several scenarios within the Twelvefold Way: the **Stirling numbers of the second kind**. These count the number of ways to partition a set of distinct elements into non-empty, unlabeled subsets. Although there's also a first kind of Stirling number, we will focus solely on the second kind for the purpose of this exposition.

---

## 2. Stirling Numbers of the Second Kind

The **Stirling numbers of the second kind** represent one of the twelve scenarios in the Twelvefold Way. Denoted by `S(n, k)`, these numbers count the ways to distribute `n` **distinct** balls into `k` **identical** boxes such that each box receives **at least one** ball. This requirement is called the **surjective condition**. In contrast, requiring that no box receives more than one ball is known as the **injective condition**.

A classic example is: *In how many ways can we divide 6 students into 2 groups?* Since the groups are unlabeled and each must contain at least one student, this falls under the case `S(6, 2)`.

Let’s explore some base cases and properties:

- `S(n, 1) = 1`: All balls must go into the single box.
- `S(n, n) = 1`: Each ball must go into its own box.
- `S(n, k) = 0` if `k > n`: It is impossible to place `n` balls into more than `n` boxes with each box non-empty.

Now, consider a nontrivial example: `S(3, 2)`. How many ways can we distribute 3 **distinct** balls into 2 **identical** boxes with at least one ball per box?

Let’s use a recursive approach. we can consider how to place the **last ball** into the existing groups/boxes formed by the first $n - 1$ balls. This leads to two distinct cases:

### Case 1: Placing the last ball into an existing non-empty box

Suppose we have already placed the first 2 balls (or $n - 1$ balls in the general case) into 2 boxes (or $k$ boxes), and each box already contains at least one ball—i.e., the **surjective condition** is satisfied.

Now we place the last ball (the 3rd ball, or the $n^th$ ball in general) into one of these existing non-empty boxes. Since there are $k$ such boxes to choose from, the number of ways to complete the grouping in this case is:

$$
k \cdot S(n - 1, k)
$$

In our specific example with $n = 3$ and $k = 2$, there are 2 existing boxes, so the contribution from this case is:

$$
2 \cdot S(2, 2)
$$

### Case 2: Placing the last ball into a new (previously empty) box

In this scenario, the first 2 balls (or $n - 1$ balls in the general case) were placed into only 1 of the 2 boxes (or into $k - 1$ of the $k$ boxes), leaving exactly **one box empty**.

To satisfy the **surjective condition** (i.e., no empty boxes allowed), the last ball **must** go into the remaining empty box to ensure all $k$ boxes are non-empty.

The number of such configurations is:

$$
S(n - 1, k - 1)
$$

In our specific example with $n = 3$ and $k = 2$, the first 2 balls occupy only 1 box, and the 3rd ball must go into the second box. So the contribution from this case is:

$$
S(2, 1)
$$

So we get:

```
S(3, 2) = 2 * S(2, 2) + S(2, 1) = 2 * 1 + 1 = 3
```

Let’s do one more:

```
S(4, 2) = 2 * S(3, 2) + S(3, 1) = 2 * 3 + 1 = 7
```

And one further:

```
S(4, 3) = 3 * S(3, 3) + S(3, 2) = 3 * 1 + 3 = 6
```

From this, we can see that the Stirling numbers of the second kind satisfy the following **recursive formula**:

```
S(n, k) = k * S(n-1, k) + S(n-1, k-1)
```

This formula reflects two cases:

1. Assigning the `n`th ball to one of the existing `k` non-empty boxes, contributing `k * S(n-1, k)`.
2. Placing the `n`th ball into a new, separate box, contributing `S(n-1, k-1)`.

This recurrence makes it easy to construct a **Stirling number table**, much like Pascal’s triangle for combinations.

###  Stirling Number Table (Second Kind)

| `n \ k` | 1 | 2   | 3   | 4    |
|---------|---|-----|-----|------|
| 1       | 1 | 0   | 0   | 0    |
| 2       | 1 | 1   | 0   | 0    |
| 3       | 1 | 3   | 1   | 0    |
| 4       | 1 | 7   | 6   | 1    |
| 5       | 1 | 15  | 25  | 10   |
| 6       | 1 | 31  | 90  | 65   |
| 7       | 1 | 63  | 301 | 350  |
| 8       | 1 | 127 | 966 | 1701 |

Each entry `S(n, k)` in the table represents the number of ways to partition `n` distinct items into `k` non-empty, unlabeled groups.

---
### Computing Stirling Numbers of the Second Kind via Set Partition Casework

Another way to compute Stirling numbers of the second kind is through **casework based on set partitions**. This method involves listing all valid partitions of `n` distinct balls into `x` indistinguishable boxes (each containing at least one ball, as required by the surjective condition), and then using combinations to count the number of ways to realize each partition. A key challenge here is to **watch out for overcounting**, especially when some box sizes are repeated.

Let’s walk through the example of distributing 6 **distinct** balls into 3 **identical** boxes, with **each box containing at least one ball**. The possible partitions of 6 into 3 positive integers are:

1. **(4, 1, 1)**
2. **(3, 2, 1)**
3. **(2, 2, 2)**

##### Partition: (4, 1, 1)
Here, two boxes have the same size, so overcounting must be corrected.

First, choose 4 balls to go into the first box: `C(6, 4)`  
Then choose 1 of the remaining 2 to go into the second box: `C(2, 1)`  
The last ball goes into the third box: `C(1, 1)`

This gives:
```
C(6, 4) * C(2, 1) * C(1, 1) = 15 * 2 * 1 = 30
```

Since the two boxes containing one ball each are indistinguishable, we divide by `2!` to correct for overcounting:
```
30 / 2! = 15 ways
```

An alternative approach is to assign the 1-ball boxes first:
```
C(6, 1) * C(5, 1) * C(4, 4) = 6 * 5 * 1 = 30
```
Again, divide by `2!`:
```
30 / 2! = 15 ways
```

As you become more comfortable with these types of problems, it may feel more natural to directly compute `C(6, 4)` as the number of ways to assign 4 balls to one box, assuming that the remaining 2 balls will go one each into the other two boxes. Since those two singleton boxes are indistinguishable and symmetric in size, dividing by `2!` accounts for the overcounting due to their interchangeability.

This shortcut works only when the remaining boxes each receive exactly one ball. As we’ll see in a later example, this approach breaks down when multiple boxes receive equal numbers of balls greater than one, as the symmetry becomes more complex and overcounting is harder to adjust for correctly.

##### Partition: (3, 2, 1)
In this case, all box sizes are distinct, so no overcounting correction is needed.

Choose 3 balls for one box: `C(6, 3)`  
From the remaining 3, choose 2 for another box: `C(3, 2)`  
The last ball goes into the third box: `C(1, 1)`

This gives:
```
C(6, 3) * C(3, 2) * C(1, 1) = 20 * 3 * 1 = 60 ways
```

You can also compute this as:
```
C(6, 1) * C(5, 2) = 6 * 10 = 60
```

Both approaches are valid.

##### Partition: (2, 2, 2)
Here, all box sizes are the same, so there is significant overcounting.

First, choose 2 balls out of 6 for the first box: `C(6, 2)`  
Then choose 2 out of the remaining 4 for the second box: `C(4, 2)`  
The final 2 go to the third box: `C(2, 2)`

This gives:
```
C(6, 2) * C(4, 2) * C(2, 2) = 15 * 6 * 1 = 90
```

Since all three boxes are the same size and indistinguishable, we divide by `3!` to correct for all permutations of the same grouping:
```
90 / 3! = 90 / 6 = 15 ways
```

##### Total
Adding all valid cases:
```
15 (from 4,1,1) + 60 (from 3,2,1) + 15 (from 2,2,2) = 90
```

This matches `S(6, 3) = 90`, the Stirling number of the second kind.

#### When is this method useful?
This case-based combinatorial method works well when `n` and `x` are small, as the number of valid partitions is manageable. It’s also especially helpful in customized constraints, such as requiring at least two balls per box, where the general Stirling number formula no longer applies directly.

---

## 3. Twelvefold Way

To understand the Twelvefold Way, we organize all 12 scenarios into a **4-by-3 grid** based on the properties of balls and boxes, and the constraints on placement.

- **Rows** capture the **identity of balls and boxes**:
  1. Distinct balls into distinct boxes
  2. Distinct balls into identical boxes
  3. Identical balls into distinct boxes
  4. Identical balls into identical boxes

- **Columns** represent the **distribution constraints**:
  1. **No restrictions**: Each box can hold any number of balls
  2. **Injective (at most one per box)**: No box may contain more than one ball
  3. **Surjective (at least one per box)**: Each box must have at least one ball

Each cell in this grid represents a unique counting scenario, and we summarize the corresponding counting formula below:

|                        | No Restrictions                        | Injective (≤1 per box)               | Surjective (≥1 per box)                        |
|------------------------|----------------------------------------|-------------------------------------|------------------------------------------------|
| Distinct balls → Distinct boxes | $k^n$                                | $P(k, n) = k! / (k - n)!$ (if `n ≤ k`) | `k! * S(n, k)`                                 |
| Distinct balls → Identical boxes | Sum over partitions with distinct parts: `S(n, 1) + ... + S(n, k)` | `1` if `n ≤ k`, else `0`               | `S(n, k)`                                     |
| Identical balls → Distinct boxes | `C(n + k - 1, k - 1)` (stars and bars) | `C(k, n)` (if `n ≤ k`)                 | `C(n - 1, k - 1)`                             |
| Identical balls → Identical boxes | Number of integer partitions of `n` into ≤`k` parts | `1` if `n ≤ k`, else `0`               | Number of integer partitions of `n` into exactly `k` parts |

This table allows us to classify any counting problem involving placing balls into boxes. By identifying the nature of the objects (balls and boxes) and the constraint applied (if any), we can immediately locate the right formula or approach to use.

### Distinct Balls → Distinct Boxes

The first row of the Twelvefold Way covers some of the most familiar counting scenarios—especially for math competition participants.

1. **No Restrictions:**  
When placing `n` distinct balls into `k` distinct boxes without any constraints, each ball has `k` choices. By the multiplication rule, the total number of possible assignments is:  
`k^n`

2. **Injective (At Most One Ball per Box):**  
Here, each box may contain at most one ball. Since the balls are distinct and the boxes are also distinct, we are essentially counting permutations:
- The first ball can go into any of the `k` boxes  
- The second ball can go into any of the remaining `k - 1` boxes  
- And so on, for `n` balls (with `n <= k`)

This gives:  
`P(k, n) = k! / (k - n)!`

3. **Surjective (At Least One Ball per Box):**  
This condition requires every box to contain at least one ball. This is closely related to the Stirling numbers of the second kind, `S(n, k)`, which count the number of ways to partition `n` distinct balls into `k` non-empty, **identical** boxes. Since our boxes here are **distinct**, we must account for all permutations of those `k` boxes:

`Total ways = k! * S(n, k)`

This progression—no restrictions, injective, and surjective—demonstrates how adding structural rules to a base scenario transforms the counting logic and requires increasingly refined tools like permutations and Stirling numbers.

### Distinct Balls → Identical Boxes

The second row of the Twelvefold Way explores scenarios where `n` distinct balls are distributed into `k` identical boxes.

1. **No Restrictions:**  
When there are no constraints, each of the `n` balls can be placed into any of the `k` boxes. However, because the boxes are identical, different arrangements that only differ by box labels are considered the same. To count correctly, we break the problem into subcases: distributing the balls into exactly 1, 2, ..., `k` non-empty boxes. For each `i` from 1 to `k`, the number of ways to place `n` distinct balls into `i` non-empty **identical** boxes is given by the Stirling number `S(n, i)`. So the total number of ways is:

`S(n, 1) + S(n, 2) + ... + S(n, k)`

2. **Injective (At Most One Ball per Box):**  
In this scenario, each box can contain at most one ball. Since the boxes are identical, it only matters which subset of `n` balls are placed into `k` boxes—not which box gets which ball. When `n <= k`, each ball can occupy a separate box, with `(k - n)` boxes left empty. Because the boxes are indistinguishable, there's only **one** way to assign the `n` balls under this constraint. When `n > k`, it's impossible to assign one ball per box without violating the "at most one" rule, so the count is **zero**.

3. **Surjective (At Least One Ball per Box):**  
This is precisely the definition of the Stirling number of the second kind, `S(n, k)`. It counts the number of ways to partition `n` distinct balls into `k` non-empty, identical boxes.

This row reveals how removing the distinction between boxes significantly reduces the number of valid arrangements and forces us to rely on more abstract tools like partition counting.

### Identical Balls → Distinct Boxes

The third row of the Twelvefold Way explores scenarios where `n` identical balls are placed into `k` distinct boxes.

1. **No Restrictions:**  
This is the classical **stars-and-bars** problem. The question is equivalent to finding the number of non-negative integer solutions to the equation:

$$
x₁ + x₂ + ... + x_k = n
$$

Here, each `xᵢ` represents the number of balls in box `i`. Because the balls are identical but the boxes are labeled (distinct), each solution corresponds to a unique arrangement. The number of solutions is:

`C(n + k - 1, k - 1)`

This formula arises from placing `k - 1` dividers (bars) among the `n` identical balls (stars) to separate them into `k` labeled groups.

2. **Injective (At Most One Ball per Box):**  
This case only makes sense when `n <= k`, since no box may contain more than one ball. To count the valid arrangements, we simply choose `n` boxes out of `k` to place one ball each:

`C(k, n)`

If `n > k`, the count is `0` because there aren’t enough boxes to place all balls without violating the injective condition.

3. **Surjective (At Least One Ball per Box):**  
In this scenario, each of the `k` boxes must contain at least one ball. First, assign one ball to each box to satisfy the surjective condition. That uses up `k` balls, leaving `n - k` balls to be distributed without restrictions.

Now we’re back to the unrestricted stars-and-bars case, but with `n - k` identical balls and `k` distinct boxes:

`C((n - k) + k - 1, k - 1) = C(n - 1, k - 1)`

Despite the intimidating condition, this case reduces cleanly to a familiar formula after a simple adjustment.

This row highlights how classic counting tools like combinations and stars-and-bars are fundamental when working with identical objects.

### Identical Balls → Identical Boxes

The fourth and final row of the Twelvefold Way addresses scenarios where `n` identical balls are distributed into `k` identical boxes. This is the most abstract and nuanced case, often involving concepts from integer partition theory.

1. **No Restrictions:**  
In this case, we count the number of ways to distribute `n` identical balls into up to `k` non-empty, identical boxes. Since both the balls and boxes are indistinguishable, different permutations of the same grouping are considered the same. We proceed by summing the number of integer partitions of `n` into exactly `1`, `2`, ..., and `k` parts:

`partition(n, 1) + partition(n, 2) + ... + partition(n, k)`

where `partition(n, i)` is the number of ways to partition `n` into `i` nonzero parts.

2. **Injective (At Most One Ball per Box):**  
Because the balls are identical and each box can hold at most one ball, each valid configuration consists of selecting `n` boxes out of the `k` to hold exactly one ball each. But since the boxes are also identical, the only thing that matters is **how many** balls are placed, not which boxes are chosen. Thus, if `n <= k`, there's only **one** way to do this. If `n > k`, the count is `0`.

3. **Surjective (At Least One Ball per Box):**  
This is where partition theory shines. We're asked to divide `n` identical balls into `k` identical boxes, each containing at least one ball. This is equivalent to counting the number of integer partitions of `n` into exactly `k` positive integers:

`partition(n, k)`

We encountered this concept earlier when calculating Stirling numbers using casework. For example, when we computed `S(6, 3)`, we considered the set partitions: `(4,1,1)`, `(3,2,1)`, and `(2,2,2)`—which correspond to the integer partitions of 6 into 3 parts. Here, each distinct partition shape represents one valid way to group the 6 identical balls into 3 identical, non-empty boxes.

Although this row may appear the most daunting at first glance, it reveals the elegant connection between the Twelvefold Way and the theory of integer partitions.

## 4. Examples

In this section, we’ll walk through a few illustrative examples to deepen our understanding of surjective cases—where each box must contain at least one ball. These examples highlight different variations and subtleties within this constraint, helping clarify how to count arrangements accurately when the surjective condition applies.

### Example 1: Dividing 6 students into 2 groups (each with at least one student)

This is a classic case of the **Stirling number of the second kind**, specifically:

$$
S(6, 2)
$$

We can alternatively compute this using casework based on the valid **partitions** of 6 into 2 positive integers:

- (5, 1)
- (4, 2)
- (3, 3)

For each partition, we count how many ways to divide the 6 **distinct** students into two **indistinguishable** groups of the specified sizes.

- For (5,1): choose 1 student to be in the small group: $\binom{6}{1} = 6$
- For (4,2): choose 2 students to be in one group: $\binom{6}{2} = 15$
- For (3,3): choose 3 students and divide by $2!$ to correct for symmetry: $\frac{\binom{6}{3}}{2!} = \frac{20}{2} = 10$

Adding these:

$$
S(6, 2) = \binom{6}{1} + \binom{6}{2} + \frac{\binom{6}{3}}{2!} = 6 + 15 + 10 = 31
$$

---

### Example 2: Dividing 6 students into a **debate** and **chess** team (each with at least one student)

Now suppose the two groups are **labeled** (e.g., one is the debate team, the other the chess team). This now becomes a case of **distributing distinct balls into distinct boxes**, with the **surjective condition** (no empty boxes), according to the **Twelvefold Way**.

We simply multiply the previous result by $2!$ to account for the labeling:

$$
\text{Total ways} = 2! \cdot S(6, 2) = 2 \cdot 31 = 62
$$

---

### Example 3: Dividing 6 students into 2 groups, each with **at least 2 students**

In this case, we can no longer use $S(6, 2)$ directly, because some partitions (like (5,1)) violate the constraint. Instead, we use casework for the valid partitions:

- (4,2): $\binom{6}{2} = 15$
- (3,3): $\frac{\binom{6}{3}}{2!} = 10$

So the total number of ways is:

$$
15 + 10 = 25
$$

---

### Example 4: Dividing 6 students into debate and chess teams, each with at least 2 students

Finally, if the two teams are **labeled**, and each must have at least 2 students, we again multiply by $2!$:

$$
2! \cdot 25 = 2 \cdot 25 = 50
$$

---

### Summary

- $S(6, 2) = 31$ — number of ways to divide into 2 indistinguishable groups (each with $\geq 1$)
- $2! \cdot S(6, 2) = 62$ — labeled groups with $\geq 1$ per group
- $25$ — indistinguishable groups with $\geq 2$ per group
- $2! \cdot 25 = 50$ — labeled groups with $\geq 2$ per group

----

## 5. Concluding Remarks

We began our exploration with a detailed discussion of the Stirling numbers of the second kind, which count the number of ways to distribute `n` distinct balls into `k` identical boxes, each with at least one ball. This served as a foundational example of surjective scenarios within the broader Twelvefold Way framework.

We then constructed a 4×3 grid that systematically organizes all twelve counting scenarios based on:
- Whether the balls are **distinct** or **identical**
- Whether the boxes are **distinct** or **identical**
- Whether there are restrictions on the distribution: **no restriction**, **injective** (at most one ball per box), or **surjective** (at least one ball per box)

To further reinforce our understanding, we walked through a few example problems focused on surjective cases, particularly where the constraint was modified (e.g., requiring at least **two** balls per box).

While the Twelvefold Way provides a powerful and elegant framework for organizing and understanding common counting problems, it is not exhaustive. Many real-world or competition problems are **variants** or **extensions** of these twelve scenarios. For example:
- Grouping students such that each group has at least **two** members
- Counting distinct arrangements of the letters in *STEVE*, where the repeated letter *E* introduces symmetry and overcounting that must be corrected

A strong command of the Twelvefold Way gives you a significant edge in tackling such problems. The key is to:
1. Carefully interpret the constraints of the problem
2. Match it to one of the twelve core scenarios
3. Identify any **variations** from the base case (e.g., additional constraints or symmetries)
4. Make appropriate adjustments, such as dividing by factorials to correct for overcounting due to identical items

With practice, you’ll be able to quickly categorize and solve most counting problems encountered in math competitions like the AMC. 

**All the best in your mathematical journey!**


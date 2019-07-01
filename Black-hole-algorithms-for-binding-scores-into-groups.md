Black hole is so heavy, it pulls matter (including light) which passed its event horizon. This natural fact inspires us very much in scores clustering problem.

Suppose we have an array of N scores, which needs to be divided into K groups whose values are similar. It's easy if the scores are uniform, we split the array into K groups of 20%.
```
# N = 10, K = 5
1 2 3 4 5 6 7 8 9 10 => {1, 2}, {3, 4}, {5, 6}, {7, 8}, {9, 10}
```

But, real world values often laid on gaussian/normal distribution or some complicated one, the equal-method won't works.

```
# bad group division
1 3 3 5 5 6 6 8 8 10 => {1, 3}, {3, 5}, {5, 6}, {6, 8}, {8, 10}
```

Result need to be more reasonable like
```
1 3 3 5 5 6 6 8 8 10 => {1}, {3, 3}, {5, 5, 6, 6}, {8, 8}, {10}
```

This post introduces a natural-based method, which is inspired by [black hole](https://en.wikipedia.org/wiki/Black_hole)'s [gravitational force](https://en.wikipedia.org/wiki/Gravity), [Kmeans](https://en.wikipedia.org/wiki/K-means_clustering) and [K-nearest neighbors](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm) algorithms to perform the result above.

## Algorithms

### Step by step

#### Step 1. Sort the array by decrease order (increase order is ok, too).

#### Step 2. Define a gravitational force function between two scores

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/06/natural_force_function.jpg)

This algorithms requires tuning two parameters
 - **F**: a threshold of **f** value. If **f >= F** then x and y will be collapsed into the same group.
 - **e**: we define **e** base on array values' distribution. **e >= 2**

This post also has a detail about [tuning F and e](https://github.com/richanchor/documentation/wiki/Black-hole-algorithms-for-binding-scores-into-groups#tuning-f-e-parameters-and-algorithms-complexity) bellow.

Note: We don't use gravitational coefficient **G** in Newton's formula because there's no Earth here :)

#### Step 3. Grouping

##### 3.0.
 - If array is empty
   - If group count = K, exit.
   - Else go to step 2. and [tuning parameters](https://github.com/richanchor/documentation/wiki/Black-hole-algorithms-for-binding-scores-into-groups#tuning-f-e-parameters-and-algorithms-complexity) **F** and **e**
 - If array is not empty, go to step **3.1.**
##### 3.1. Create a new group G, increase group count. Poll (pick and remove from array) X as first value of arrays, put X to new group. Go to step **3.2.**

##### 3.2. Pick Y as first value of arrays
 - if f(X, Y) > F, remove Y out of array and add to group G. go to step **3.2.**
 - if f(X, Y) < F, go to step **3.0.**

### Simulation for algorithms

```
# * * * * * * * * * (new group)
# # # * * * * * * * (add values)
# # # @ * * * * * * (new group)
# # # @ @ @ @ * * * (add values)
# # # @ @ @ @ & * * (new group)
# # # @ @ @ @ & & & (add values)
Groups: {# # #}, {@ @ @ @}, {& & &}
```

### Tuning (F, e) parameters and algorithms' complexity

Let's look again into the gravitational force function

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/06/natural_force_function.jpg)

We can see:
 - The higher **F** requires a greater **f**, so the less group can be divided. (strong force pull more matter, so less chance to create many of groups)
 - The higher **e** causes a weaker force, the more group can be divided. (less force pull less matter, more chance to create many of groups)

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/06/natural_force_kfe.jpg)

By these observations, we have three ways of tuning **(F, e)**, and so perform different algorithms' complexity

#### Fix F and find e

We search for value of **e** until the algorithms can divide array into K groups. Normally we fix a range, for ex: (1, 100]. **e** is a real number, so we choose small epsilon to find **e**.

So the space to find **e** is **Re = (100 - 1) / epsilon**

Complexity: **N*log(Re)**

Using this way we must ensure the range contains the value of **e** which can divide arrays into K groups.

#### Fix e and find F

This similar to method above, we search for F between the range of (max array value - min array value) / epsilon

**RF = (max value - min value) / epsilon**

Complexity: **N*log(RF)**

Using this way we must ensure the range contains the value of **F** which can divide arrays into K groups.

The result in the example from this post's introduction came from assigning fixed parameter **e = 4.0**
```
1 3 3 5 5 6 6 8 8 10 => {1}, {3, 3}, {5, 5, 6, 6}, {8, 8}, {10}
```

#### Find both F and e

This way requires us to find **e** in **Re**, for each of **e** we find **F** in **RF** until the array can be divided into K groups.

Complexity: **N*log(Re)*log(RF)**.

We recommend this third method in case we have no ideas about array's values distribution

## Application

We can use this algorithms rank our website/app/game by visits, downloads, purchases... (1 to 5 stars)

Or, like google normalizes all websites' [pagerank](https://en.wikipedia.org/wiki/PageRank) into 0 to 9. Our algorithms was also applied to site ranking at [Coccoc search](http://coccoc.com/search#!), too.

In [RFM model](https://blog.richanchor.com/2016/06/09/using-rfm-model-to-evaluate-customers-database/), we use this algorithms to divide array values into 5 groups of similar scores. It performs a better RFM model than the usual.


P/S: This is our in-garage algorithms, it's great if it could help your work. All ideas of improvement or generalization for the algorithms are welcome. ;)
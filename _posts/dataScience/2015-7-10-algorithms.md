---
layout: post
title: UnionFind
category: DataScience
tags: [algorithm, coursera]
---
  
<!-- more -->


# 1. Quick-find

### 1.1 Data structure
* Integer array id[] of length N
* Interpretation: p and q are connected iff(if and only if) they have the same id
![pic](/images/DataScience/algorithm_1.png)

### 1.2 Find
+ Check if p and q have the same id
	- id[6]=0 id[1]=1 6 and 1 are not connected

### 1.3 Union
+ Merge components containing p and q, change entries whose id equals id[p] to id[q]
	- after union 6 and 1  
![pic](/images/DataScience/algorithm_2.png)

### 1.4 Java implementation
```java
	public class QuickFindUF {
		private int[] id;

		public QuickFind(int N) {
			id = new int[N];
			for (int i = 0; i < N; i++) 
				id[i] = i;
		}

		public boolean connected(int p, int q) {
			return id[p] == id[q];
		}

		public void union(int p, int q) {
			int pid = id[p];
			int qid = id[q];
			for (int i = 0; i < id.length, i++)
				if(id[i] == pid) 
					id[i] = qid;
		}
	}
```

### 1.5 Quick-find is too slow. 
* It takes $N^2$ array accesses to process a sequence of $N$ union commands on $N$ objects.



# 2. Quick union[lazy approach]

### 2.1 Data structure
* Integer array id[] of length N
* Interpretation: id[i] is parent of i
* _Root_ of i is id[id[id[...id[i]...]]]

### 2.2 Find
+ Check if p and q have the same root

### 2.3 Union
+ To merge components containing p and q, set the id of p's root to the id of q's root.
![pic](/images/DataScience/algorithm_3.png)

### 2.4 Java implemention

```java
	public class QuickFindUF {
		private int[] id;

		public QuickFind(int N) {
			id = new int[N];
			for (int i = 0; i < N; i++) 
				id[i] = i;
		}

		public int root(int i) {
			while(i != id[i])
				i = id[i];
			return i;
		}

		public boolean connected(int p, int q) {
			return root(p) == root(q);
		}

		public void union(int p, int q) {
			int i = root(p);
			int j = root(q);
			id[i] = j;
		}
	}
```

### 2.5 Quick union is also too slow
![pic](/images/DataScience/algorithm_4.png)

Submission for Redmart Coding Challenge
---
http://geeks.redmart.com/2015/01/07/skiing-in-singapore-a-coding-diversion/

The core issue of this challenge is not actually to find what the longest path is, but rather what its length is and where does it start and end. The solution I've chosen uses a dynamic programming algorithm to solve the skiing problem in O(n) time.

Dynamic Programming equation
---

Some terms
 -Neighbour: an adjacent point (up, down, left or right) 
 -Starting point: the point with the highest value along a path (when we ski, we start high and end low)
 -Ending point: the point with the lowest value along a path
 -height(i,j): the height of the point (i,j)
 -path(i,j): the length of the **longest** path that starts at a point (i,j)
 -end(i,j): the height of the ending point of the longest path that starts at (i,j)

In the case of this map,
```
4 8 7 3
2 6 9 3
4 5 4 2
2 3 1 6

Neighbours of (0,1) are (0,0), (0,2), (1,1)
height(0,1) = 8
path(0,1) = 5
end(0,1) = 1
```

**Longest path**
The main variable we are interested in is path(i,j), which is simply max(path(x,y)) + 1, where x,y is a neighbour to (i,j). This can be easily computed by considering path(x,y). 

If (i,j) is higher than its neighbour, then there exists a path that starts at (i,j) and goes through that neighbour. The greatest possible length of this path would be the greatest length starting at the neighbour + the additional length of adding the point (i,j) to the path, i.e. a length of 1.

The formula is thus:
```
path(i,j)	= 1						if there are no lower height neighbours of (i,j)
			= max(path(x,y)) + 1 	where x,y are lower height neighbours of (i,j)
```

The longest path in the map is simply the largest path(i,j) for any valid (i,j).

**Ending point of path**
Let's assume we have found a unique longest path that starts at (i,j) and that we know that it passes through neighbour i, k. The paths starting at (i,j) and i,k would naturally have the same ending point.

The complication here exists when there isn't a unique longest path e.g. a point has two neighbours with a longest path length of 1. In this case, we simply pick the lowest of these values. 

The formula for the ending point of the longest path, end(i,j) is thus:
```
end(i,j) 	= height(i,j)		if there are no lower height neighbours of (i,j)
 			= min(end(x,y)) 	where x,y are lower height neighbours of (i,j) **and** path(i,j) == path(x,y) + 1
```

Algorithm description
---
The two functions that solve the problem are max_algorithm() and find_max().

 -find_max() finds path(i,j) and end(i,j) for a given point (i,j) recursively and stores them
 -max_algorithm() iteratively calls find_max() over all points that were missed by the recursion and tracks the length of the longest paths (globally) and their ending heights
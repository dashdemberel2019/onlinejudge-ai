# Point Placement - Problem Statement (English Translation)

## Problem

A teacher gave students an exercise to place and remove several points on a rectangular coordinate plane according to the following conditions:

- Since the coordinate plane is infinite, a specific rectangular region is specified. In other words, points are placed on the plane bounded by the lines x=1, x=2, y=-1, y=N.
- Start placing points from the top-left corner at point (1, -1).
- Place the next point as far as possible from all previously placed points. If there are multiple options, choose the topmost position first, and if still tied, choose the leftmost position.
- A new point can be placed at the coordinate location where a point with a given number was removed.
- When placing a point, at least one integer position is unmarked; when removing, at least one point is marked.

## Input

The first line contains two integers N and M (1 ≤ N ≤ 150000, 1 ≤ M ≤ 30000). The next M lines contain either '+' or '- ' followed by a point number. "+" means place a point. "–" means remove the point with that number.

## Output

M pairs of numbers representing the coordinates of the placed points.

## Example

### Input
```
3 7
+
+ 
+
- 2
+
- 1
+
```

### Output
```
1 -1
3 -2
1 -2
3 -1
1 -1
```

### Explanation

First, place a point directly at (1, -1). The farthest point from this is (3, -2). Then, since there are multiple possible options for placing, place at the top-left point (1, -2). Point 2 is removed, so place at (3, -1). Then point 1 is removed, so place at (1, -1).

## Coordinate System Mapping

**Important Note:** This problem is equivalent to the CEOI 2013 "Tram" problem with a different coordinate system:

- **Mongolian version:** x ∈ [1, N], y ∈ {-1, -2}
  - (1, -1) = top-left
  - (N, -1) = bottom-left
  - (1, -2) = top-right
  - (N, -2) = bottom-right

- **English (TRAM) version:** row ∈ [1, N], column ∈ {1, 2}
  - (1, 1) = top-left
  - (N, 1) = bottom-left
  - (1, 2) = top-right
  - (N, 2) = bottom-right

**Mapping:** x_mongolian = row_tram, y_mongolian = -column_tram

## Solution Approach

The problem is to maximize the minimum distance to occupied points. For each new point:

1. If the grid is empty, place at (1, -1) [or (1, 1) in TRAM coordinates]
2. Otherwise, for each free position:
   - Calculate the minimum distance to any occupied point
   - Track the position with the maximum such distance
   - Break ties by preferring smaller x (row), then smaller |y| (column)

Time Complexity: O(N·M²) - sufficient for 65 points in the grading scheme

## See Also

- CEOI 2013 Day 1 - Tram
- https://oj.uz/problem/view/CEOI13_tram

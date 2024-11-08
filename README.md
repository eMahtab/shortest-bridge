# Shortest Bridge
## https://leetcode.com/problems/shortest-bridge
You are given an n x n binary matrix grid where 1 represents land and 0 represents water.

An island is a 4-directionally connected group of 1's not connected to any other 1's. There are exactly two islands in grid.

You may change 0's to 1's to connect the two islands to form one island.

Return the smallest number of 0's you must flip to connect the two islands.

```
Example 1:

Input: grid = [[0,1],[1,0]]
Output: 1

Example 2:

Input: grid = [[0,1,0],[0,0,0],[0,0,1]]
Output: 2

Example 3:

Input: grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
Output: 1
``` 

## Constraints:

1. n == grid.length == grid[i].length
2. 2 <= n <= 100
3. grid[i][j] is either 0 or 1.
4. There are exactly two islands in grid.

# Implementation 1 : Using BFS try to reach any `1` of the other island (DFS+BFS)
```java
class Solution {
    public int shortestBridge(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        boolean firstIslandFound = false;
        for(int row = 0; row < rows && !firstIslandFound; row++) {
            for(int col = 0; col < cols; col++) {
                if(grid[row][col] == 1) {
                    firstIslandFound = true;
                    dfs(grid, row, col, queue);
                    break;
                }
            }
        }
        int distance = 0;
        int[][] directions = {{0, 1}, {0,-1}, {-1, 0}, {1, 0}};
        Set<String> visited = new HashSet<>();
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 1; i <= size; i++) {
                int[] pos = queue.remove();
                String location = pos[0]+","+pos[1];
                if(visited.contains(location)) continue;
                visited.add(location);
                for(int[] direction : directions) {
                    int x = pos[0] + direction[0];
                    int y = pos[1] + direction[1];
                    if(x < 0 || x >= grid.length || y < 0 || y >= grid[0].length)
                      continue;
                    if(grid[x][y] == 1)
                      return distance;  
                    else if(grid[x][y] == 0) {
                        String key = x+","+y;
                        if(!visited.contains(key))
                          queue.add(new int[]{x,y});
                    }    
                }
            }
            distance++;
        }
        return -1;
    }

    private void dfs(int[][] grid, int row, int col, Queue<int[]> queue) {
        if(row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || grid[row][col] != 1)
          return;

        grid[row][col] = 2;
        queue.add(new int[]{row, col});
        dfs(grid, row, col + 1, queue);
        dfs(grid, row, col - 1, queue);
        dfs(grid, row - 1, col, queue);
        dfs(grid, row + 1, col, queue);  
    }
}
```


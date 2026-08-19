# Rat in a Maze

## What is it?
A classic grid-backtracking problem: find (all, or one) path for a rat to travel from the top-left to bottom-right of a maze, moving only through open cells, avoiding blocked ones.

## Why it's a distinct pattern from N-Queens
N-Queens ([[Backtracking]] Pattern 6) places one item per row with global constraints (columns, diagonals). Rat in a Maze instead **moves cell-to-cell** and must track a visited-set to avoid revisiting cells in the current path — the general template for **grid/graph path-finding backtracking problems** (word search, path with obstacles, etc.).

## How it works
At each cell, try each allowed direction (typically down and right, or all 4 directions depending on the problem). If the move is valid (in bounds, not blocked, not already visited in the current path), mark it visited, recurse, then **unmark** it on the way back (backtrack) so other paths can use that cell.

```cpp
bool isSafe(vector<vector<int>>& maze, int x, int y, int n, vector<vector<bool>>& visited) {
    return x >= 0 && x < n && y >= 0 && y < n && maze[x][y] == 1 && !visited[x][y];
}

void solveMaze(vector<vector<int>>& maze, int x, int y, int n,
               vector<vector<bool>>& visited, string path, vector<string>& result) {
    if (x == n - 1 && y == n - 1) { result.push_back(path); return; }

    int dx[] = {1, 0, 0, -1};   // D, R, L, U (order matches lexicographic convention)
    int dy[] = {0, 1, -1, 0};
    string dirName = "DRLU";

    visited[x][y] = true;
    for (int i = 0; i < 4; i++) {
        int nx = x + dx[i], ny = y + dy[i];
        if (isSafe(maze, nx, ny, n, visited)) {
            solveMaze(maze, nx, ny, n, visited, path + dirName[i], result);
        }
    }
    visited[x][y] = false;   // backtrack — unmark for other paths
}
```

## Complexity
- Time: O(4^(n²)) worst case (branching factor 4 at each of n² cells) — heavily pruned in practice by `isSafe`.
- Space: O(n²) for the visited grid + recursion stack.

## When to use it
Grid traversal problems needing **all paths** or path existence under movement/obstacle constraints. Signal phrases: "find a path from start to end," "all possible paths in a grid," "rat in a maze."

## Common mistakes
- Forgetting to unmark `visited[x][y] = false` before returning — breaks the ability for other paths to reuse that cell, silently under-counting valid paths.
- Not checking grid bounds **before** checking `maze[x][y]` — causes out-of-bounds access.
- Confusing "find one path" (return `bool`, short-circuit on first success — see Backtracking Pattern 7's boolean-signal style) with "find all paths" (accumulate into `result`, don't short-circuit) — using the wrong return-type style for the actual requirement.

## Related concepts
- [[Backtracking]] — this is the grid/graph specialization of the same "choose → recurse → undo" template (Pattern 1), applied to movement instead of inclusion/exclusion.

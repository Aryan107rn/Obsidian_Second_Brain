# Recursion & Backtracking

## Concept
**Recursion:** a function solves a problem by calling itself on smaller subproblems, with a base case that stops the calls. Every recursive call gets its own stack frame holding local variables — this is why recursion has O(depth) space cost even when no data structure is explicitly used.

**Backtracking:** a refinement of recursion for exploring all possible configurations (subsets, permutations, combinations, paths) — try a choice, recurse, then **undo the choice** ("backtrack") before trying the next option. It's brute-force search with early pruning of invalid branches.

**When to apply recursion generally:** the problem can be broken into a smaller version of itself + a way to combine sub-results (divide and conquer), or the problem is naturally defined recursively (trees, nested structures).

**When to apply backtracking specifically:** need to generate/count **all** valid configurations satisfying some constraint — permutations, combinations, subsets, N-Queens, Sudoku, path-finding with constraints. Signal words: "all possible," "every arrangement," "find all solutions."

---

## Pattern 1: Basic Recursion Structure
**When to apply:** any problem with a natural smaller-subproblem decomposition.
```cpp
int factorial(int n) {
    if (n <= 1) return 1;          // base case
    return n * factorial(n - 1);   // recursive case
}
```
- Every recursive function needs: (1) a base case that terminates, (2) a recursive case that makes progress toward the base case.
- **Remember:** Forgetting the base case or not making progress toward it causes infinite recursion → stack overflow. Always identify the base case FIRST before writing the recursive case.

## Pattern 2: Recursion on Arrays/Strings (reduce problem size by 1)
**When to apply:** problems expressible as "solve for first element + recurse on the rest," e.g. sum of array, reverse a string, check palindrome.
```cpp
int sumArray(vector<int>& a, int i) {
    if (i == a.size()) return 0;              // base case: past the end
    return a[i] + sumArray(a, i + 1);          // combine current + rest
}
```
- Time: O(n), Space: O(n) call stack.
- **Remember:** This pattern directly generalizes to Linked List recursion (see [[Linked List]] reversal) — "process current node/element, recurse on rest, combine."

## Pattern 3: Backtracking Template
**When to apply:** the general skeleton for every backtracking problem — subsets, permutations, combinations, N-Queens, etc.
```cpp
void backtrack(/* state */, vector<int>& path, vector<vector<int>>& result) {
    if (/* goal reached */) {
        result.push_back(path);   // record a valid solution (copy needed!)
        return;
    }
    for (/* each choice */) {
        path.push_back(choice);        // make choice
        backtrack(/* updated state */, path, result);
        path.pop_back();               // undo choice — THE key backtracking step
    }
}
```
- **Remember:** `result.push_back(path)` copies `path` by value — necessary because `path` keeps getting mutated after this line. Pushing a reference/pointer would end up with all entries pointing to the same (eventually empty) vector.

## Pattern 4: Subsets (Power Set)
**When to apply:** generate all possible subsets of a set — "power set" problems.
```cpp
void subsets(vector<int>& nums, int idx, vector<int>& path, vector<vector<int>>& result) {
    if (idx == nums.size()) {
        result.push_back(path);
        return;
    }
    // choice 1: exclude nums[idx]
    subsets(nums, idx + 1, path, result);
    // choice 2: include nums[idx]
    path.push_back(nums[idx]);
    subsets(nums, idx + 1, path, result);
    path.pop_back();
}
```
- Time: O(2ⁿ · n) — 2ⁿ subsets, O(n) to copy each into result. Space: O(n) recursion depth.
- **Remember:** The "include or exclude" binary choice at each element is the core idea behind subset generation — the same structure appears in 0/1 Knapsack DP later.

## Pattern 5: Permutations
**When to apply:** generate all orderings of a set of elements.
```cpp
void permute(vector<int>& nums, vector<bool>& used, vector<int>& path, vector<vector<int>>& result) {
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        if (used[i]) continue;
        used[i] = true;
        path.push_back(nums[i]);
        permute(nums, used, path, result);
        path.pop_back();
        used[i] = false;              // undo — backtrack
    }
}
```
- Time: O(n! · n), Space: O(n)
- **Remember:** `used[]` array tracks which elements are already in the current path — without it, elements would repeat within a single permutation. Alternative: swap-based in-place permutation (no extra `used` array, swaps elements into position and swaps back).

## Pattern 6: Combinations / Combination Sum
**When to apply:** choose k elements from n (order doesn't matter), or find all combinations summing to a target (with or without reuse of elements).
```cpp
void combinationSum(vector<int>& candidates, int target, int idx, vector<int>& path, vector<vector<int>>& result) {
    if (target == 0) { result.push_back(path); return; }
    if (target < 0 || idx == candidates.size()) return;

    // include candidates[idx] again (reuse allowed) — stay at idx
    path.push_back(candidates[idx]);
    combinationSum(candidates, target - candidates[idx], idx, path, result);
    path.pop_back();

    // move on without using candidates[idx]
    combinationSum(candidates, target, idx + 1, path, result);
}
```
- Time: exponential (bounded by branching factor and target size), Space: O(target/min-value) recursion depth.
- **Remember:** The key difference from permutations is **not resetting idx to 0** — this avoids counting `[2,3]` and `[3,2]` as different combinations. Staying at `idx` (vs moving to `idx+1`) controls whether elements can be reused.

## Pattern 7: Pruning / Early Termination
**When to apply:** always — pruning is what makes backtracking tractable instead of pure brute force. Check constraints as early as possible instead of generating a full invalid path and discarding it at the end.
```cpp
// Bad: generate full path, check validity only at the end
// Good: check validity incrementally, stop exploring a branch the moment it's invalid
if (target < 0) return;   // prune immediately, don't keep recursing deeper
```
- **Remember:** The difference between a backtracking solution that passes and one that times out is almost always pruning quality — always ask "can I detect this branch is invalid before recursing further?"

## Pattern 8: N-Queens (Constraint Satisfaction)
**When to apply:** classic constraint-satisfaction backtracking — place N non-attacking queens on an N×N board. Represents the general pattern for grid/board placement problems with row/column/diagonal constraints.
```cpp
bool isSafe(vector<string>& board, int row, int col, int n) {
    for (int i = 0; i < row; i++) if (board[i][col] == 'Q') return false;
    for (int i = row-1, j = col-1; i >= 0 && j >= 0; i--, j--) if (board[i][j] == 'Q') return false;
    for (int i = row-1, j = col+1; i >= 0 && j < n; i--, j++) if (board[i][j] == 'Q') return false;
    return true;
}
void solveNQueens(vector<string>& board, int row, int n, vector<vector<string>>& result) {
    if (row == n) { result.push_back(board); return; }
    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 'Q';class Solution {
public:
    double power(double x,int n){
        if(n==0)return 1;
        double half=power(x,n/2);
        if(n%2==0){
            return half*half;
        }
        else{
            return x*half*half;
        }     
    }
    double myPow(double x, int n) {
        long long N=n;
        if(N<0){
            x=1/x;
            N=-N;
        }
        return power(x,N);
    }
};class Solution {
public:
    double power(double x,int n){
        if(n==0)return 1;
        double half=power(x,n/2);
        if(n%2==0){
            return half*half;
        }
        else{
            return x*half*half;
        }     
    }
    double myPow(double x, int n) {
        long long N=n;
        if(N<0){
            x=1/x;
            N=-N;
        }
        return power(x,N);
    }
};class Solution {
public:
    double power(double x,int n){
        if(n==0)return 1;
        double half=power(x,n/2);
        if(n%2==0){
            return half*half;
        }
        else{
            return x*half*half;
        }     
    }
    double myPow(double x, int n) {
        long long N=n;
        if(N<0){
            x=1/x;
            N=-N;
        }
        return power(x,N);
    }
};
            solveNQueens(board, row + 1, n, result);
            board[row][col] = '.';        // backtrack
        }
    }
}
```
- Time: O(n!) roughly (heavily pruned by `isSafe`), Space: O(n²) board + O(n) recursion.
- **Remember:** Use hashsets/bitmasks for column and both diagonals (`col`, `row-col`, `row+col`) instead of re-scanning the board in `isSafe` for an O(1) check instead of O(n) — a common optimization for larger N.

## Pattern 9: Sudoku Solver (Grid Backtracking)
**When to apply:** fill a grid respecting row/column/box constraints — same family as N-Queens but with more complex constraint checking.
- **Intuition:** find an empty cell, try digits 1-9, recurse if valid, undo if the recursive call fails to find a full solution.
- Time: worst-case exponential, but heavily pruned in practice by constraint checking.
- **Remember:** Return `true`/`false` from the recursive call (not `void`) so a filled cell that leads to a dead end can be undone and a different digit tried — this "signal success/failure back up the call stack" pattern is essential whenever backtracking needs to know if a *complete* solution was found, not just enumerate all of them.

## Pattern 10: Memoization (Recursion + Caching)
**When to apply:** recursive solution recomputes the same subproblems repeatedly (overlapping subproblems) — classic sign is exponential time recursion for a problem with only polynomially many distinct subproblem states (e.g. Fibonacci, many DP problems before they're converted to iterative DP).
```cpp
unordered_map<int,long long> memo;
long long fib(int n) {
    if (n <= 1) return n;
    if (memo.count(n)) return memo[n];
    return memo[n] = fib(n-1) + fib(n-2);
}
```
- Time: O(n) with memo vs O(2ⁿ) without. Space: O(n) for memo + recursion stack.
- **Remember:** This is the bridge from plain recursion into Dynamic Programming — "recursion + memoization" is literally top-down DP. Full DP treatment (including bottom-up tabulation) belongs in a dedicated DP note.

## Pattern 11: Recursion on Trees (preview)
**When to apply:** trees are inherently recursive structures — "a tree is a node + two subtrees" — so nearly every tree operation (traversal, height, search) is naturally recursive.
```cpp
int height(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(height(root->left), height(root->right));
}
```
- **Remember:** Full tree traversal/recursion patterns belong in a dedicated Trees note — flagged here because it's the most common real-world application of the "recurse on subproblem, combine results" idea from Pattern 2.

---

## When to apply — quick reference
- Smaller-subproblem decomposition with combine step → **Basic recursion**
- Need ALL subsets of a set → **Subsets (include/exclude)**
- Need ALL orderings of elements → **Permutations (with `used[]` tracking)**
- Need ALL combinations summing to a target (reuse allowed) → **Combination sum (stay at idx)**
- Need ALL combinations, no reuse → **Combination sum variant (move to idx+1 on include too)**
- Grid/board placement with row/col/diagonal constraints → **N-Queens style constraint backtracking**
- Need to know if *a* complete valid solution exists (not enumerate all) → **Boolean-returning backtracking (Sudoku style)**
- Recursion recomputing the same subproblem repeatedly → **Memoization (top-down DP)**
- Any solution-space exploration where invalid branches can be detected early → **Prune constraints as early as possible, always**

## Common mistakes
- Missing or unreachable base case → infinite recursion → stack overflow.
- Pushing `path` (a mutable reference/vector being reused) into `result` without copying — later mutations corrupt already-stored "solutions." Always push a **copy**.
- Forgetting to undo state (`pop_back()`, `used[i] = false`, unmark grid cell) after the recursive call returns — this breaks backtracking's core guarantee that siblings explore independently.
- Combination sum: resetting `idx` to 0 when reuse should be prevented — turns "combinations" into over-counted "permutations."
- Late validity checking (generate full invalid path, discard at the end) instead of pruning early — turns tractable backtracking into a timeout.
- Sudoku/N-Queens style problems: using `void` return type when you actually need to stop at the *first* valid solution — should return `bool` and check it after each recursive call to short-circuit further exploration.
- Deep recursion without memoization on problems with overlapping subproblems (e.g. naive Fibonacci) — exponential blowup that's trivially avoidable.

## Related concepts
[[Arrays]]
[[Strings]]
[[Linked List]]
[[CPP Complete Revision]]

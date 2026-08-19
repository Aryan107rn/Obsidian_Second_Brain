# Merge K Sorted Lists

## What is it?
Given `k` sorted linked lists (or arrays), merge them all into one sorted result. A generalization of [[Linked List]] Pattern 5 (merge two sorted lists) to many lists at once.

## Why does it exist as its own pattern?
Merging two lists at a time is O(n+m). Naively merging k lists **pairwise, one at a time** (merge list 1+2, then merge that with 3, then with 4...) costs O(n·k) total, where n is the total number of elements — inefficient when k is large. Using a **min-heap** instead brings this down to O(n log k).

## How it works (min-heap approach)
Push the head of every list into a min-heap (keyed by value). Repeatedly pop the smallest, append it to the result, and push that node's `next` (if it exists) back into the heap.

```cpp
struct Compare {
    bool operator()(Node* a, Node* b) { return a->val > b->val; }  // min-heap
};

Node* mergeKLists(vector<Node*>& lists) {
    priority_queue<Node*, vector<Node*>, Compare> pq;
    for (Node* head : lists) if (head) pq.push(head);

    Node dummy(0);
    Node* tail = &dummy;
    while (!pq.empty()) {
        Node* smallest = pq.top(); pq.pop();
        tail->next = smallest;
        tail = tail->next;
        if (smallest->next) pq.push(smallest->next);
    }
    return dummy.next;
}
```

## Alternative — Divide and Conquer
Pair up lists and merge them two at a time in rounds (like merge sort's combine step): merge (1,2), (3,4), (5,6)... then merge the results pairwise again, until one list remains. Also achieves O(n log k), without needing a heap.

## Complexity
- **Min-heap:** Time O(n log k) — n total elements, each heap push/pop is O(log k). Space O(k) for the heap.
- **Divide and conquer:** Time O(n log k), Space O(log k) recursion (or O(1) if done iteratively level by level).
- **Naive pairwise:** Time O(n·k) — avoid this for large k.

## When to use it
"Merge k sorted [lists/arrays]" — appears standalone, and as a subroutine in external sorting (merging sorted chunks too large to fit in memory) and in k-way merge variants of other problems (e.g. "smallest range covering elements from k lists").

## Common mistakes
- Using the naive pairwise-merge-everything-into-one-list approach for large k — technically correct but O(n·k), times out on large inputs.
- Forgetting to check `if (head)` before pushing a list's head into the heap — empty input lists cause null pointer issues.
- Custom comparator direction: C++'s `priority_queue` is a **max-heap by default** — the `operator()` must return `true` when `a` should come *after* `b` (i.e. `a->val > b->val` for a min-heap), which is easy to get backwards.

## Related concepts
- [[Linked List]] — Pattern 5 (Merge Two Sorted Lists) is the base case this generalizes from; Pattern 6 (Merge Sort) uses the same divide-and-conquer combine idea.

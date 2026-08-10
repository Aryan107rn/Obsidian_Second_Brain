# Strings

## Concept
A string is essentially an array of characters, so most array patterns (two pointer, sliding window, prefix) apply directly. What makes strings a distinct topic is character-frequency reasoning, pattern matching, and specialized algorithms for searching one string inside another.

**Note on C++ strings:** `std::string` is mutable and dynamically sized (like `vector<char>`), supports `+`, `substr()`, comparison operators. `s[i]` is O(1) access. `s.substr(l, len)` is O(len) — creates a new string, so calling it in a loop can blow up complexity.

---

## Pattern 1: Two Pointer (Palindrome check, reverse)
**When to apply:** checking symmetry, comparing from both ends, or in-place reversal.
```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        if (s[l] != s[r]) return false;
        l++; r--;
    }
    return true;
}
```
- Time: O(n), Space: O(1)
- **Remember:** For "valid palindrome ignoring non-alphanumeric," skip invalid chars with inner while loops on both pointers before comparing.

## Pattern 2: Sliding Window (Substring problems)
**When to apply:** "longest/shortest substring with [condition]" — no repeating characters, at most k distinct characters, contains all characters of another string, etc.
```cpp
int lengthOfLongestSubstring(string s) {   // longest substring without repeating chars
    unordered_map<char,int> lastSeen;
    int l = 0, maxLen = 0;
    for (int r = 0; r < s.size(); r++) {
        if (lastSeen.count(s[r]) && lastSeen[s[r]] >= l)
            l = lastSeen[s[r]] + 1;         // jump window start past the duplicate
        lastSeen[s[r]] = r;
        maxLen = max(maxLen, r - l + 1);
    }
    return maxLen;
}
```
- Time: O(n), Space: O(min(n, charset size))
- **Remember:** Jumping `l` directly to `lastSeen[s[r]] + 1` (instead of incrementing one at a time) is what keeps this O(n) instead of O(n²).

## Pattern 3: Frequency Counting / Hashing (Anagrams)
**When to apply:** anagram checks, "group anagrams," character frequency comparisons.
```cpp
bool isAnagram(string s, string t) {
    if (s.size() != t.size()) return false;
    vector<int> count(26, 0);
    for (char c : s) count[c - 'a']++;
    for (char c : t) count[c - 'a']--;
    return all_of(count.begin(), count.end(), [](int x){ return x == 0; });
}
```
- Time: O(n), Space: O(26) = O(1) for lowercase-only alphabet.
- **Remember:** A sorted-string as a hashmap key (`sort(s.begin(), s.end())`) is the classic trick for "group anagrams" — anagrams sort to the same string.

## Pattern 4: KMP Algorithm (Pattern Matching)
**When to apply:** find all occurrences of a pattern P in text T efficiently — naive substring search is O(n·m), KMP achieves O(n+m) by avoiding re-checking characters that are guaranteed to match.
**Intuition:** Precompute a "longest proper prefix that is also a suffix" (LPS) array for the pattern. On a mismatch, instead of restarting the pattern from index 0, jump using the LPS array — part of the text already matched part of the pattern, so redundant comparisons can be skipped.
```cpp
vector<int> computeLPS(string& pat) {
    int n = pat.size();
    vector<int> lps(n, 0);
    int len = 0, i = 1;
    while (i < n) {
        if (pat[i] == pat[len]) lps[i++] = ++len;
        else if (len) len = lps[len - 1];
        else lps[i++] = 0;
    }
    return lps;
}
vector<int> kmpSearch(string& text, string& pat) {
    vector<int> lps = computeLPS(pat);
    vector<int> matches;
    int i = 0, j = 0;
    while (i < text.size()) {
        if (text[i] == pat[j]) { i++; j++; }
        if (j == pat.size()) { matches.push_back(i - j); j = lps[j - 1]; }
        else if (i < text.size() && text[i] != pat[j]) {
            j ? j = lps[j - 1] : i++;
        }
    }
    return matches;
}
```
- Time: O(n + m), Space: O(m) for LPS array.
- **Remember:** The LPS array is the entire trick — "how much of the pattern's prefix can be reused" on a mismatch instead of restarting. This idea (failure function) reappears in the Z-algorithm and Aho-Corasick.

## Pattern 5: Z-Algorithm (Pattern Matching alternative)
**When to apply:** same use case as KMP (pattern matching, finding all occurrences), sometimes preferred for its simpler array meaning, and useful beyond matching too.
**Intuition:** Build a Z-array where `Z[i]` = length of the longest substring starting at i that matches a prefix of the string. To search for pattern P in text T, form `P + '#' + T` and look for positions where `Z[i] == len(P)`.
- Time: O(n+m), Space: O(n+m)
- **Remember:** Z-array gives prefix-match lengths at every position directly — also useful for counting distinct substrings or finding the shortest repeating unit of a string.

## Pattern 6: Rabin-Karp (Rolling Hash)
**When to apply:** pattern matching where average O(n+m) via hashing is preferred over KMP's failure function — generalizes well to multi-pattern search and 2D pattern matching.
**Intuition:** Compute a hash for the pattern and for each equal-length window of text. Compare hashes first (O(1) after rolling update); only do a full character comparison on a hash match (guards against collisions).
```cpp
bool rabinKarp(string& text, string& pat) {
    int n = text.size(), m = pat.size();
    if (m > n) return false;
    const int base = 256, mod = 1e9 + 7;
    long long patHash = 0, textHash = 0, h = 1;
    for (int i = 0; i < m - 1; i++) h = (h * base) % mod;
    for (int i = 0; i < m; i++) {
        patHash = (base * patHash + pat[i]) % mod;
        textHash = (base * textHash + text[i]) % mod;
    }
    for (int i = 0; i <= n - m; i++) {
        if (patHash == textHash && text.substr(i, m) == pat) return true; // verify to avoid collision false-positive
        if (i < n - m) {
            textHash = (base * (textHash - text[i] * h) + text[i + m]) % mod;
            if (textHash < 0) textHash += mod;
        }
    }
    return false;
}
```
- Time: O(n+m) average, O(n·m) worst case (many hash collisions).
- **Remember:** Always verify with a direct character comparison when hashes match — silently trusting a hash match is a common bug source.

## Pattern 7: Trie (Prefix Tree)
**When to apply:** need fast prefix-based operations — autocomplete, "does any word start with prefix X," dictionary search, longest common prefix among many strings.
**Intuition:** Tree where each edge is a character; root-to-marked-node path spells a stored word. Shared prefixes share the same path, so lookups/prefix-checks cost O(word length), independent of how many words are stored.
```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    bool isEnd = false;
};
class Trie {
    TrieNode* root = new TrieNode();
public:
    void insert(string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c - 'a']) node->children[c - 'a'] = new TrieNode();
            node = node->children[c - 'a'];
        }
        node->isEnd = true;
    }
    bool search(string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c - 'a']) return false;
            node = node->children[c - 'a'];
        }
        return node->isEnd;
    }
    bool startsWith(string& prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (!node->children[c - 'a']) return false;
            node = node->children[c - 'a'];
        }
        return true;
    }
};
```
- Time: O(L) per operation (L = word/prefix length), Space: O(total characters across all inserted words) worst case.
- **Remember:** Trie beats a hashset of strings when **prefix** queries are needed — a hashset only supports exact-match, not "does anything start with this."

## Pattern 8: Longest Common Subsequence / Substring (bridge to DP)
**When to apply:** comparing two strings for shared structure — diff tools, DNA comparison, edit-distance-adjacent problems.
- LCS (subsequence, not necessarily contiguous): O(n·m) DP.
- Longest common **substring** (contiguous) uses a similar DP table but resets to 0 on mismatch instead of carrying forward.
- **Remember:** Fundamentally a string-comparison pattern, but the full recurrence/code belongs in a dedicated DP note — flagged here for connection purposes.

## Pattern 9: String Compression / Run-Length Encoding
**When to apply:** compress consecutive repeated characters into count+char form, or decompress back — e.g. `"aaabbc"` → `"a3b2c1"`.
```cpp
string compress(string s) {
    string res;
    int n = s.size(), i = 0;
    while (i < n) {
        char c = s[i]; int count = 0;
        while (i < n && s[i] == c) { i++; count++; }
        res += c + to_string(count);
    }
    return res;
}
```
- Time: O(n), Space: O(n) worst case.
- **Remember:** Always compare compressed length to original — for strings with no repeats, "compression" makes the string longer.

## Pattern 10: Anagram Grouping / Sorting-Based Grouping
**When to apply:** group multiple strings by whether they're anagrams of each other.
```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    for (string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        groups[key].push_back(s);
    }
    vector<vector<string>> res;
    for (auto& [k, v] : groups) res.push_back(v);
    return res;
}
```
- Time: O(n·k log k), n = number of strings, k = max string length.
- **Remember:** A character-count tuple (26 ints) as key is faster (O(k) vs O(k log k) per string) than sorting, when the alphabet is small and fixed.

---

## When to apply — quick reference
- Symmetry check / in-place reversal → **Two pointer**
- Longest/shortest substring under a condition → **Sliding window**
- Anagram check, char-frequency comparison → **Frequency counting (array of 26)**
- Find pattern occurrences in text, O(n+m) guaranteed → **KMP**
- Pattern matching / prefix-length analysis → **Z-algorithm**
- Pattern matching with hashing, multi-pattern search → **Rabin-Karp**
- Prefix queries (autocomplete, "starts with") → **Trie**
- Compare structural similarity of two strings → **LCS / DP** (see DP notes)
- Compress consecutive repeats → **Run-length encoding**
- Group strings by anagram relationship → **Sort-as-key hashing**

## Common mistakes
- Using `substr()` inside a loop — each call is O(len), turning intended O(n) into O(n²).
- Sliding window off-by-one: not jumping the window-start pointer correctly on a duplicate (jump to `lastSeen+1`, don't just increment `l` once).
- KMP: mis-indexing the LPS array — `lps[i]` represents longest proper prefix-suffix for `pat[0..i]`, easy to get subtly wrong.
- Rabin-Karp: skipping the verification step after a hash match — leads to false positives on collision.
- Trie: forgetting to mark `isEnd` correctly — a prefix existing doesn't mean the full word was inserted.
- Assuming ASCII/lowercase-only when the problem allows uppercase, digits, or Unicode — frequency arrays sized 26 break silently.

## Related concepts
[[Arrays]]
[[Binary Search]]
[[CPP Complete Revision]]


# 70. Design Add and Search Words Data Structure

## Problem Statement

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

- `WordDictionary()` Initializes the object.
- `void addWord(word)` Adds `word` to the data structure, it can be matched later.
- `boolean search(word)` Returns `true` if `word` is in the data structure, otherwise `false`. `word` may contain dots `.` where dots can be matched with any letter.

**Example:**

**Input:**
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b..d"]]
**Output:**
[null,null,null,null,false,true,true,true]

**Explanation:**
```
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

## Solution

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        curr = self.root
        for char in word:
            if char not in curr.children:
                curr.children[char] = TrieNode()
            curr = curr.children[char]
        curr.is_end_of_word = True

    def search(self, word: str) -> bool:
        def dfs(j, node):
            curr = node
            for i in range(j, len(word)):
                char = word[i]
                if char == '.':
                    # If it's a dot, try all possible children
                    for child in curr.children.values():
                        if dfs(i + 1, child):
                            return True
                    return False # No child path led to a match
                else:
                    # If it's a regular character, proceed normally
                    if char not in curr.children:
                        return False
                    curr = curr.children[char]
            return curr.is_end_of_word

        return dfs(0, self.root)
```

## Explanation

This problem extends the basic Trie (prefix tree) data structure to support searching with wildcard characters (`.`).

**`TrieNode` Class:** (Same as in "Implement Trie")

-   `children`: A dictionary mapping characters to child `TrieNode`s.
-   `is_end_of_word`: A boolean indicating if a word ends at this node.

**`WordDictionary` Class Methods:**

1.  **`__init__(self)`:** Initializes the trie with a `root` node.

2.  **`addWord(self, word)`:** (Same as `insert` in "Implement Trie")
    -   Traverses the trie, creating new nodes as needed for characters in the `word`.
    -   Marks the last node as `is_end_of_word = True`.

3.  **`search(self, word)`:** This is where the main difference lies. It uses a recursive Depth-First Search (DFS) to handle the wildcard character `.`.
    -   **`dfs(j, node)` Function:**
        -   `j`: The current index in the `word` we are trying to match.
        -   `node`: The current `TrieNode` we are at in the trie.

    -   **Iteration:** The `dfs` function iterates from index `j` to the end of the `word`.

    -   **Handling `.` (Wildcard):** If `char == '.'`:
        -   This means the dot can match any character. So, we need to recursively call `dfs` for *all* children of the current `node`.
        -   If any of these recursive calls return `True` (meaning a match was found down that path), then we return `True` immediately.
        -   If none of the children paths lead to a match, we return `False`.

    -   **Handling Regular Characters:** If `char` is a regular letter:
        -   Check if `char` exists as a child of the current `node`. If not, no match is possible, return `False`.
        -   Move `curr` to the corresponding child node.

    -   **Base Case for `dfs`:** After the loop finishes (meaning we have processed all characters in the `word` from index `j` onwards), we return `curr.is_end_of_word`. This ensures that we only consider a match if a complete word ends at the current trie node.

    -   The initial call to `search` starts the DFS from index 0 and the `root` node.

**Time and Space Complexity:**

-   **`addWord`:** O(L), where L is the length of the word.
-   **`search`:** In the worst case (e.g., a word like `...` where all characters are wildcards), the search can visit many paths. The time complexity can be roughly O(26^L) in the worst case, but on average, it's much faster. More precisely, it's O(L * 26) in the worst case for a single search, where L is the length of the word. The total time complexity depends on the number of words and search queries.
-   **Space Complexity:** O(Total Characters), similar to a regular Trie.

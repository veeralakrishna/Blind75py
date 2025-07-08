
# 69. Implement Trie (Prefix Tree)

## Problem Statement

A trie (pronounced "try") or prefix tree is a tree data structure used to efficiently retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the `Trie` class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string that has the prefix `prefix`, and `false` otherwise.

**Example 1:**

**Input:**
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
**Output:**
[null, null, true, false, true, null, true]

**Explanation:**
```
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");     
trie.search("app");     // return True
```

## Solution

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        curr = self.root
        for char in word:
            if char not in curr.children:
                curr.children[char] = TrieNode()
            curr = curr.children[char]
        curr.is_end_of_word = True

    def search(self, word: str) -> bool:
        curr = self.root
        for char in word:
            if char not in curr.children:
                return False
            curr = curr.children[char]
        return curr.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        curr = self.root
        for char in prefix:
            if char not in curr.children:
                return False
            curr = curr.children[char]
        return True
```

## Explanation

A Trie (prefix tree) is a tree-like data structure where each node represents a character, and paths from the root to a node represent prefixes. Each node can have multiple children, typically one for each possible character.

**`TrieNode` Class:**

-   `children`: A dictionary (or hash map) that maps a character to its corresponding child `TrieNode`. This allows for efficient lookup of the next character in a path.
-   `is_end_of_word`: A boolean flag that is `True` if the path from the root to this node represents a complete word that was inserted into the trie.

**`Trie` Class Methods:**

1.  **`__init__(self)`:** Initializes the trie with a `root` node, which is an empty `TrieNode`.

2.  **`insert(self, word)`:**
    -   Start from the `root`.
    -   For each `char` in the `word`:
        -   If the `char` is not a child of the current node, create a new `TrieNode` for it and add it to `curr.children`.
        -   Move `curr` to this child node.
    -   After processing all characters, set `curr.is_end_of_word = True` to mark the end of the inserted word.

3.  **`search(self, word)`:**
    -   Start from the `root`.
    -   For each `char` in the `word`:
        -   If the `char` is not a child of the current node, the word does not exist in the trie. Return `False`.
        -   Move `curr` to this child node.
    -   After traversing all characters, return `curr.is_end_of_word`. This ensures that we only return `True` if the entire word was inserted, not just a prefix.

4.  **`startsWith(self, prefix)`:**
    -   Start from the `root`.
    -   For each `char` in the `prefix`:
        -   If the `char` is not a child of the current node, the prefix does not exist in the trie. Return `False`.
        -   Move `curr` to this child node.
    -   If the loop completes, it means the entire `prefix` exists in the trie. Return `True`.

**Time and Space Complexity:**

-   **`insert`:** O(L), where L is the length of the word. We traverse the trie once for each character.
-   **`search`:** O(L), where L is the length of the word. We traverse the trie once for each character.
-   **`startsWith`:** O(L), where L is the length of the prefix. We traverse the trie once for each character.
-   **Space Complexity:** O(Total Characters), where Total Characters is the sum of the lengths of all words inserted into the trie. In the worst case, each character creates a new node.

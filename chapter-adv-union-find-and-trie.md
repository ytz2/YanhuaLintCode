## 323. Number of Connected Components in an Undirected Graph

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there iGiven`n`nodes labeled from`0`to`n - 1`and a list of undirected edges \(each edge is a pair of nodes\), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```
Input: 
n = 5
 and 
edges = [[0, 1], [1, 2], [3, 4]]


     0          3
     |          |
     1 --- 2    4 


Output: 
2
```

**Example 2:**

```
Input: 
n = 5
 and 
edges = [[0, 1], [1, 2], [2, 3], [3, 4]]


     0           4
     |           |
     1 --- 2 --- 3


Output:  
1
```

**Note:**  
You can assume that no duplicate edges will appear in`edges`. Since all edges are undirected,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in`edges`.

[https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

看到这种图联通不连通就想到UF了， 这里是UF的一个变种，找有几个subset.维护一个n就可以了，最开始为n个，每连一个就--

另外还有一个考察的地方可能是query每个subset的size,其实就是维护一个size\[n\] 每次connect的时候。 应该说这道题的变种是数岛II。

father\[i\] = j

size\[j\] += size\[i\]就可以了

这样query的时候找到所有father是自己的，然后去query他的size就是subset size

```cpp
class Solution {
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        if (edges.empty() || n <= 1) return n;
        vector<int> parent(n, 0);
        for (auto i = 0; i < n; i++) parent[i] = i;
        for (auto& each : edges) {
            if (Union(each[0], each[1], parent)) n--;
        }
        return n;
    }

    int Find(int i, vector<int>& parent) {
        if (parent[i] != i) {
            parent[i] = Find(parent[i], parent);
        }
        return parent[i];
    }

    bool Union(int i, int j, vector<int>& parent) {
        auto pi = Find(i, parent);
        auto pj = Find(j, parent);
        if (pi == pj) return false;
        parent[pi] = pj;
        return true;
    }
};
```

## 684. Redundant Connection

In this problem, a tree is an**undirected**graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes \(with distinct values 1, 2, ..., N\), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of`edges`. Each element of`edges`is a pair`[u, v]`with`u < v`, that represents an**undirected**edge connecting nodes`u`and`v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge`[u, v]`should be in the same format, with`u < v`.

**Example 1:**

```
Input:
 [[1,2], [1,3], [2,3]]

Output:
 [2,3]

Explanation:
 The given undirected graph will be like this:
  1
 / \
2 - 3
```

**Example 2:**

```
Input:
 [[1,2], [2,3], [3,4], [1,4], [1,5]]

Output:
 [1,4]

Explanation:
 The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

**Note:**

The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

**Update \(2017-09-26\):**  
We have overhauled the problem description + test cases and specified clearly the graph is an**undirected**graph. For the**directed**graph follow up please see[**Redundant Connection II**](https://leetcode.com/problems/redundant-connection-ii/description/)\). We apologize for any inconvenience caused.

这道题的类比是valid graph tree.

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> parent(edges.size() + 1, 0);
        for (int i = 0; i < parent.size(); i++) parent[i] = i;
        for (auto& each : edges) {
            if (!connect(each[0], each[1], parent) ) return each;
        }
        return {};
    }

    int find(int i, vector<int>& parent) {
        if (parent[i] != i) {
            parent[i] = find(parent[i], parent);
        }
        return parent[i];
    }

    bool connect(int i, int j, vector<int>& parent) {
        auto pi = find(i, parent);
        auto pj = find(j, parent);
        if (pi == pj) return false;
        parent[pi] = pj;
        return true;
    }
};
```

## 685. Redundant Connection II

In this problem, a rooted tree is a**directed**graph such that, there is exactly one node \(the root\) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes \(with distinct values 1, 2, ..., N\), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of`edges`. Each element of`edges`is a pair`[u, v]`that represents a**directed**edge connecting nodes`u`and`v`, where`u`is a parent of child`v`.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

**Example 1:**

```
Input:
 [[1,2], [1,3], [2,3]]

Output:
 [2,3]

Explanation:
 The given directed graph will be like this:
  1
 / \
v   v
2-->3
```

**Note:**

The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

有向图和无向图不一样，有向图还要找到变成树的结构有两种情况

第一种情况： 无环： 退化成无向图UF查找， 比如例子

第二种情况： 有环： 一个节点有两个parent,那么找出构造这两个parent的边作为candidate, 删掉其中一条， UF查找，如果还是有环，那么肯定就是没删掉的另外一条边， 或者UF找到的边。 如果没环了，说明就是被删掉那条边了

![](/assets/685.png)

```cpp
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n+1, 0);
        for (auto i = 0; i < n+1; i++) {
            parent[i] = i;
        }
        vector<int> father(n+1, 0);
        vector<int> p1, p2;
        for (auto& edge : edges) {
            if (father[edge[1]] == 0) {
                father[edge[1]] = edge[0];
                continue;
            }
            p1 = edge;
            p2 = {father[edge[1]], edge[1]};
            edge.clear();
        }

        for (const auto& edge : edges) {
            if (edge.empty()) continue;
            if (!connect(edge[0], edge[1], parent)) {
                return p2.empty() ? edge : p2;
            }
        }
        return p1;
    }

    int find(int i, vector<int>& parent) {
        if (parent[i] != i) {
            parent[i] = find(parent[i], parent);
        }
        return parent[i];
    }

    bool connect(int i, int j, vector<int>& parent) {
        auto pi = find(i, parent);
        auto pj = find(j, parent);
        if (pi == pj) return false;
        parent[pi] = pj;
        return true;
    }

};
```

## 721. Accounts Merge

Given a list`accounts`, each element`accounts[i]`is a list of strings, where the first element`accounts[i][0]`is aname, and the rest of the elements areemailsrepresenting emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails**in sorted order**. The accounts themselves can be returned in any order.

两个启示：

UnionFind  可以用string link string

merger类的题目可以考虑UF

剩下的就是堆代码了

**Example 1:**

```
Input:

accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]

Output:
 [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]

Explanation:

The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

```cpp
class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        unordered_map<string, pair<int,int>> parent;
        for (int i = 0; i < accounts.size(); i++) {
            for (int j = 1; j < accounts[i].size(); j++) {
                if (parent.count(accounts[i][j])) continue;
                parent[accounts[i][j]] = make_pair(i, j);
            }
        }
        for (int i = 0; i < accounts.size(); i++) {
            for (int j = 2; j < accounts[i].size(); j++) {
                connect(accounts[i][1], accounts[i][j], parent, accounts);
            }
        }
        unordered_map<string, vector<string>> hash;
        for (auto& each : parent) {
            const auto ppos = Find(each.first, parent, accounts);
            const auto& par = accounts[ppos.first][ppos.second];
            if (!hash.count(par))
                hash[par] = {accounts[ppos.first][0]};
            hash[par].push_back(each.first);
        }
        vector<vector<string>> res;
        for (auto& each : hash) {
            sort(each.second.begin()+1, each.second.end());
            res.push_back(each.second);
        }
        return res;
    }

    pair<int, int> Find(const std::string& val,  unordered_map<string, pair<int,int>>& parent, const vector<vector<string>>& accounts) {
        const auto& pos = parent[val];
        const auto& pstr = accounts[pos.first][pos.second];
        if ( pstr != val) {
            parent[val] = Find(pstr, parent, accounts);
        }
        return parent[val];
    }

    void connect(const std::string& l, const std::string& r,  unordered_map<string, pair<int,int>>& parent, const vector<vector<string>>& accounts) {
        auto pl = Find(l, parent, accounts);
        auto pr = Find(r, parent, accounts);
        if (pl == pr) return;
        const auto& plstr = accounts[pl.first][pl.second];
        parent[plstr] = pr;
    }
};
```

## 773. Sentence Similarity II

Given two sentences`words1, words2`\(each represented as an array of strings\), and a list of similar word pairs`pairs`, determine if two sentences are similar.

For example,`words1 = ["great", "acting", "skills"]`and`words2 = ["fine", "drama", "talent"]`are similar, if the similar word pairs are`pairs = [["great", "good"], ["fine", "good"], ["acting","drama"], ["skills","talent"]]`.

Note that the similarity relation**is**transitive. For example, if "great" and "good" are similar, and "fine" and "good" are similar, then "great" and "fine"**are similar**.

Similarity is also symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.

Also, a word is always similar with itself. For example, the sentences`words1 = ["great"], words2 = ["great"], pairs = []`are similar, even though there are no specified similar word pairs.

Finally, sentences can only be similar if they have the same number of words. So a sentence like`words1 = ["great"]`can never be similar to`words2 = ["doubleplus","good"]`.

[https://leetcode.com/problems/sentence-similarity-ii/](https://leetcode.com/problems/sentence-similarity-ii/)

```cpp
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<vector<string>>& pairs) {
        if (words1.size() != words2.size()) return false;
        unordered_map<string, string> parent;
        for (const auto& each : words1) parent[each] = each;
        for (const auto& each : words2) parent[each] = each;
        for (auto& each : pairs) connect(each[0], each[1], parent);
        for (int i = 0; i < words1.size(); i++) {
            if (find(words1[i], parent) != find(words2[i], parent)) return false;
        }
        return true;
    }

    string find(const string& str,  unordered_map<string, string>& parent) {
        if (parent[str] != str) {
            parent[str] = find(parent[str], parent);
        }
        return parent[str];
    }

    void connect(const string& l, const string& r, unordered_map<string, string>& parent) {
        auto pl = find(l, parent);
        auto pr = find(r, parent);
        if (pl == pr) return;
        parent[pl] = pr;
    }
};
```

## TRIE

## 211. Add and Search Word - Data structure design

Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```

search\(word\) can search a literal word or a regular expression string containing only letters`a-z`or`.`. A`.`means it can represent any one letter.

**Example:**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -
>
 false
search("bad") -
>
 true
search(".ad") -
>
 true
search("b..") -
>
 true
```

**Note:**  
You may assume that all words are consist of lowercase letters`a-z`.

```cpp
class WordDictionary {
public:
    /** Initialize your data structure here. */
    WordDictionary() {
        
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        auto cur = &root;
        for (auto c : word) {
            if (nullptr == cur->children[c- 'a']) 
                cur->children[c - 'a'] = new Node;
            cur = cur->children[c - 'a'];
        }
        cur->isWord = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        return search(word, 0, &root);
    }
    
private:
    struct Node {
        bool isWord = false;
        vector<Node*> children = vector<Node*>(26, nullptr);
        Node() = default;
        ~Node() {
            for(auto each : children) {
                if (each) delete each;
            }
        }
    };
    Node root;    
private:
    
    bool search(const string& word, int pos, Node*  cur) {
        if (!cur) return false;
        if (pos >= word.size()) return false;
        auto c = word[pos];
        bool isLast = pos == word.size() - 1;
        if (c == '.') {
            for (auto each : cur->children) {
                if (isLast && each && each->isWord) return true;
                if (!isLast && each && search(word, pos+1, each)) return true;
            }
            return false;
        }
        auto node = cur->children[c - 'a'];
        if (!node) return false;
        return isLast? node->isWord : search(word, pos + 1, node);
    }
    

};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```

## 642. Design Search Autocomplete System

Design a search autocomplete system for a search engine. Users may input a sentence \(at least one word and end with a special character`'#'`\). For**each character**they type**except '\#'**, you need to return the**top 3**historical hot sentences that have prefix the same as the part of sentence already typed. Here are the specific rules:

1. The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.
2. The returned top 3 hot sentences should be sorted by hot degree \(The first is the hottest one\). If several sentences have the same degree of hot, you need to use ASCII-code order \(smaller one appears first\).
3. If less than 3 hot sentences exist, then just return as many as you can.
4. When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.

Your job is to implement the following functions:

The constructor function:

`AutocompleteSystem(String[] sentences, int[] times):`This is the constructor. The input is**historical data**.`Sentences`is a string array consists of previously typed sentences.`Times`is the corresponding times a sentence has been typed. Your system should record these historical data.

Now, the user wants to input a new sentence. The following function will provide the next character the user types:

`List<String> input(char c):`The input`c`is the next character typed by the user. The character will only be lower-case letters \(`'a'`to`'z'`\), blank space \(`' '`\) or a special character \(`'#'`\). Also, the previously typed sentence should be recorded in your system. The output will be the**top 3**historical hot sentences that have prefix the same as the part of sentence already typed.

**Example:**  
**Operation:**AutocompleteSystem\(\["i love you", "island","ironman", "i love leetcode"\], \[5,3,2,2\]\)  
The system have already tracked down the following sentences and their corresponding times:  
`"i love you"`:`5`times  
`"island"`:`3`times  
`"ironman"`:`2`times  
`"i love leetcode"`:`2`times  
Now, the user begins another search:

**Operation:**input\('i'\)  
**Output:**\["i love you", "island","i love leetcode"\]  
**Explanation:**  
There are four sentences that have prefix`"i"`. Among them, "ironman" and "i love leetcode" have same hot degree. Since`' '`has ASCII code 32 and`'r'`has ASCII code 114, "i love leetcode" should be in front of "ironman". Also we only need to output top 3 hot sentences, so "ironman" will be ignored.

**Operation:**input\(' '\)  
**Output:**\["i love you","i love leetcode"\]  
**Explanation:**  
There are only two sentences that have prefix`"i "`.

**Operation:**input\('a'\)  
**Output:**\[\]  
**Explanation:**  
There are no sentences that have prefix`"i a"`.

**Operation:**input\('\#'\)  
**Output:**\[\]  
**Explanation:**  
The user finished the input, the sentence`"i a"`should be saved as a historical sentence in system. And the following input will be counted as a new search.

**Note:**

1. The input sentence will always start with a letter and end with '\#', and only one blank space will exist between two words.
2. The number of
   **complete sentences**
   that to be searched won't exceed 100. The length of each sentence including those in the historical data won't exceed 100.
3. Please use double-quote instead of single-quote when you write test cases even for a character input.
4. Please remember to
   **RESET**
   your class variables declared in class AutocompleteSystem, as static/class variables are
   **persisted across multiple test cases**
   . Please see
   [here](https://leetcode.com/faq/#different-output)
   for more details.

两段代码要注意

1. 如何用lambda做 set 或者map的comparator

   set&lt;string, std::function&lt;bool\(const string& left,const  string& right\)&gt;&gt; top3{

   ```
    \[this\]\(const string& left,const  string& right\)

    {

      if \(freq\[left\] == freq\[right\]\)

          return left &lt; right;

      return freq\[left\] &gt; freq\[right\];

    }
   ```

   };

2 算TOPK的时候，要先删掉这个str再++, 然后再添加。 如果直接++的话会break二分查找

```
void update\(const string& str\)

{   



    if \(top3.count\(str\)\)

        top3.erase\(str\);

    freq\[str\]++;

    top3.insert\(str\);

    if \(top3.size\(\) &gt; 3\)

        top3.erase\(\*top3.rbegin\(\)\);



}
```

```cpp
#include <functional>
using namespace std;
class TrieNode{
public:


    TrieNode()
    {

    }

    ~TrieNode()
    {

    }

    void update(const string& str, int times)
    {
        freq[str] = times;
        top3.insert(str);
        if (top3.size() > 3)
            top3.erase(*top3.rbegin());
    }

    void update(const string& str)
    {   

        if (top3.count(str))
            top3.erase(str);
        freq[str]++;
        top3.insert(str);
        if (top3.size() > 3)
            top3.erase(*top3.rbegin());

    }

    vector<string> topThree()
    {
        vector<string> res(top3.begin(), top3.end());
        return res;
    }

    unordered_map<char, TrieNode*> children;
    unordered_map<string ,int> freq;
    set<string, std::function<bool(const string& left,const  string& right)>> top3{
        [this](const string& left,const  string& right)
        {
          if (freq[left] == freq[right])
              return left < right;
          return freq[left] > freq[right];
        }
    };

};



class AutocompleteSystem {
public:
    AutocompleteSystem(vector<string> sentences, vector<int> times) {
        root = new TrieNode();
        cur = root;
        for (int i = 0; i < sentences.size(); i++)
        {
            auto& sentence = sentences[i];
            int time = times[i];
            auto p = root;
            for (auto c : sentence)
            {
                if (!p->children.count(c))
                    p->children[c] = new TrieNode();
                p = p->children[c];
                p->update(sentence, time);
            }
        }
    }

    vector<string> input(char c) {
        if (c == '#')
        {
            auto p = root;
            for (auto c : typed)
            {
                if (!p->children.count(c))
                    p->children[c] = new TrieNode();
                p = p->children[c];
                p->update(typed);
            }
            typed.clear();
            cur = root;
            return {};
        }
        typed.push_back(c);
        if (!cur->children.count(c))
            cur->children[c] = new TrieNode;
        cur = cur->children[c];
        return cur->topThree();
    }
    string typed;
    TrieNode *root;
    TrieNode *cur;
};
```

## 425. Word Squares

Given a set of words**\(without duplicates\)**, find all[word squares](https://en.wikipedia.org/wiki/Word_square)you can build from them.

A sequence of words forms a valid word square if thekthrow and column read the exact same string, where 0 ≤k&lt; max\(numRows, numColumns\).

For example, the word sequence`["ball","area","lead","lady"]`forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```

**Note:**

1. There are at least 1 and at most 1000 words.
2. All words will have the exact same length.
3. Word length is at least 1 and at most 5.
4. Each word contains only lowercase English alphabet
   `a-z`
   .

**Example 1:**

```
Input:

["area","lead","wall","lady","ball"]


Output:

[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]


Explanation:

The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

**Example 2:**

```
Input:

["abat","baba","atan","atal"]


Output:

[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]


Explanation:

The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

犯了好几个错误：

1. 这个是可以重复利用的
2. Trie是可以放所有startswith的，直接遍历当前startswith就可以了。

---

```cpp
struct TrieNode{

    TrieNode()
        :children(vector<TrieNode*>(26, nullptr))
        {}

    ~TrieNode()
    {
        for (auto p : children)
            if (p) delete p;
    }
    vector<TrieNode*> children;
    vector<string> words;
};

class Trie{
public:

    Trie(unordered_set<string>& words)
    {
        root = new TrieNode();
        for (auto word: words)
        {
            auto p = root;
            p->words.push_back(word);
            for (auto c : word)
            {
                int i = c-'a';
                if (!p->children[i])
                    p->children[i] = new TrieNode();
                p = p->children[i];
                p->words.push_back(word);
            }
        }
    }

    vector<string> startWith(const std::string& str)
    {
        static vector<string> dummy;
        auto p = root;
        for (auto c: str)
        {
            int i = c-'a';
            if (!p->children[i])
                return dummy;
            p = p->children[i];
        }
        return p->words;
    }

    ~Trie(){
        if (root)
            delete root;
    }
    TrieNode* root;
};


class Solution {
public:
    vector<vector<string>> wordSquares(vector<string>& words) {
        unordered_set<string> sets(words.begin(), words.end());
        Trie trie(sets);
        vector<vector<string>> results;
        vector<string> solution;
        helper(trie, sets, results, solution, words[0].size());
        return results;
    }

    void helper(Trie& trie, unordered_set<string>& collection, vector<vector<string>>& results, vector<string>& solution, int size)
    {
        if (solution.size() == size)
        {
            results.push_back(solution);
            return;
        }

        string startWith;
        for (int i = 0; i < solution.size(); i++)
            startWith.push_back(solution[i][solution.size()]);

        auto candidates = trie.startWith(startWith);

        for (auto each : candidates)
        {
            solution.push_back(each);
            helper(trie, collection, results, solution,  size);
            solution.pop_back();
        }
    }

};
```

## 




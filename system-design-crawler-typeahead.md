# Crawler

# Scenario

Download WWW in 1 week

Assumption: 1 Trillion web page

```
                  100k per page

                  Download in one week
```

| QPS | 1e12/1e5/7 = 1 million /s |
| :--- | :--- |
| Storage | 1e12\*.1MB = 1e11 mb = 100 PB |

# Service

Evolve

From Single Thread to MultiThread

![](/assets/crawlerNondistribute.png)

![](/assets/crawler2.png)

![](/assets/crawlerFinal.png)

TypeAhead

```
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




# TinyURL

![](/assets/shortUrl.png)



# Scenario

DAU 100M

| Post | 100-300 0.1 per peson | 100 byte, 10MB/S 1G/day |
| :--- | :--- | :--- |
| Get | 1k-3k per person \(1 NoSqr / 3 SQL\) |  |



# Service

![](/assets/URLSVC.png)

算法， 依赖于自增ID

```
class Solution {
public:
    
    Solution()
    {
        srand(NULL);
        
    }
    
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        static string choice("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ");
        static int size = choice.size();
        string res(6,'0');
        do{
            for (int i = 0; i<6; i++)
                res[i] = choice[rand()%size];
        }
        while(tbl_.count(res));
        tbl_[res] = longUrl;
        return res;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return tbl_[shortUrl];
    }
    
    unordered_map<string, string> tbl_;
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

优点： 快，缺点：依赖全局id

随机算法： 优点不依赖全局，缺点，当系统占有量大后，collision 大

```
class Solution {
public:
    
    Solution()
    {
        srand(NULL);
        
    }
    
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        static string choice("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ");
        static int size = choice.size();
        string res(6,'0');
        do{
            for (int i = 0; i<6; i++)
                res[i] = choice[rand()%size];
        }
        while(tbl_.count(res));
        tbl_[res] = longUrl;
        return res;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return tbl_[shortUrl];
    }
    
    unordered_map<string, string> tbl_;
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```




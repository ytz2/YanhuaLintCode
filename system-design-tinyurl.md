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



# 随机的办法也还好：

就是就算要六位就够了，也要放七位，这样减少collision.

第二个就是replica & sharding都给你做好了，这样可以偷懒

![](/assets/randomTinyUrl.png)

# 全局ID:

replica: 用MYSQL的master slave模式

但是sharding就会有问题： 全局不再唯一

解决的办法是多加一位， 生成 按照long url进行sharding, 然后用sharding的instance的全局id， 把sharding encode到第一位。 

这样就解决了这个问题



# Scale 

![](/assets/tinyURLplanA.png)



![](/assets/tinuyURL2.png)


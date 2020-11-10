## 200. Number of Islands

Given an`m x n`2d`grid`map of`'1'`s \(land\) and`'0'`s \(water\), return_the number of islands_.

An**island**is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
 grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]

Output:
 1
```

**Example 2:**

```
Input:
 grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

Output:
 3
```

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty())
            return 0;
        int m = grid.size(), n = grid[0].size();
        queue<pair<int /*x*/, int /*y*/>> q;
        static vector<int> dx{-1, 1, 0, 0};
        static vector<int> dy{0, 0, -1, 1};
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0')
                    continue;
                count++;
                q.emplace(i, j);
                grid[i][j] = '0';
                while(!q.empty()) {
                    auto pos = q.front();
                    q.pop();
                    for (int k = 0; k < 4; k++) {
                        auto nx = pos.first + dx[k];
                        auto ny = pos.second + dy[k];
                        if (nx < 0 || nx >= m || ny < 0 || ny >= n || grid[nx][ny] == '0')
                            continue;
                        grid[nx][ny] = '0';
                        q.emplace(nx, ny);
                    }
                }
            }
        }
        return count;
    }
};

695. Max Area of Island

// 这道题有个变种， 给一堆数字，找出最多的凑一起的一堆， 遍历的时候记得count++ 就好
// T = O(N), S= O(N)

#pragma once
#include <iostream>
#include <queue>
#include <vector>
#include <cassert>
using namespace std;

class Connected {
public:
    Connected() = default;
    ~Connected() = default;
    void test() {
        vector<vector<int>> input{
            {1, 1, 2, 2, 3},
            {1, 1, 0, 3, 3},
            {2, 4, 2, 1, 1},
            {3, 2, 1, 4, 3}
        };
        int exp = 4;
        auto res = connected(input);
        assert(res == exp);
        cout << "Connected pass" << endl;
    }

    int connected(vector<vector<int>>& input) {
        int m = input.size();
        int n = input[0].size();
        int res = 0;
        static vector<int> dx{ -1, 1, 0, 0 };
        static vector<int> dy{ 0, 0, -1, 1 };
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visited[i][j])
                    continue;
                int count = 0;
                queue<pair<int,int>> q;
                q.emplace(i, j);
                int v = input[i][j];
                visited[i][j] = true;
                while (!q.empty()) {
                    count++;
                    int x = q.front().first;
                    int y = q.front().second;
                    q.pop();
                    for (int k = 0; k < 4; k++) {
                        auto nx = x + dx[k];
                        auto ny = y + dy[k];
                        if (nx < 0 || nx >= m || ny < 0 || ny >= n || visited[nx][ny] || input[nx][ny] != v )
                            continue;
                        visited[nx][ny] = true;
                        q.emplace(nx, ny);
                    }
                }
                res = max(res, count);
            }
        }
        return res;
    }
};
```

## 210. Course Schedule II

T = O\(V + E\) , v is number of vertices and E is number of edges

There are a total of`n`courses you have to take labelled from`0`to`n - 1`.

Some courses may have`prerequisites`, for example, if `prerequisites[i] = [ai, bi]` this means you must take the course`bi`before the course`ai`.

Given the total number of courses `numCourses`and a list of the`prerequisite`pairs, return the ordering of courses you should take to finish all courses.

If there are many valid answers, return**any**of them. If it is impossible to finish all courses, return**an empty array**.

**Example 1:**

```
Input:
 numCourses = 2, prerequisites = [[1,0]]

Output:
 [0,1]

Explanation:
 There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input:
 numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]

Output:
 [0,2,1,3]

Explanation:
 There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input:
 numCourses = 1, prerequisites = []

Output:
 [0].
```

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> result;
        unordered_map<int, unordered_set<int>> graph;
        // build graph;
        for (const auto& each : prerequisites) {
            for (int i = 1; i < each.size(); i++) {
                graph[each[i]].insert(each[i - 1]);
            }
            if (!graph.count(each[0]))
                graph[each[0]] = unordered_set<int>();
        }
        for (int i = 0; i < numCourses; i++) {
            if (!graph.count(i))
                graph[i] = unordered_set<int>();
        }
        // indegrees;
        unordered_map<int, int> indegrees;
        for (const auto& node : graph) {
            for (const auto& child : node.second) {
                indegrees[child]++;
            }
        }

        queue<int> q;
        // topsort
        for (const auto& node : graph) {
            if (!indegrees.count(node.first)) {
                q.push(node.first);
            }
        }

        // BFS
        while(!q.empty()) {
            auto v = q.front();
            q.pop();
            result.push_back(v);
            for (const auto& child : graph[v]) {
                if (indegrees.count(child) && --indegrees[child] == 0) {
                    q.push(child);
                    indegrees.erase(child);
                }
            }
        }

        return numCourses == result.size() ? result : vector<int>();
    }
};


#pragma once
#include <queue>
#include <iostream>
#include <vector>
#include <unordered_set>
#include <unordered_map>

using namespace std;

class Dependency {
public:
    Dependency() = default;

    void test() {
        cout << "DD: dependency" << endl;
        vector<vector<int>> inputNoCycle{
            { 1, 3}, {3, 5}, {4}, { 2, 3}
        };
        bool res = hasCycle(inputNoCycle);
        if (res) {
            cout << "expect to have no cycle but found, fail" << endl;
        }
        else {
            cout << "PASS, expect no cycle and get no cycle" << endl;
        }

        vector<vector<int>> inputCycle{
            { 1, 3}, {3, 5}, {4}, { 2, 3}, {2, 1}, {4, 2}, {3, 1}
        };
        res = hasCycle(inputCycle);
        if (res) {
            cout << "expect to have cycle and found cycle, pass" << endl;
        }
        else {
            cout << "expect to have cycle but found no cycle, fail" << endl;
        }
    }


    bool hasCycle(vector<vector<int>>& input) {
        unordered_map<int, unordered_set<int>> graph;
        // build graph,  {0, 1} : 0 -> 1 or 0 depends on 1
        for (const auto& each : input) {
            if (each.size()  == 2) {
                graph[each[1]].insert(each[0]);
            }
            if (!graph.count(each[0]))
                graph[each[0]] = unordered_set<int>();
        }

        // build indegrees
        unordered_map<int, int> indegrees;
        for (const auto& each : graph) {
            for (const auto child : each.second) {
                indegrees[child]++;
            }
        }

        // seed the q for bfs
        queue<int> q;
        unordered_set<int> visited;
        for (const auto& node : graph) {
            if (!indegrees.count(node.first)) {
                q.push(node.first);
            }
        }

        int count = 0;
        while (!q.empty()) {
            auto k = q.front();
            q.pop();
            count++;
            for (const auto& child : graph[k]) {
                if (indegrees.count(child) && --indegrees[child] == 0) {
                    q.push(child);
                    indegrees.erase(child);
                }
            }
        }
        return count != graph.size();
    }
};
```

## 759. Employee Free Time

We are given a list`schedule`of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping`Intervals`, and these intervals are in sorted order.

Return the list of finite intervals representing**common, positive-length free time**forallemployees, also in sorted order.

\(Even though we are representing`Intervals`in the form`[x, y]`, the objects inside are`Intervals`, not lists or arrays. For example,`schedule[0][0].start = 1`,`schedule[0][0].end = 2`, and`schedule[0][0][0]`is not defined\).  Also, we wouldn't include intervals like \[5, 5\] in our answer, as they have zero length.

**Example 1:**

```
Input:
 schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]

Output:
 [[3,4]]

Explanation:
 There are a total of three employees, and all common
free time intervals would be [-inf, 1], [3, 4], [10, inf].
We discard any intervals that contain inf as they aren't finite.
```

**Example 2:**

```
Input:
 schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]

Output:
 [[5,6],[7,9]]
```

[https://leetcode.com/problems/employee-free-time/](https://leetcode.com/problems/employee-free-time/)

```cpp
/*
// Definition for an Interval.
class Interval {
public:
    int start;
    int end;

    Interval() {}

    Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
public:
    vector<Interval> employeeFreeTime(vector<vector<Interval>> schedule) {
        auto sortFunc = [&schedule](const pair<int, int>& l, const pair<int, int>& r) {
            return schedule[l.first][l.second].start  > schedule[r.first][r.second].start;
        };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(sortFunc)> pq(sortFunc);
        for (int i = 0; i < schedule.size(); i++) {
            if (schedule[i].empty()) continue;
            pq.push(make_pair(i, 0));
        }
        vector<Interval> result;
        if (pq.empty())
            return result;
        auto head = pq.top();
        Interval interval = schedule[head.first][head.second];
        while(!pq.empty()) {
            auto pos = pq.top();
            pq.pop();
            const auto& head = schedule[pos.first][pos.second];
            if (head.start > interval.end) {
                result.push_back(Interval(interval.end, head.start));
                interval = head;
            } else if (head.start <= interval.end) {
                interval.end = max(interval.end, head.end);
            }
            if (pos.second != schedule[pos.first].size() - 1) {
                pos.second++;
                pq.push(pos);
            }
        }
        return result;
    }
};//
```

```
#pragma once
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;
class Meeting {
public :
    Meeting() = default;
    ~Meeting() = default;
    void test() {
        vector<vector<int>> input{
            {-4,0}, {0, 2}, {4,21}, {13, 17}, {18, 21}, {32, 39} 
        };

        // begin and end case
        auto res = findSlots(input, -6, 22, 1); /// -6, -4   2, 4   21, 22
        cout << "input:  -6, 22, 1" << endl;
        print(res);
        res = findSlots(input, -3,55, 2); // 2, 4   21 32,      39, 55
        cout << "input: -3,55, 2" << endl;
        print(res);
        res = findSlots(input, -9, 42, 8); // 
        cout << "input: -9, 42, 8" << endl; // 21 32
        print(res);
    }
    void print(vector<vector<int>>& input) {
        for (auto& each : input) {
            cout << each[0] << "-->" << each[1] << endl;
        }
        cout << endl;
    }

    vector<vector<int>> findSlots(vector<vector<int>> input, int from, int to, int minLen) {
        vector<vector<int>> result;
        int last_end = from;
        sort(input.begin(), input.end(), [](const vector<int>& l, const vector<int>& r) {
            return l[0] < r[0];
        });
        for (const auto& each : input) {
            if ( min(to, each[0]) - last_end >= minLen) {
                result.push_back({ last_end, min(each[0], to) });
            }
            last_end = max(last_end, each[1]);
        }
        if (to - last_end >= minLen) {
            result.push_back({last_end, to});
        }
        return result;
    }

    vector<vector<int>>  findSlots2(vector<vector<int>> input, int from, int to, int minLen) {
        vector<vector<int>> result;
        vector<pair<int, bool>> timelines;
        for (const auto& each : input) {
            if (each.size() != 2 || each[0] >= each[1]) // invalid input
                continue;
            if (each[1] < from) continue;
            if (each[0] > to) continue;
            timelines.emplace_back( each[0] < from ? from : each[0] , true);
            timelines.emplace_back( each[1] > to ? to : each[1] , false);
        }
        sort(timelines.begin(), timelines.end(), [](const pair<int, bool>& l, const pair<int, bool>& r) {
            if (l.first == r.first) {
                return l.second;
            }
            return l.first < r.first;
        });

        int count = 0;
        int start = from;
        for (const auto& each : timelines) {
            if (each.second) {
                if (count == 0 && each.first - start >= minLen) {
                    result.push_back({ start, each.first });
                }
                count++;
            }
            else {
                count--;
                if (count == 0) {
                    start = each.first;
                }
            }
        }
        if (to - start  >= minLen) {
            result.push_back({ start, to });
        }
        return result;
    }
};
```

## 1359.Count All Valid Pickup and Delivery Options

Given`n`orders, each order consist in pickup and delivery services.

Count all valid pickup/delivery possible sequences such that delivery\(i\) is always after of pickup\(i\).

Since the answer may be too large, return it modulo 10^9 + 7.

**Example 1:**

```
Input:
 n = 1

Output:
 1

Explanation:
 Unique order (P1, D1), Delivery 1 always is after of Pickup 1.
```

**Example 2:**

```
Input:
 n = 2

Output:
 6

Explanation:
 All possible orders: 
(P1,P2,D1,D2), (P1,P2,D2,D1), (P1,D1,P2,D2), (P2,P1,D1,D2), (P2,P1,D2,D1) and (P2,D2,P1,D1).
This is an invalid order (P1,D2,P2,D1) because Pickup 2 is after of Delivery 2.
```

**Example 3:**

```
Input:
 n = 3

Output:
 90
```

```
class Solution {
public:
    int countOrders(int n) {
        unordered_set<int> picks, drops;
        return helper(picks, drops, n) % 1000000009;
    }

    long helper(unordered_set<int>& picks, unordered_set<int>& drops, int n) {
        if (drops.size() == n)
            return 1;
        long res = 0;
        for (int i = 1; i <= n; i++) {
            if (!picks.count(i)) {
                picks.insert(i);
                res += helper(picks, drops, n);
                picks.erase(i);
            } else if (!drops.count(i)) {
                drops.insert(i);
                res += helper(picks, drops, n);
                drops.erase(i);
            }
        }
        return res;
    } 
};
```

```
#pragma once
#include <assert.h>
#include <iostream>
#include <string>
#include <unordered_set>
#include <vector>

using namespace std;
class Pickup {
public:
    Pickup() = default;

    bool isValid(vector<vector<string>>& input) { // o(n), o(n)
        unordered_set<string> picks;
        unordered_set<string> drops;
        for (const auto& entry : input) {
            const auto& action = entry[0];
            const auto& location = entry[1];
            if (action == "P") {
                if (picks.count(location))
                    return false;
                picks.insert(location);
            }
            else { // drop
                if (drops.count(location) || !picks.count(location))
                    return false;
                drops.insert(location);
            }
        }
        return picks.size() == drops.size();
    }

    // o(2n!)/2^n, permute (2n)!, everytime, we prune and divide by 2, thus it is
    vector<vector<vector<string>>> permute(int n) {
        unordered_set<int> picks, drops;
        vector<vector<vector<string>>> result;
        vector<vector<string>> route;
        helper(n, picks, drops, route, result);
        return result;
    }

    void helper(int n, unordered_set<int>& picks, unordered_set<int>& drops, vector<vector<string>>& route, vector<vector<vector<string>>>& result) {
        if (route.size() == 2 * n) {
            if (picks.size() == drops.size()) {
                result.push_back(route);
            }
            return;
        }
        for (int i = 1; i <= n; i++) {
            string location = to_string(i);
            if (!picks.count(i)) {
                picks.insert(i);
                route.push_back({ "P", to_string(i) });
                helper(n, picks, drops, route, result);
                picks.erase(i);
                route.pop_back();
            }
            else if (!drops.count(i)){
                drops.insert(i);
                route.push_back({ "D", to_string(i) });
                helper(n, picks, drops, route, result);
                drops.erase(i);
                route.pop_back();
            }
        }
    }

    void testPermute() {
        {
            auto res = permute(1);
            assert(res.size() == 1);
        }
        {
            auto res = permute(2);
            assert(res.size() == 6);
        }
        {
            auto res = permute(3);
            assert(res.size() == 90);
        }
        {
            auto res = permute(4);
            assert(res.size() == 2520);
        }
    }

    void testValid() {
        {
            vector<vector<string>> input{
                {"P", "1"}, {"D", "1"}
            };
            assert(isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "1"}, {"D", "1"},
                {"P", "2"}, {"D", "2"},
            };
            assert(isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "2"}, {"D", "2"},
                {"P", "1"}, {"D", "1"},
            };
            assert(isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "2"}, {"D", "2"},
                {"P", "1"}, {"D", "2"},
            };
            assert(!isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "1"}, {"D", "2"},
            };
            assert(!isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "2"}, {"D", "2"},
                {"P", "1"}
            };
            assert(!isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "2"}, {"D", "2"},
                {"D", "1"}
            };
            assert(!isValid(input));
        }
        {
            vector<vector<string>> input{
                {"P", "2"}, {"P", "1"},
                {"D", "1"}, {"D", "2"},
            };
            assert(isValid(input));
        }
    }

    void test() {
        testValid();
        testPermute();
        cout << "DD: pickup pass" << endl;
    }
};
```

## 973 .K Closest Points to OriginGiven`n`orders, each order consist in pickup and delivery services.

We have a list of`points` on the plane.  Find the`K`closest points to the origin`(0, 0)`.

\(Here, the distance between two points on a plane is the Euclidean distance.\)

You may return the answer in any order.  The answer is guaranteed to be unique \(except for the order that it is in.\)

**Example 1:**

```
Input: 
points = 
[[1,3],[-2,2]]
, K = 
1
Output: 
[[-2,2]]
Explanation: 

The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) 
<
 sqrt(10), (-2, 2) is closer to the origin.
We only want the closest K = 1 points from the origin, so the answer is just [[-2,2]].
```

**Example 2:**

```
Input: 
points = 
[[3,3],[5,-1],[-2,4]]
, K = 
2
Output: 
[[3,3],[-2,4]]

(The answer [[-2,4],[3,3]] would also be accepted.)
```

```
#pragma once
#include <algorithm>
#include <assert.h>  
#include <memory>
#include <queue>
#include <string>
#include <time.h>
#include <vector>
#include <unordered_map>
#include <time.h>
#include <thread>
#include <chrono>

using namespace std;

struct Dasher {
    Dasher(int id, int x, int y, int r)
        :id(id), x(x), y(y), rate(r) {}
    int id = 0;
    int x = 0;
    int y = 0;
    int rate = 0;
};

class DasherAPI {
public:
    DasherAPI() = default;
    virtual ~DasherAPI() = default;
    virtual vector<Dasher> getDashers(int x, int y) = 0;
};

class MockAPI : public DasherAPI {
public:
    MockAPI() = default;
    virtual ~MockAPI() = default;
    void Return(const vector<Dasher>& input) {
        dashers_ = input;
    }
    vector<Dasher> getDashers(int x, int y) override {
        return dashers_;
    }
private:
    vector<Dasher> dashers_;
};

// offer complexity o(n) space, o(n log(k)) time comlexity
// offer cache after done
class NearestDasher {
public:
    NearestDasher(DasherAPI* api, int ttl)
        :api_(api), ttl_(ttl)
    {}
    vector<int> findDashers(int x, int y) {
        auto k = key(x, y);
        // cache
        if (cache_.count(k)) {
            auto now = time(NULL);
            if (now - cache_[k].first < ttl_)
                return cache_[k].second;
            cache_.erase(k);
        }
        // local lambda for comparison in priority queue
        auto cmp = [x, y](const Dasher& l, const Dasher& r) {
            int distl = (l.x - x) * (l.x - x) + (l.y - y) * (l.y - y);
            int distr = (r.x - x) * (r.x - x) + (r.y - y) * (r.y - y);
            if (distl == distr) {
                return l.rate > r.rate;
            }
            return distl < distr;
        };
        priority_queue<Dasher, vector<Dasher>, decltype(cmp)> pq(cmp);
        auto dashers = api_->getDashers(x, y);
        for (const auto& dasher : dashers) {
            pq.push(dasher);
            if (pq.size() > 3) {
                pq.pop();
            }
        }
        vector<int> result;
        while (!pq.empty()) {
            result.push_back(pq.top().id);
            pq.pop();
        }
        // reverse as we were maintianing max heap 
        reverse(result.begin(), result.end());
        cache_[k] = make_pair(time(NULL), result);
        return result;
    }

private:
    string key(int x, int y) const {
        return to_string(x) + "," + to_string(y);
    }
private:
    DasherAPI* api_;
    int ttl_;
    unordered_map<string, pair<time_t, vector<int>>> cache_;
};

class TestNearestDasher {
public:
    TestNearestDasher() = default;
    void test(){
        {
            // 1. empty return
            MockAPI emptyDasher;
            NearestDasher emptyTest(&emptyDasher, 0);
            vector<int> exp;
            assert(emptyTest.findDashers(0, 0) == exp);
        }
        {
            // 2. normal case
            MockAPI normalDasher;
            vector<Dasher> dashers;
            dashers.emplace_back(1, 0, 1, 100);
            dashers.emplace_back(2, 2, 0, 0);
            dashers.emplace_back(3, 1, 1, 50);
            dashers.emplace_back(4, 2, 2, 100);
            normalDasher.Return(dashers);
            NearestDasher normalTest(&normalDasher, 0);
            vector<int> exp{ 1, 3, 2 };
            assert(normalTest.findDashers(0, 0) == exp);
        }
        {
            // 3. draw case
            MockAPI drawlDasher;
            vector<Dasher> dashers;
            dashers.emplace_back(1, 0, 1, 100);
            dashers.emplace_back(2, 2, 0, 0);
            dashers.emplace_back(3, 1, 1, 50);
            dashers.emplace_back(4, 0, 2, 100);
            drawlDasher.Return(dashers);
            NearestDasher drawTest(&drawlDasher, 0);
            vector<int> exp{ 1, 3, 4 };
            assert(drawTest.findDashers(0, 0) == exp);
        }

        {
            // 2. normal case
            MockAPI normalDasher;
            vector<Dasher> dashers;
            dashers.emplace_back(1, 0, 1, 100);
            dashers.emplace_back(2, 2, 0, 0);
            dashers.emplace_back(3, 1, 1, 50);
            dashers.emplace_back(4, 2, 2, 100);
            normalDasher.Return(dashers);
            NearestDasher normalTest(&normalDasher, 1);
            vector<int> exp{ 1, 3, 2 };
            assert(normalTest.findDashers(0, 0) == exp);
            dashers.clear();
            normalDasher.Return(dashers);
            assert(normalTest.findDashers(0, 0) == exp);
            this_thread::sleep_for(chrono::seconds(2));
            exp.clear();
            assert(normalTest.findDashers(0, 0) == exp);
        }
    }
};
```

```
#pragma once
#include <iostream>
#include <list>
#include <sstream>
#include <string>
#include <unordered_map>
#include <vector>
#include <cassert>
using namespace std;

class CommonVisitSubSeq {
public:
    CommonVisitSubSeq() = default;
    ~CommonVisitSubSeq() = default;
    void test() {
        cout << "DD: CommonVisitSubSeq" << endl;
        string input = "U1 x t1\nU2 x t2\nU1 y t3\nU1 z t4\nU2 y t5\nU3 x t6\nU1 m t7\nU2 z t8";        
        string expected = "x y z";
        auto res = getSeq(input, 3);
        cerr << "expecting " << expected << " got " << res << endl;
        if (res != expected) {
            cerr << "FAIL" << endl;
        }
        else {
            cerr << "PASS" << endl;
        }
        test2();
    }

    // O(kn), n is entries, k is length
    // space O( uk ) + O(n) u is number of user, k is length, n is number of candidates
    string getSeq(const std::string& input, int k) {
        stringstream ss(input);
        string line;
        unordered_map<string, list<string>> userTokens;
        unordered_map<string, int> counter;
        int maxLen = 0;
        string res;
        char delim = ' ';
        while (getline(ss, line)) {
            stringstream ws(line);
            vector<string> tokens;
            string token;
            while (getline(ws, token, delim)) {
                tokens.push_back(token);
            }
            if (tokens.size() != 3) {
                cerr << "invalid input " << line << endl;
                continue;
            }
            string user = tokens[0];
            string value = tokens[1];
            string ts = tokens[2];
            auto& visited = userTokens[user];
            visited.push_back(value);
            if (visited.size() > (size_t)k)
                visited.pop_front();
            string key;
            for (auto it = visited.begin(); it != visited.end(); it++) {
                key += *it + delim;
            }
            key.pop_back();
            counter[key]++;
            if (visited.size() == k && counter[key] > maxLen) {
                maxLen = counter[key];
                res = key;
            }
        }
        return res;
    }

    string getSeq2(const vector<vector<string>>& input, int k) {
        unordered_map<string, list<string>> webs;
        unordered_map<string, int> counter;
        int maxLen = 0;
        string res;
        for (const auto& entry : input) {
            auto& visited = webs[entry[0]];
            visited.push_back(entry[1]);
            if (visited.size() > k)
                visited.pop_front();
            if (visited.size() == k) {
                string key;
                for (const auto& each : visited) {
                    key +=  each + " ";
                }
                key.pop_back();
                counter[key]++;
                if (counter[key] > maxLen) {
                    maxLen = counter[key];
                    res = key;
                }
            }
        }
        return res;
    }

    /*
      auto comp = [](int x, int y){ return x < y; };
   auto set  = std::set<int,decltype(comp)>( comp );
    */
    void test2() {
        cout << "DD: CommonVisitSubSeq2" << endl;
        string expected = "x y z";
        vector<vector<string>> input{
            {"u1", "x"}, {"u2", "x"}, {"u1", "y"}, {"u1", "z"}, {"u2", "y"}, {"u1", "m"}, {"u2", "z"}, {"u3", "t"}
        };
        auto res = getSeq2(input, 3);
        assert(res == expected);
    }
};
```

## 1197 .Minimum Knight Moves

In an**infinite**chess board with coordinates from`-infinity` to`+infinity`, you have a**knight**at square `[0, 0]`.

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

move everything to first quanrant and leave some buffer to letit jump back

```
class Solution {
public:
    size_t key(const pair<int,int>&p) {
        return p.first*1000 + p.second;
    }
    int minKnightMoves(int x, int y) {
        x = abs(x);
        y = abs(y);
        unordered_set<size_t> visited;
        queue<pair<int, int>> q;
        auto beg = make_pair<int, int>(0, 0);
        q.push(beg);
        visited.insert(key(beg));
        int step = -1;
        static vector<int> dx{-2, -2, -1, -1, 1, 1, 2, 2};
        static vector<int> dy{1,  -1,  2, -2, 2, -2, 1, -1};
        while(!q.empty()) {
            step++;
            int n = q.size();
            for (int i = 0; i < n; i++) {
                auto node = q.front();
                if (node.first == x && node.second == y)
                    return step;
                q.pop();
                for (int j = 0; j < 8; j++) {
                    auto nx = node.first + dx[j];
                    auto ny = node.second + dy[j];
                    auto nextNode = make_pair(nx, ny);
                    auto kstr = key(nextNode);
                    if (!visited.count(kstr) && nx >= -8 && ny >= -8) {
                        visited.insert(kstr);
                        q.push(nextNode);   
                    }

                }
            }

        }
        return 0;
    }
};
```



## 1472.Design Browser History

You have a**browser**of one tab where you start on the`homepage`and you can visit another`url`, get back in the history number of`steps`or move forward in the history number of`steps`.

Implement the`BrowserHistory`class:

* `BrowserHistory(string homepage)`
  Initializes the object with the
  `homepage`
   of the browser.
* `void visit(string url)`
   Visits 
  `url`
  from the current page. It clears up all the forward history.
* `string back(int steps)`
   Move
  `steps`
  back in history. If you can only return
  `x`
  steps in the history and
  `steps `
  `>`
  ` x`
  , you will return only
  `x`
  steps. Return the current
  `url`
   after moving back in history
  **at most**
  `steps`
  .
* `string forward(int steps)`
   Move
  `steps`
  forward in history. If you can only forward
  `x`
  steps in the history and
  `steps `
  `>`
  ` x`
  , you will forward only 
  `x`
  steps. Return the current
  `url`
   after forwarding in history
  **at most**
  `steps`
  .

[https://leetcode.com/problems/design-browser-history/](https://leetcode.com/problems/design-browser-history/)

```
class BrowserHistory {
public:
    BrowserHistory(string homepage) {
        nodes.push_back(homepage);
        it = nodes.begin();
    }
    
    void visit(string url) {
        auto& var = nodes;
        auto n = next(it);
        nodes.insert(n, url);
        ++it;
        if (n != nodes.end())
            nodes.erase(n, nodes.end());
    }
    
    string back(int steps) {
        while(steps && it != nodes.begin()){
            steps--;
            it--;
        }
        return *it;
    }
    
    string forward(int steps) {
        for (int i = 0; i < steps; i++) {
            if (it == prev(nodes.end()))
                break;
            it++;
        }
        return *it;
    }
    
    list<string> nodes;
    list<string>::iterator it;
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```



